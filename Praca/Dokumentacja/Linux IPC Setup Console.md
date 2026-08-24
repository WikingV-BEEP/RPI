# Linux IPC Setup Console

## Źródło
- Repozytorium: https://github.com/mercuriual2302/Linux-IPC-Setup
- Autor: `mercuriual2302`
- Gałąź domyślna: `main`
- Licencja: MIT

## Cel projektu
Aplikacja desktopowa do przygotowania i zarządzania sterownikami Beckhoff CX z Linuxem bez ręcznego używania terminala. Łączy się z urządzeniem przez SSH, instaluje środowisko TwinCAT 3 i pozwala wykonywać dalszą konfigurację z poziomu GUI.

Projekt jest zbudowany w Electronie i był testowany na:
- CX9240 — arm64,
- CX5120 — amd64,
- Debian Trixie RT Linux.

## Najważniejsze funkcje
- automatyczne provisioning sterownika przez SSH,
- zapis danych logowania MyBeckhoff do konfiguracji APT,
- wybór kanału repozytorium Beckhoff,
- instalacja TwinCAT 3 XAR i dodatkowych pakietów,
- inicjalizacja TF2000 HMI Server,
- konfiguracja TF1200 UI Client,
- konfiguracja sieci,
- zarządzanie firewallem,
- zarządzanie użytkownikami i hasłami,
- instalowanie kluczy SSH,
- aktualizacja i odinstalowywanie pakietów,
- zmiana kanału APT,
- podgląd informacji systemowych,
- wykrywanie urządzeń CX w sieci,
- interaktywna powłoka SSH,
- dwupanelowy menedżer plików SFTP,
- obsługa wielu urządzeń w trybie Fleet,
- tworzenie i odtwarzanie konfiguracji za pomocą Recipes.

## Wymagania
### Uruchomienie ze źródeł
- Node.js 18 lub nowszy,
- npm.

### Gotowy plik EXE
Nie wymaga dodatkowych zależności — uruchomienie przez dwuklik.

### Sterownik docelowy
- Beckhoff CX9240, CX5120 lub kompatybilny,
- Debian Trixie RT Linux,
- aktywne SSH na porcie 22,
- aktywne konto `Administrator`,
- konto MyBeckhoff z uprawnieniami do oprogramowania TwinCAT.

## Uruchomienie ze źródeł
```bash
git clone https://github.com/mercuriual2302/Linux-IPC-Setup.git
cd "Linux-IPC-Setup/Linux Setup App (Recommended)/Linux-Setup-App"
npm install
npm start
```

## Budowanie aplikacji Windows
```bash
# Instalator NSIS + wersja portable
npm run dist

# Tylko instalator
npm run build:win

# Tylko wersja portable
npm run build:portable
```

Pliki wynikowe trafiają do katalogu `dist/`.

## Moduły aplikacji
### Overview / Dashboard
Pokazuje:
- hostname,
- uptime,
- kernel,
- wersję TwinCAT,
- aktualny kanał APT,
- zajętość dysku,
- pamięć,
- interfejsy sieciowe,
- status usług.

### Setup
Główny kreator przygotowania sterownika:
- wybór danych MyBeckhoff,
- wybór kanału `trixie-stable` lub `trixie-unstable`,
- wybór pakietów,
- opcjonalne przypięcie konkretnych wersji,
- uruchomienie instalacji.

`trixie-stable` jest zalecany. `trixie-unstable` powinien być używany wyłącznie po zaleceniu wsparcia Beckhoff.

Pakiet `tc31-xar-um` jest wybierany jak zwykły pakiet i nie jest już instalowany automatycznie.

Można także wygenerować samodzielny skrypt `.sh`. Do jego uruchomienia potrzebny jest `sshpass`.

### Recipes
Pozwalają zapisać stan wzorcowego CX do pliku JSON i odtworzyć go na nowym sterowniku.

Zapisywane elementy:
- kanał APT,
- pakiety TwinCAT wraz z wersjami,
- reguły firewalla,
- konfiguracja TF1200.

Ustawienia sieciowe, hostname i AMS NetID są zapisywane tylko informacyjnie i nie są automatycznie kopiowane, aby uniknąć nadania wielu urządzeniom tej samej tożsamości.

### Fleet
Pozwala wykonać operację na wielu CX jednocześnie:
- zastosować Recipe,
- zmienić kanał APT,
- wykonać pełną aktualizację systemu.

Tryby pracy:
- równoległy — kilka urządzeń jednocześnie,
- sekwencyjny — urządzenia obsługiwane po kolei.

Tryb równoległy posiada mechanizm circuit breaker, który może przerwać całą operację po przekroczeniu liczby błędów.

### Services, Network, Firewall, Users, Packages
Narzędzia do bieżącego zarządzania gotowym sterownikiem:
- restart usług,
- zmiana sieci,
- konfiguracja firewalla,
- dodawanie i usuwanie użytkowników,
- aktualizacja pakietów,
- odinstalowanie pakietów z zachowaniem lub usunięciem konfiguracji.

### TF1200 UI Client
Konfiguracja przeglądarki kioskowej wyświetlającej HMI na monitorze podłączonym do CX.

Przed zmianami aplikacja:
- odczytuje aktualną konfigurację,
- wykonuje kopię zapasową z timestampem.

### Shell
Pełna interaktywna sesja SSH. Obsługuje między innymi:
- `vim`,
- `top`,
- `journalctl -f`,
- interaktywne zapytania o hasło sudo.

### Files
Dwupanelowa przeglądarka SFTP:
- komputer lokalny po jednej stronie,
- CX po drugiej,
- upload i download,
- podgląd plików tekstowych,
- tworzenie katalogów,
- kasowanie plików.

SFTP nie obsługuje `sudo`, więc dostępne są wyłącznie pliki, do których zalogowane konto ma uprawnienia.

## Wykrywanie urządzeń
Przycisk Scan może znaleźć CX:
- w tej samej sieci lokalnej — po adresie MAC,
- przy bezpośrednim połączeniu przewodem — po adresie link-local.

Urządzenia są oznaczane jako Linux lub Windows.

## Dostęp sterownika do Internetu
Jeżeli CX nie ma dostępu do repozytorium Beckhoff, aplikacja może tymczasowo przekierować ruch pakietów przez połączenie internetowe laptopa.

Mechanizm działa również w trybie Fleet i sprawdza osobno każde urządzenie.

## Profile połączeń
### Profiles
Przechowują:
- nazwę urządzenia,
- adres IP,
- hasło Administratora.

### MyBeckhoff
Przechowują dane logowania MyBeckhoff niezależnie od konkretnego CX.

Dane są walidowane bezpośrednio z laptopa względem serwera APT Beckhoff.

Dane MyBeckhoff są szyfrowane lokalnie przez Electron `safeStorage`:
- Windows DPAPI,
- macOS Keychain.

Jeżeli system nie ma dostępnego keyringu, aplikacja zapisze dane jawnie i wyświetli ostrzeżenie.

## Bezpieczeństwo danych
- hasło Administratora jest trzymane w pamięci tylko podczas sesji,
- dane MyBeckhoff trafiają na CX do `/etc/apt/auth.conf.d/bhf.conf`,
- profile są zapisywane lokalnie poza repozytorium,
- profile nie są commitowane do GitHub.

Ścieżki profili w Windows:
```text
%APPDATA%\Linux Setup Console\cx-profiles.json
%APPDATA%\Linux Setup Console\mybeckhoff-profiles.json
```

## Struktura repozytorium
```text
Linux Setup App (Recommended)/
  Linux-Setup-App/
    main.js
    preload.js
    src/
      ssh-manager.js
      script-builder.js
      sftp-manager.js
      discovery.js
      socks-proxy.js
      recipe.js
      fleet.js
      reachability.js
    renderer/
      index.html
      renderer.js
      styles.css
sample-scripts/
Linux Setup Guide.docx
```

## Ważne uwagi
- Aplikacja wymusza pojedynczą instancję.
- Domyślne hasło konta `Administrator` na świeżym obrazie CX to `1`.
- Zmiana adresu IP aktywnego interfejsu zerwie bieżącą sesję SSH — jest to normalne.
- Polecenie Shutdown wykonuje pełne wyłączenie ACPI; CX nie uruchomi się automatycznie.
- Do ponownego uruchomienia należy użyć Restart.
- Standardowy obraz CX9240/CX5120 zawiera podsystem SFTP.
- Przy problemach z SFTP należy sprawdzić wpis `Subsystem sftp ...` w `/etc/ssh/sshd_config`.

## Ocena przydatności
Projekt może być użyteczny jako:
- narzędzie serwisowe dla CX z Linuxem,
- sposób na standaryzację wdrożeń TwinCAT/BSD Linux IPC,
- baza do własnego konfiguratora urządzeń,
- przykład użycia Electron + SSH + SFTP + provisioning,
- narzędzie do szybkiego przygotowywania wielu sterowników.

## Do sprawdzenia przed użyciem produkcyjnym
- sposób przechowywania haseł profili CX,
- walidacja komend wykonywanych z uprawnieniami sudo,
- obsługa przerwanej instalacji,
- mechanizm cofania zmian,
- zgodność z używaną wersją obrazu Beckhoff Linux,
- zachowanie aplikacji przy zmianach pakietów w repozytorium Beckhoff,
- wiarygodność i utrzymanie projektu przez autora.