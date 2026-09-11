---
tags:
  - praca
  - twincat
  - raspberry-pi
  - linux
  - mqtt
created: 2026-09-08
tested: 2026-09-07
---
# TwinCAT 3 XAR na Raspberry Pi 3B+ — PREEMPT_RT, Docker i MQTT

Instrukcja odtworzenia konfiguracji uruchomionej 6–7 września 2026. Wynik: **XAR 3.1.4026.28 uruchomił się w CONFIG, lokalne ADS zwróciło 15, kontener był healthy, a broker potwierdził połączenie klienta TwinCAT.** Nie wykonaliśmy programu PLC, pomiarów jittera ani testu EtherCAT. Połączenie Windows XAE opisano jako następny krok; nie zostało zweryfikowane w tej sesji.

To eksperymentalna konfiguracja RPi. Oficjalny przykład Beckhoff zakłada wspierany IPC z Beckhoff RT Linux. Samo powodzenie startu na RPi nie potwierdza pełnej zgodności platformy ani licencji PLC.

## 1. Parametry i zakres odtworzenia

| Element | Wartość użyta w teście |
|---|---|
| Sprzęt | Raspberry Pi 3 Model B Plus Rev 1.3, około 1 GB RAM |
| System | Debian 13.5 ARM64, rozruch Raspberry Pi z `/boot/firmware` |
| Kernel początkowy | `6.18.34+rpt-rpi-v8` |
| Kernel RT | `6.18.39+rpt-rpi-v8-rt` |
| Wersja pakietów kernela | `1:6.18.39-1+rpt1` |
| SSH | `admin@192.168.1.37` (Wi-Fi); `.38` to Ethernet tego samego RPi |
| Projekt | `/home/admin/twincat-test` |
| Docker | już zainstalowany; w czasie testu 29.6.0, Compose v2 |
| XAR | `tc31-xar-um=4026.28.0-1`, ARM64 |
| Obraz/kontener | `twincat-test:4026.28-arm64` / `twincat-test` |
| Broker istniejący | `automation-mosquitto:1883`, dostęp z Windows `192.168.1.37:1883` |
| Sieć Docker istniejąca | `automation_default` |
| Temat ADS-over-MQTT | `AdsOverMqtt-TwinCAT-Test` |
| AMS Net ID XAR | `192.168.1.37.1.1` |
| Klient MQTT XAR | `twincat-rpi-test` |
| Klient MQTT XAE | `twincat-xae-test` |

**Punkt wyjścia:** działający system ARM64, SSH, sudo, Docker z Compose oraz istniejący broker MQTT. Nie instalowaliśmy systemu na karcie ani nowego brokera w ramach tej pracy. Odtwarzając na innym hoście, przygotuj te elementy wcześniej, a następnie dopasuj adresy i nazwę sieci w poniższych plikach.

Wszystkie bloki `bash` wykonuj na RPi jako `admin`, chyba że opis mówi inaczej. Bloki `powershell` wykonuj w Windows. Kod zapisujący pliki jest przeznaczony do nowego katalogu wdrożenia; nie uruchamiaj całej instrukcji ponownie na działającej instalacji.

Komendy i pliki odtwarzają końcową konfigurację. Uporządkowano ich kolejność: w rzeczywistej sesji obraz zbudowano przed zmianą kernela, co ujawniło błąd RT. Skrypt kopii rozruchu poniżej wylicza datę i SHA256 nowej kopii zamiast używać sumy z naszego egzemplarza. Przyszła dostępność przypiętych wersji w repozytoriach nie jest gwarantowana.

## 2. Połączenie i kontrola systemu

```powershell
ssh admin@192.168.1.37
```

W naszej sesji po skonfigurowaniu klucza używaliśmy:

```powershell
ssh -i "$env:USERPROFILE\.ssh\codex_rpi_twincat" admin@192.168.1.37
```

Na nowym komputerze ten klucz nie istnieje — zaloguj się własnym kluczem lub hasłem SSH. Hasła SSH, MQTT i myBeckhoff nie są zapisane w tej notatce.

```bash
uname -a
uname -m
cat /etc/os-release
sudo -n true
docker version
docker compose version
docker ps --format '{{.Names}}: {{.Status}}'
docker network inspect automation_default >/dev/null
docker inspect --format '{{.State.Status}}' automation-mosquitto
df -h / /boot/firmware
free -m
vcgencmd get_throttled
mkdir -p /home/admin/twincat-test
cd /home/admin/twincat-test
```

Oczekiwane `uname -m`: `aarch64`. RPi ma ograniczoną pamięć. U nas zgłaszano `throttled=0x50005`, czyli niedonapięcie i throttling; wymaga to poprawy zasilania przed oceną jakości realtime.

Do kolejnych kroków potrzebne są `python3`, `git`, `curl`, `make`, kompilator C i opcjonalnie `strace`. W razie braków:

```bash
sudo apt-get update
sudo apt-get install -y python3 git curl ca-certificates build-essential strace
```

## 3. Kopia rozruchu PRZED instalacją kernela

Zachowujemy cały FAT boot, konfigurację, nazwę starego kernela i listę kontenerów. Aktualizacja pakietów może zmienić obrazy zwykłego kernela, dlatego sama kopia `config.txt` jest niewystarczająca.

```bash
cd /home/admin/twincat-test
sudo python3 - <<'PY'
import datetime, hashlib, json, os, subprocess
from pathlib import Path
root = Path('/home/admin/twincat-test')
meta = root / 'rt-backup-info.json'
if meta.exists():
    raise SystemExit('Istnieje już opis kopii; nie nadpisuj go.')
stamp = datetime.datetime.now().strftime('%Y%m%d-%H%M%S')
backup = root / ('rt-backup-' + stamp)
backup.mkdir(mode=0o700)
archive = backup / 'boot-firmware.tar'
subprocess.run(['tar', '-C', '/boot/firmware', '-cpf', str(archive), '.'], check=True)
with archive.open('rb') as stream:
    checksum = hashlib.file_digest(stream, 'sha256').hexdigest()
old_kernel = subprocess.check_output(['uname', '-r'], text=True).strip()
for filename, cmd in [('packages-before.txt', ['dpkg-query', '-W']),
                      ('containers-before.txt', ['docker', 'ps', '--format', '{{.Names}}: {{.Status}}'])]:
    (backup / filename).write_text(subprocess.check_output(cmd, text=True))
info = {'backup': str(backup), 'sha256': checksum, 'old_kernel': old_kernel,
        'fallback': 'pre-rt-' + stamp}
meta.write_text(json.dumps(info, indent=2))
os.chmod(meta, 0o600)
os.sync()
print('Kopia:', backup)
print('SHA256:', checksum)
PY
```

Kopia z naszej sesji: `/home/admin/twincat-test/rt-backup-20260906-2328/boot-firmware.tar`, SHA256 `3a727bb76e1a7471b35fdaa1139e177833db90fbf32e38ec5abb1647cb3508d8`. Tej sumy nie używaj do innej kopii.

## 4. Instalacja gotowego kernela Raspberry Pi RT

```bash
sudo apt-get update
apt-cache policy linux-image-rpi-v8-rt
sudo env DEBIAN_FRONTEND=noninteractive apt-get install -y \
  linux-image-rpi-v8-rt=1:6.18.39-1+rpt1
sudo dpkg --audit
grep CONFIG_PREEMPT_RT /boot/config-6.18.39+rpt-rpi-v8-rt
ls -lh /boot/firmware/kernel8_rt.img /boot/firmware/initramfs8_rt
```

W sesji użyto `apt-get install -y linux-image-rpi-v8-rt`, którego kandydatem była powyższa wersja. Tutaj przypięto wersję dla odtwarzalności. Jeśli nie jest dostępna, nie podstawiaj innej bez dostosowania `VERSION` i sprawdzenia artefaktów rozruchowych.

Oczekiwane: `CONFIG_PREEMPT_RT=y`. Pakiety Raspberry Pi i ich hooki przygotowały `kernel8_rt.img` oraz `initramfs8_rt`. Nie zastępowaliśmy ich ogólnym pakietem Debiana `linux-image-rt-arm64`.

## 5. Przygotowanie próbnego startu i starego kernela jako fallback

Poniższy wariant skryptu użytego w sesji bierze nazwę kopii, sumę i stary kernel z kroku 3. Przeznaczony jest dla testowanego sposobu rozruchu **RPi 3B+**. Zachowuje oryginalny `config.txt`, dodaje konfigurację RT oraz wymusza `dwc2` dla USB. Nie przenoś ustawień w ciemno na Pi 4/5.

```bash
cat > prepare_rt_boot_repro.py <<'PY'
"""Prepare a one-shot PREEMPT_RT boot with the previous boot files as fallback."""
import hashlib
import json
import os
from pathlib import Path
import subprocess
import tarfile

BOOT = Path('/boot/firmware')
ROOT = Path('/home/admin/twincat-test')
INFO = json.loads((ROOT / 'rt-backup-info.json').read_text())
BACKUP = Path(INFO['backup'])
ARCHIVE = BACKUP / 'boot-firmware.tar'
FALLBACK = BOOT / INFO['fallback']
VERSION = '6.18.39+rpt-rpi-v8-rt'


def digest(path):
    with path.open('rb') as source:
        return hashlib.file_digest(source, 'sha256').hexdigest()


def write_file(path, text):
    temporary = path.with_name(path.name + '.pending')
    with temporary.open('w', encoding='utf-8', newline='\n') as stream:
        stream.write(text)
        stream.flush()
        os.fsync(stream.fileno())
    temporary.replace(path)


def main():
    if os.geteuid() != 0:
        raise SystemExit('Run with sudo.')
    if digest(ARCHIVE) != INFO['sha256']:
        raise SystemExit('Backup checksum mismatch.')
    if FALLBACK.exists() or (BOOT / 'tryboot.txt').exists():
        raise SystemExit('Fallback or tryboot already exists; inspect before changing.')
    kernel_config = (Path('/boot') / ('config-' + VERSION)).read_text()
    if 'CONFIG_PREEMPT_RT=y\n' not in kernel_config:
        raise SystemExit('Target kernel is not configured for PREEMPT_RT.')
    for generated, installed in [
        (BOOT / 'kernel8_rt.img', Path('/boot') / ('vmlinuz-' + VERSION)),
        (BOOT / 'initramfs8_rt', Path('/boot') / ('initrd.img-' + VERSION)),
    ]:
        if generated.stat().st_size < 1_000_000 or digest(generated) != digest(installed):
            raise SystemExit('Missing or mismatched boot artifact: ' + str(generated))
    if not (Path('/lib/modules') / INFO['old_kernel']).is_dir():
        raise SystemExit('Previous kernel modules missing.')
    with tarfile.open(ARCHIVE) as archive:
        original = archive.extractfile('./config.txt').read().decode('utf-8')
        cmdline = archive.extractfile('./cmdline.txt').read().decode('utf-8').strip()
    if (BOOT / 'config.txt').read_text() != original:
        raise SystemExit('Boot configuration has changed since backup; inspect first.')
    FALLBACK.mkdir()
    subprocess.run(['tar', '-xf', str(ARCHIVE), '-C', str(FALLBACK)], check=True)
    for filename in ['kernel8.img', 'initramfs8', 'bcm2710-rpi-3-b-plus.dtb', 'overlays/README']:
        if not (FALLBACK / filename).is_file():
            raise SystemExit('Incomplete fallback: ' + filename)
    fallback_config = original.rstrip() + '\n\n# Previous verified kernel and boot files\n[all]\nos_prefix={fallback}/\n'.format(fallback=INFO['fallback'])
    rt_config = original.rstrip() + '\n\n# PREEMPT_RT test configuration\n[all]\nos_prefix=\nkernel=kernel8_rt.img\nauto_initramfs=0\ninitramfs initramfs8_rt followkernel\ndtoverlay=dwc2,dr_mode=host\ncmdline=cmdline-rt.txt\n'
    cmdline += ' cgroup_enable=memory cgroup_memory=1\n'
    write_file(BOOT / 'cmdline-rt.txt', cmdline)
    write_file(BOOT / 'tryboot.txt', rt_config)
    write_file(ROOT / 'rt-final-config.txt', rt_config)
    write_file(ROOT / 'rt-fallback-config.txt', fallback_config)
    write_file(BOOT / 'config.txt', fallback_config)
    os.sync()
    print('Verified RT kernel and initramfs; old kernel/modules retained.')
    print('Prepared tryboot.txt for RT and config.txt for original kernel fallback.')


if __name__ == '__main__':
    main()
PY
```

```bash
sudo python3 prepare_rt_boot_repro.py
tail -12 /boot/firmware/tryboot.txt
tail -5 /boot/firmware/config.txt
df -h /boot/firmware
sudo sync
sudo reboot '0 tryboot'
```

`0 tryboot` jest jednym argumentem. Ten restart używa jednorazowo `tryboot.txt`. Normalny `config.txt` wskazuje wtedy zachowany stary zestaw boot poprzez `os_prefix`. Jeżeli próbny start się nie powiedzie, kolejny zwykły rozruch powinien użyć starej konfiguracji. Nie usuwaj starych modułów ani katalogu fallback.

Po powrocie SSH (na naszym obciążonym Pi start Dockera i 14 usług trwał kilka minut):

```bash
uname -a
grep CONFIG_PREEMPT_RT /boot/config-$(uname -r)
cat /sys/fs/cgroup/cgroup.controllers
docker ps --format '{{.Names}}: {{.Status}}'
free -m
```

Potwierdź `PREEMPT_RT` oraz kontroler `memory`. W naszym kernelu `/proc/cgroups` pokazuje tylko kontrolery v1; do tego testu używaj `cgroup.controllers`. Firmware dodawał `cgroup_disable=memory`, a końcowe `cgroup_enable=memory` skutecznie go odwróciło. Zachowany historyczny parametr `cgroup_memory=1` został przez ten kernel zgłoszony jako nieznany.

Po sprawdzeniu sieci i istniejących usług utrwal tę samą konfigurację RT:

```bash
cd /home/admin/twincat-test
sudo cmp rt-final-config.txt /boot/firmware/tryboot.txt
sudo cp rt-final-config.txt /boot/firmware/config.txt.pending
sudo mv /boot/firmware/config.txt.pending /boot/firmware/config.txt
sudo sync
sudo cmp rt-final-config.txt /boot/firmware/config.txt
```

Po tym kroku RT jest domyślny. Samo odłączenie zasilania nie przywraca już starego kernela. W sesji nie wykonywano drugiego restartu po utrwaleniu ustawień.

## 6. Brak `/sys/kernel/realtime`: moduł zgodności

Pomimo aktywnego PREEMPT_RT XAR zgłaszał `Realtime kernel is not active.`. `strace` pokazał `openat(..., "/sys/kernel/realtime", O_RDONLY) = -1 ENOENT`. Kernel RPi nie miał starszego interfejsu statusu używanego przez TwinCAT.

Dodaliśmy mały moduł tworzący rzeczywisty, tylko do odczytu atrybut sysfs. Zwraca `1` tylko po kompilacji dla kernela z `CONFIG_PREEMPT_RT`; nie włącza RT i nie zmienia schedulera. Nie należy zastępować tego zwykłym plikiem z cyfrą 1 na kernelu bez RT.

```bash
sudo apt-get install -y linux-headers-$(uname -r)
cd /home/admin/twincat-test
mkdir -p rt-sysfs-compat
```

```bash
cat > rt-sysfs-compat/rt_sysfs_compat.c <<'EOF_FILE'
// SPDX-License-Identifier: GPL-2.0-only
/* Restore the legacy RT status attribute for consumers such as TwinCAT. */
#include <linux/init.h>
#include <linux/kobject.h>
#include <linux/module.h>
#include <linux/sysfs.h>

#ifndef CONFIG_PREEMPT_RT
#error "This compatibility module must only be built for a PREEMPT_RT kernel"
#endif

static ssize_t realtime_show(struct kobject *kobj,
                            struct kobj_attribute *attr, char *buf)
{
    return sysfs_emit(buf, "1\n");
}

static struct kobj_attribute realtime_attr = __ATTR_RO(realtime);

static int __init rt_sysfs_compat_init(void)
{
    /* An existing attribute makes this fail; never replace a native one. */
    return sysfs_create_file(kernel_kobj, &realtime_attr.attr);
}

static void __exit rt_sysfs_compat_exit(void)
{
    sysfs_remove_file(kernel_kobj, &realtime_attr.attr);
}

module_init(rt_sysfs_compat_init);
module_exit(rt_sysfs_compat_exit);
MODULE_LICENSE("GPL");
MODULE_DESCRIPTION("Legacy /sys/kernel/realtime attribute for PREEMPT_RT");
MODULE_VERSION("1.0");
EOF_FILE
```

```bash
cat > rt-sysfs-compat/Makefile <<'EOF_FILE'
obj-m := rt_sysfs_compat.o
EOF_FILE
```

Poniższy test ładowania/usuwania wykonaj przed uruchomieniem XAR. Jeśli kernel ma już natywny `/sys/kernel/realtime` z wartością 1, moduł nie jest potrzebny.

```bash
cd /home/admin/twincat-test/rt-sysfs-compat
make -C /lib/modules/$(uname -r)/build M="$PWD" W=1 modules
sudo modinfo ./rt_sysfs_compat.ko
sudo insmod ./rt_sysfs_compat.ko
cat /sys/kernel/realtime
ls -l /sys/kernel/realtime
sudo rmmod rt_sysfs_compat
test ! -e /sys/kernel/realtime
sudo insmod ./rt_sysfs_compat.ko
test "$(cat /sys/kernel/realtime)" = 1
```

Instalacja oraz ładowanie przy starcie:

```bash
sudo install -D -m 0644 rt_sysfs_compat.ko \
  /lib/modules/$(uname -r)/updates/rt_sysfs_compat.ko
sudo depmod -a
printf '%s\n' rt_sysfs_compat | sudo tee /etc/modules-load.d/rt-sysfs-compat.conf
sudo modprobe rt_sysfs_compat
sudo modinfo -n rt_sysfs_compat
```

To moduł spoza drzewa kernela. Normalna kontrola wersji jest zachowana; nie używaj wymuszonego ładowania do innego kernela. Konfigurację autostartu zapisaliśmy, ale nie testowaliśmy jej kolejnym restartem. Nie instalowaliśmy DKMS.

## 7. Źródła przykładu Beckhoff

```bash
cd /home/admin/twincat-test
git clone https://github.com/Beckhoff/TC_XAR_Container_Sample.git upstream
git -C upstream checkout --detach 365e1224c1725399360659900ff8bd5af99162af
git -C upstream rev-parse HEAD > UPSTREAM_COMMIT
```

U nas pliki pobrano z tej rewizji po problemie z klonowaniem na dysku Google Drive. Powyższe klonowanie na lokalnym systemie plików RPi daje ten sam zestaw plików. Nie uruchamiaj `upstream/docker-compose.yaml`, bo zawiera własny broker — korzystamy z istniejącego MQTT.

W użytej rewizji `apt-config/bhf.list` zawiera:

```text
deb [signed-by=/usr/share/keyrings/bhf.asc] https://deb.beckhoff.com/debian trixie-stable main
```

Natomiast `apt-config/debian.sources.list`:

```text
deb https://deb-mirror.beckhoff.com/debian trixie main
deb https://deb-mirror.beckhoff.com/debian-security trixie-security main
```

## 8. Dostęp do pakietów myBeckhoff

W sesji istniał już poprawny plik `/etc/apt/auth.conf.d/bhf.conf`. Docker przekazuje go jako sekret BuildKit; nie kopiujemy go do kontekstu budowania ani obrazu.

Jeśli odtwarzasz na nowym RPi bez tego pliku, utwórz go poniższą komendą. Pyta o dane bez umieszczania hasła w historii powłoki i odmawia nadpisania istniejącego pliku:

```bash
sudo python3 - <<'PY'
from pathlib import Path
import getpass, os
path = Path('/etc/apt/auth.conf.d/bhf.conf')
if path.exists():
    raise SystemExit('Plik już istnieje — zachowano go.')
login = getpass.getpass('Login myBeckhoff (e-mail): ')
password = getpass.getpass('Hasło myBeckhoff: ')
def quote(value):
    if '\n' in value or '\r' in value:
        raise ValueError('Niedozwolony znak nowej linii')
    return '"' + value.replace('\\', '\\\\').replace('"', '\\"') + '"'
path.parent.mkdir(parents=True, exist_ok=True)
fd = os.open(path, os.O_WRONLY | os.O_CREAT | os.O_EXCL, 0o600)
with os.fdopen(fd, 'w') as stream:
    stream.write('machine deb.beckhoff.com\nlogin ' + quote(login) + '\npassword ' + quote(password) + '\n')
print('Zapisano chroniony plik uwierzytelnienia.')
PY
```

To pomocnicza procedura dla nowej instalacji; w naszym teście wykorzystano wcześniejsze dane. Konto musi mieć dostęp do wymaganych pakietów.

## 9. Dockerfile i logger wewnątrz kontenera

W katalogu `/home/admin/twincat-test` zapisz dokładnie następujące pliki. Logger BusyBox przechwytuje syslog runtime'u do `docker logs`. Nie montujemy dziennika hosta do kontenera.

```bash
cat > Dockerfile <<'EOF_FILE'
FROM debian:trixie-slim
RUN apt-get update && apt-get install --yes --no-install-recommends curl ca-certificates \
    && rm -rf /var/lib/apt/lists/*
RUN --mount=type=secret,id=apt,required=true \
    curl --fail --netrc-file /run/secrets/apt -o /usr/share/keyrings/bhf.asc https://deb.beckhoff.com/repo.pub
RUN rm /etc/apt/sources.list.d/debian.sources
COPY upstream/tc31-xar-base/apt-config/bhf.list upstream/tc31-xar-base/apt-config/debian.sources.list /etc/apt/sources.list.d/
RUN --mount=type=secret,id=apt,required=true \
    apt-get -o Dir::Etc::netrc=/run/secrets/apt update \
    && apt-get -o Dir::Etc::netrc=/run/secrets/apt install --yes --no-install-recommends tc31-xar-um=4026.28.0-1 adstool \
    && rm -rf /var/lib/apt/lists/*
COPY upstream/tc31-xar-base/entrypoint.sh /app/entrypoint.sh
WORKDIR /app
CMD ["/bin/sh", "/app/entrypoint.sh"]

RUN --mount=type=secret,id=apt,required=true \
    apt-get -o Dir::Etc::netrc=/run/secrets/apt update \
    && apt-get -o Dir::Etc::netrc=/run/secrets/apt install --yes --no-install-recommends busybox \
    && rm -rf /var/lib/apt/lists/*
COPY logging-entrypoint.sh /app/entrypoint.sh
EOF_FILE
```

```bash
cat > logging-entrypoint.sh <<'EOF_FILE'
#!/bin/sh
set -e
/bin/busybox syslogd -n -O /dev/stdout &
exec /usr/bin/TcSystemServiceUm -f 0x7 -i "${AMS_NETID}" -p /var/run/TcSystemServiceUm.pid
EOF_FILE
```

```bash
cat > .dockerignore <<'EOF_FILE'
secrets
runtime
.env
*.log
*.py
README.md
upstream/README.md
upstream/tc31-xar-base/apt-config/bhf.conf
EOF_FILE
```

Dla kolejności z tej instrukcji (backup przed budowaniem obrazu) rozszerz wykluczenia, aby kopie boot i diagnostyka nie trafiały do kontekstu Docker:

```bash
cat >> .dockerignore <<'EOF_IGNORE'
rt-backup*
rt-final-config.txt
rt-fallback-config.txt
rt-sysfs-compat
*.inspect
*.md
*.ps1
upstream/.git
EOF_IGNORE
```

Budowanie ARM64 bezpośrednio na RPi:

```bash
cd /home/admin/twincat-test
sudo docker build --platform linux/arm64 \
  --secret id=apt,src=/etc/apt/auth.conf.d/bhf.conf \
  -t twincat-test:4026.28-arm64 .
docker image inspect --format '{{.Architecture}}' twincat-test:4026.28-arm64
```

Oczekiwane: `arm64`. Komunikat o niedostępnej wersji `tc31-xar-um` wymaga sprawdzenia repozytorium; nie oznacza problemu CPU. Hasło nie może trafić do `ARG`, `ENV` ani `COPY` w Dockerfile.

## 10. Pierwsza konfiguracja runtime'u i tras MQTT

Skrypt użyty u nas pobiera istniejące `MQTT_USERNAME` i `MQTT_PASSWORD` z kontenera `tuya-mqtt-bridge`, nie wyświetlając ich. To zależność od naszego środowiska: broker musi już działać i akceptować te dane. Jeżeli nie masz tego bridge'a, zamień fragment pobierający `info/env/username/password` na odczyt własnego użytkownika i hasła przez `getpass`; pozostała logika zostaje taka sama.

Skrypt kopiuje kompletną domyślną konfigurację z obrazu, ustawia CONFIG i tworzy dwie trasy: dla runtime'u i Windows XAE. Jest jednorazowy — odmawia działania, jeśli `runtime/` już istnieje.

```bash
cat > configure_runtime.py <<'PY'
"""Run on the Pi after building. Credentials stay on the Pi, outside the image."""
import json
import os
from pathlib import Path
import subprocess
import xml.etree.ElementTree as ET

ROOT = Path(__file__).resolve().parent
IMAGE = 'twincat-test:4026.28-arm64'
TOPIC = 'AdsOverMqtt-TwinCAT-Test'


def docker(*args):
    return subprocess.check_output(['docker', *args], text=True).strip()


def write_route(path, host, client, username, password):
    tree = ET.Element('TcConfig')
    mqtt = ET.SubElement(ET.SubElement(tree, 'RemoteConnections'), 'Mqtt', ClientId=client)
    ET.SubElement(mqtt, 'Address', Port='1883').text = host
    ET.SubElement(mqtt, 'Topic').text = TOPIC
    ET.SubElement(mqtt, 'User').text = username
    ET.SubElement(mqtt, 'Pwd').text = password
    ET.indent(tree)
    ET.ElementTree(tree).write(path, encoding='utf-8', xml_declaration=True)
    path.chmod(0o600)


def main():
    os.umask(0o077)
    info = json.loads(docker('inspect', 'tuya-mqtt-bridge'))[0]
    env = dict(v.split('=', 1) for v in info['Config']['Env'] if '=' in v)
    username, password = env.get('MQTT_USERNAME'), env.get('MQTT_PASSWORD')
    if not username or not password:
        raise SystemExit('Existing MQTT credentials not found in bridge environment; no configuration written.')
    runtime = ROOT / 'runtime'
    if runtime.exists():
        raise SystemExit('Runtime directory exists; preserve it and inspect before reconfiguration.')
    runtime.mkdir(mode=0o700)
    seed = docker('create', IMAGE)
    try:
        subprocess.run(['docker', 'cp', seed + ':/etc/TwinCAT/3.1/.', str(runtime)], check=True)
    finally:
        docker('rm', seed)
    target = runtime / 'Target'
    target.mkdir(exist_ok=True)
    write_route(target / 'StaticRoutes.xml', 'automation-mosquitto', 'twincat-rpi-test', username, password)
    regdir = target / 'RegFiles'
    regdir.mkdir(exist_ok=True)
    sample = ROOT / 'upstream/tc31-xar-base/TwinCAT/SysStartupState.reg'
    data = sample.read_bytes()
    encoding = 'utf-16' if data.startswith((b'\xff\xfe', b'\xfe\xff')) else 'utf-8'
    reg = data.decode(encoding)
    reg = reg.replace('"SysStartupState"=dword:00000005', '"SysStartupState"=dword:0000000f')
    (regdir / 'SysStartupState.reg').write_text(reg, encoding=encoding)
    secrets = ROOT / 'secrets'
    secrets.mkdir(mode=0o700, exist_ok=True)
    write_route(secrets / 'mqtt-xae.xml', '192.168.1.37', 'twincat-xae-test', username, password)
    print('Runtime configured in CONFIG mode; existing MQTT credentials stored only in restricted files on Pi.')
    print('MQTT broker: automation-mosquitto:1883; topic: ' + TOPIC)


if __name__ == '__main__':
    main()
PY
```

```bash
cd /home/admin/twincat-test
sudo python3 configure_runtime.py
sudo test -s runtime/Target/StaticRoutes.xml
sudo test -s secrets/mqtt-xae.xml
```

Katalogi `runtime/` i `secrets/` oraz XML z hasłem pozostają chronione na RPi. Nie wklejaj ich treści do notatek, repozytorium lub logów.

## 11. Compose — istniejący broker, limity i CONFIG

```bash
cat > compose.yaml <<'EOF_FILE'
name: twincat-test
services:
  runtime:
    image: twincat-test:4026.28-arm64
    platform: linux/arm64
    container_name: twincat-test
    hostname: twincat-test
    restart: "on-failure:3"
    init: true
    mem_limit: 256m
    memswap_limit: 384m
    cpus: 1.0
    pids_limit: 160
    cap_add:
      - SYS_NICE
      - IPC_LOCK
    ulimits:
      memlock:
        soft: 67108864
        hard: 67108864
    environment:
      AMS_NETID: "192.168.1.37.1.1"
      PCI_DEVICES: "NONE"
    volumes:
      - ./runtime:/etc/TwinCAT/3.1
    networks:
      - mqtt
    healthcheck:
      test: ["CMD", "tcadstool", "127.0.0.1.1.1", "state"]
      start_period: 40s
      interval: 30s
      timeout: 10s
      retries: 3
    logging:
      driver: json-file
      options:
        max-size: "5m"
        max-file: "2"
networks:
  mqtt:
    external: true
    name: automation_default
EOF_FILE
```

```bash
cd /home/admin/twincat-test
docker compose config --quiet
docker compose up -d
docker exec twincat-test cat /sys/kernel/realtime
docker exec twincat-test tcadstool 127.0.0.1.1.1 state
docker compose ps
docker logs --tail 60 twincat-test
```

Oczekiwane: `1`, następnie `15` (CONFIG), a po kontroli zdrowia `healthy`. `127.0.0.1.1.1` w komendzie to lokalny AMS Net ID dla zapytania wewnątrz kontenera; z Windows wybieramy `192.168.1.37.1.1`.

Uruchomienie w CONFIG jest zamierzone. Nie ma tu aktywowanego projektu PLC. `PCI_DEVICES=NONE`, brak `libtcrte`, brak przekazania karty sieciowej i brak deklaracji EtherCAT.

## 12. Weryfikacja działania i pamięci

```bash
docker inspect --format '{{.State.Status}} {{.State.Health.Status}} OOM={{.State.OOMKilled}}' twincat-test
docker logs --since 10m automation-mosquitto 2>&1 | grep twincat-rpi-test
docker exec twincat-test cat /sys/fs/cgroup/memory.max /sys/fs/cgroup/memory.swap.max
docker stats --no-stream twincat-test
docker ps --format '{{.Names}}: {{.Status}}'
```

Wyniki z sesji:

- `running healthy OOM=false`.
- `TwinCAT system start completed. AdsState: >15<`.
- Broker: nowe połączenie klienta `twincat-rpi-test`.
- `memory.max=268435456`, `memory.swap.max=134217728` — 256 MiB RAM i do 128 MiB swap (Compose `memswap_limit: 384m` to suma).
- Około 119 MiB RAM runtime'u; 14 wcześniejszych kontenerów pozostało uruchomionych.

Jeżeli kontener utworzono przed włączeniem memory cgroups, stare ustawienia mogą mieć `Memory=0`. Odtwórz wyłącznie testowy kontener:

```bash
cd /home/admin/twincat-test
docker compose stop
docker compose create --force-recreate
docker inspect --format 'memory={{.HostConfig.Memory}} swap={{.HostConfig.MemorySwap}}' twincat-test
docker compose up -d
```

Dodatkowy test istniejącego brokera (uruchamiany wewnątrz naszego bridge'a, który zawiera Paho MQTT 2.x i zmienne z danymi logowania):

```bash
cat > verify_mqtt.py <<'PY'
"""Run inside the existing bridge; credentials remain in its environment."""
import os
import threading
import uuid

import paho.mqtt.client as mqtt

topic = 'codex/test/twincat-rt/' + uuid.uuid4().hex
payload = b'PREEMPT_RT MQTT connectivity test'
received = threading.Event()
client = mqtt.Client(mqtt.CallbackAPIVersion.VERSION2,
                     client_id='codex-rt-check-' + uuid.uuid4().hex[:10])
client.username_pw_set(os.environ['MQTT_USERNAME'], os.environ['MQTT_PASSWORD'])

def on_connect(client, userdata, flags, reason_code, properties):
    if reason_code == 0:
        client.subscribe(topic, qos=1)

def on_subscribe(client, userdata, mid, reason_codes, properties):
    if reason_codes and all(code.value < 128 for code in reason_codes):
        client.publish(topic, payload, qos=1, retain=False)

def on_message(client, userdata, message):
    if message.topic == topic and message.payload == payload:
        received.set()

client.on_connect = on_connect
client.on_subscribe = on_subscribe
client.on_message = on_message
client.connect(os.environ.get('MQTT_HOST', '127.0.0.1'),
               int(os.environ.get('MQTT_PORT', '1883')), 30)
client.loop_start()
ok = received.wait(25)
client.unsubscribe(topic)
client.disconnect()
client.loop_stop()
print('MQTT authenticated publish/subscribe: ' + ('PASS' if ok else 'FAIL'))
raise SystemExit(0 if ok else 1)
PY
```

```bash
docker exec -i tuya-mqtt-bridge python - < /home/admin/twincat-test/verify_mqtt.py
```

Wynik: `MQTT authenticated publish/subscribe: PASS`. To test brokera na losowym temacie bez retain. Osobny wpis `twincat-rpi-test` w logu brokera potwierdza połączenie XAR; sam test Paho nie dowodzi działania ADS-over-MQTT pomiędzy XAE a XAR.

## 13. Połączenie z Windows TwinCAT XAE

Wymagany TwinCAT 3 Engineering na Windows i dostęp sieciowy do brokera. Plik trasy został przygotowany na RPi: `/home/admin/twincat-test/secrets/mqtt-xae.xml`.

Pobierz do lokalnego `C:\Temp` w PowerShell (własny klucz SSH, jeśli nie korzystasz z klucza sesji):

```powershell
New-Item -ItemType Directory -Force -Path C:\Temp | Out-Null
$route = & ssh -i "$env:USERPROFILE\.ssh\codex_rpi_twincat" admin@192.168.1.37 "sudo cat /home/admin/twincat-test/secrets/mqtt-xae.xml"
if ($LASTEXITCODE -ne 0) { throw 'Nie udało się pobrać trasy' }
$xmlText = $route -join "`n"
$null = [xml]$xmlText
[IO.File]::WriteAllText('C:\Temp\mqtt-xae.xml', $xmlText, [Text.UTF8Encoding]::new($false))
```

W sesji taki plik zapisaliśmy i sprawdziliśmy jego XML. Nie zamieszczamy jego hasła w notatce.

Skopiuj plik do katalogu routera Twojej instalacji:

- Zwykle `C:\Program Files (x86)\Beckhoff\TwinCAT\3.1\Target\Routes\`.
- Starsza instalacja: `C:\TwinCAT\3.1\Target\Routes\`.
- Windows Usermode Runtime (domyślnie): `%ProgramData%\Beckhoff\TwinCAT\3.1\Runtimes\UmRT_Default\3.1\Target\Routes\`.

Wybierz właściwy katalog dla używanego routera. Następnie z ikony TwinCAT przejdź do **Config**, aby router wczytał XML (przerywa to ewentualny lokalny program). W XAE otwórz **Choose Target System** i wybierz AMS Net ID **192.168.1.37.1.1**. RPi i Windows muszą mieć różne AMS Net ID; klient MQTT `twincat-xae-test` również nie może być równocześnie używany na kilku stacjach.

Jeśli celu nie ma, sprawdź dostęp do portu:

```powershell
Test-NetConnection 192.168.1.37 -Port 1883
```

Oraz połączenie stacji w logu brokera:

```bash
docker logs --since 10m automation-mosquitto 2>&1 | grep -E 'twincat-xae-test|twincat-rpi-test'
```

Nie konfigurowaliśmy bezpośredniego publikowania portów ADS na hoście; w tym wariancie drogą do kontenera jest MQTT. Połączenie XAE, pobranie konfiguracji i uruchomienie PLC to osobne kolejne testy. Nie potwierdzono jeszcze licencji konkretnego programu ani docelowego PLC ARM64.

## 14. Obsługa, aktualizacja i wycofanie

```bash
cd /home/admin/twincat-test
docker compose stop
docker compose start
docker compose logs --tail 80
```

Usunięcie samego kontenera z zachowaniem katalogu runtime:

```bash
docker compose down
```

Polityka `on-failure:3` nie gwarantuje automatycznego startu po restarcie hosta. Po restarcie sprawdź moduł i uruchom `docker compose up -d`. Moduł ma autoload; kontener zachowuje ustawienia testowe.

### Po aktualizacji kernela RT

Moduł musi być zbudowany dla aktualnie uruchomionego kernela. Po przełączeniu na nowy RT:

```bash
uname -r
grep CONFIG_PREEMPT_RT /boot/config-$(uname -r)
sudo apt-get install -y linux-headers-$(uname -r)
cd /home/admin/twincat-test/rt-sysfs-compat
make -C /lib/modules/$(uname -r)/build M="$PWD" clean
make -C /lib/modules/$(uname -r)/build M="$PWD" W=1 modules
sudo install -D -m 0644 rt_sysfs_compat.ko /lib/modules/$(uname -r)/updates/rt_sysfs_compat.ko
sudo depmod -a
sudo modprobe rt_sysfs_compat
cat /sys/kernel/realtime
cd ..
docker compose up -d
```

Jeśli nowy kernel ma już natywny wpis `realtime=1`, pomiń budowanie/ładowanie i usuń konfigurację autoload własnego modułu.

### Usunięcie modułu bez zmiany kernela

```bash
cd /home/admin/twincat-test
docker compose stop
sudo rm /etc/modules-load.d/rt-sysfs-compat.conf
sudo modprobe -r rt_sysfs_compat
sudo rm /lib/modules/$(uname -r)/updates/rt_sysfs_compat.ko
sudo depmod -a
```

### Powrót do starego kernela

```bash
cd /home/admin/twincat-test
docker compose stop
sudo rm -f /etc/modules-load.d/rt-sysfs-compat.conf
sudo cp rt-fallback-config.txt /boot/firmware/config.txt
sudo sync
sudo reboot
```

W razie braku sieci można na karcie SD podmienić `config.txt` na zachowany `rt-fallback-config.txt`; katalog ze starymi plikami boot musi pozostać kompletny. Ta ścieżka wycofania była przygotowana, ale nie wykonywaliśmy powrotnego rozruchu w teście.

## 15. Napotkane problemy i ich rozwiązania

| Objaw | Przyczyna / działanie |
|---|---|
| `Realtime kernel is not active` na starym kernelu | Instalacja i rozruch PREEMPT_RT |
| Ten sam błąd mimo `PREEMPT_RT` w uname | Brak `/sys/kernel/realtime`; moduł zgodności z rozdziału 6 |
| ADS error 1864 przed poprawką | Router nie został zainicjalizowany; sam proces `running` nie oznacza poprawnego startu |
| Brak limitu pamięci | Włączyć memory cgroups i odtworzyć testowy kontener |
| `modinfo: command not found` jako admin | Użyć `sudo modinfo` — narzędzie było poza PATH użytkownika |
| Brak użytecznych logów XAR | Logger BusyBox wewnątrz kontenera |
| RTE driver not found, USB/device tree warnings | W naszym teście start kontynuował się do CONFIG; te funkcje nie były skonfigurowane |
| Wolny start usług po restarcie | Odczekać na Docker/usługi, sprawdzić RAM oraz zasilanie; nie uznawać samego timeoutu SSH za nieudany boot |

Opcjonalnie: dokładny sposób diagnozy brakującego pliku (zatrzymaj XAR, aby nie uruchamiać dwóch instancji na tej samej konfiguracji):

```bash
cd /home/admin/twincat-test
docker compose stop
docker compose run --rm --no-deps --name twincat-rt-diagnostic \
  -v /usr/bin/strace:/usr/local/bin/strace:ro \
  --entrypoint /usr/bin/timeout runtime 20 \
  /usr/local/bin/strace -f -e trace=openat,uname,sched_setscheduler \
  -o /etc/TwinCAT/3.1/rt-strace.log \
  /usr/bin/TcSystemServiceUm -f 0x7 -i 192.168.1.37.1.1 -p /tmp/TcSystemServiceUm.pid
sudo grep realtime runtime/rt-strace.log
docker compose up -d
```

W naszym środowisku binarny `strace` hosta działał z bibliotekami obrazu; przy innej dystrybucji może wymagać instalacji wewnątrz obrazu diagnostycznego. Timeout kończy się kodem niezerowym — jest to oczekiwane dla ograniczonego czasowo testu.

## Źródła i pliki tej instalacji

- [Oficjalny przykład Beckhoff](https://github.com/Beckhoff/TC_XAR_Container_Sample), rewizja `365e1224c1725399360659900ff8bd5af99162af`.
- [Konfiguracja ADS-over-MQTT i ścieżki routera](https://infosys.beckhoff.com/content/1033/tc3_ads_over_mqtt/17769091979.html).
- [Dokumentacja startu Raspberry Pi — tryboot](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html).
- [config.txt, os_prefix, kernel i initramfs](https://www.raspberrypi.com/documentation/computers/config_txt.html).
- [Wątek Raspberry Pi o gotowym kernelu RT](https://forums.raspberrypi.com/viewtopic.php?t=388298).
- [Oryginalna poprawka statusu /sys/kernel/realtime](https://www.osadl.org/monitoring/patches/r0s8/sysfs__Add__sys_kernel_realtime_entry.patch.html).
- [Dokumentacja budowania modułów zewnętrznych](https://docs.kernel.org/kbuild/modules.html).

Kopia robocza projektu Windows: `G:\Mój dysk\Raspberr PI Home\twincat-test`. Projekt i pełne logi RPi: `/home/admin/twincat-test`. Opis końcowych wyników: `STATUS.md`. Sekrety istnieją wyłącznie w chronionych plikach wdrożenia i w lokalnej trasie Windows — nie są częścią tej instrukcji.
