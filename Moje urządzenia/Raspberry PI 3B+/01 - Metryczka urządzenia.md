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
