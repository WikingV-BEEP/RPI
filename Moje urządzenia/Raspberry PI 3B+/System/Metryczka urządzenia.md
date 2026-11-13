---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - rpi
  - system
updated: 2026-08-24
---

# Metryczka urządzenia

## Identyfikacja

| Pole | Wartość |
| --- | --- |
| Urządzenie | Raspberry PI 3B+ |
| Hostname | `RaspberryPI3bp` |
| Klasa sprzętu | Raspberry Pi 3B+ / klasa 3B+ według wcześniejszego audytu |
| System | Debian 13 `arm64` |
| Model pracy | Debian jako baza + Docker na usługi automatyki |
| Panel administracyjny | Cockpit na porcie `9090` |
| Główne zastosowanie | Automatyka domowa, MQTT, Tuya, MCP, Obsidian/Codex |

## Do odświeżenia

- Dokładny model z `/proc/device-tree/model`.
- Dokładna wersja kernela.
- Aktualny uptime.
- Aktualna temperatura i throttling.
- Aktualna lista adresów IP.
- Aktualny stan dysku i partycji.

## Zasoby

Ostatni potwierdzony odczyt z 2026-07-20:

| Zasób | Stan |
| --- | --- |
| RAM | około `905 MiB` dostępnej pamięci |
| Użycie RAM | około `478 MiB` użyte |
| Pamięć dostępna przez cache | około `426 MiB` |
| Swap | około `506 MiB` już użyte |
| Swap systemowy | `zram`, widoczny jako `dev-zram0.swap` |

## Wniosek

To urządzenie ma mały zapas RAM. Home Assistant Container jest możliwy, ale wymaga ostrożnego uruchamiania i monitorowania. Kod `Exited (137)` przy Home Assistancie sugeruje problem z pamięcią albo zakończenie procesu pod presją zasobów.

## Zasada pracy na tym Pi

- Najpierw backup.
- Potem jedna zmiana naraz.
- Po zmianie sprawdzić kontenery, logi, RAM i swap.
- Nie usuwać Cockpita, dopóki Home Assistant i SSH nie pokrywają realnie potrzeb administracyjnych.

## Stan historyczny przed migracją

- Ostatni potwierdzony odczyt systemu i usług: 2026-07-20.
- Ostatni potwierdzony odczyt MQTT/MCP/Tuya: 2026-07-16.
- Próby świeżego odczytu 2026-08-24 nie zwróciły raportu z workera, więc część parametrów ma status `do odświeżenia`.
- W tej sekcji są świadomie zapisane jawne sekrety dostępu do RPi i OAuth MCP. Przy kolejnej porządkowej rundzie najlepiej przenieść je do managera haseł.

### Konfiguracja przed migracją

- Bazą jest Debian + Docker, a nie Home Assistant OS.
- Cockpit pełni rolę panelu administracji systemem i nie powinien być usuwany przed stabilną migracją do Home Assistanta.
- Mosquitto jest centralnym brokerem MQTT.
- OAuth MCP przechodzi przez auth proxy i Caddy; poprawny publiczny MQTT MCP endpoint to `https://wikingv.servehalflife.com/mqtt-mcp/mcp`.
- `tuya-mqtt-bridge` obsługuje lokalne gniazdka Tuya przez LAN.
- `mqtt-mcp-connector` wystawia kontrolę MQTT/MCP, ale komendy sterujące należy wykonywać ostrożnie.
- Home Assistant był zatrzymany z kodem `Exited (137)`, więc problem RAM/swap jest najważniejszym ryzykiem.

### Migracja na Windows 10 — aktualizacja 20.10.2026

Opis powyżej dokumentuje wcześniejszy stos RPi. Przeniesione usługi Home Server działają według zapisu migracji na mini PC Windows 10 / Docker Desktop (WSL 2). Aktualny zakres i wyjątki: [[Moje urządzenia/Win 10 Home Automation/Migracja RPi na Windows 10]]. Zobacz [[Moje urządzenia/Win 10 Home Automation/Home Server MCP Gateway|katalog narzędzi MCP]] i [[Moje urządzenia/Win 10 Home Automation/Testy MCP 2026-10-20|wyniki testów]]. Nie traktuj dawnych portów i statusów kontenerów RPi jako bieżącego audytu Windows.
