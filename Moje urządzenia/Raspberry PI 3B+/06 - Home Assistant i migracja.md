---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - home-assistant
  - migracja
updated: 2026-08-24
---

# Home Assistant i migracja

## Aktualny sens architektury

Home Assistant powinien być traktowany jako dashboard i silnik automatyzacji, a nie jako zamiennik Cockpita. Cockpit zostaje do administracji systemem, a Home Assistant może stopniowo przejąć panel Tuya i automatyzacje.

## Stan z audytu 2026-07-20

| Element | Stan |
| --- | --- |
| Kontener | `automation-home-assistant` |
| Obraz | `ghcr.io/home-assistant/home-assistant:stable` |
| Status | `Exited (137)` |
| Katalog config | `/opt/automation/home-assistant/config` |
| Onboarding | zakończony |
| MQTT integration | brak skonfigurowanego wpisu |
| Encje Tuya | brak |
| Add-ons/Supervisor | brak, bo to Home Assistant Container |

## Znane integracje / elementy HA

W konfiguracji były widoczne podstawowe integracje systemowe i onboardingowe, między innymi:

- `sun`,
- `analytics`,
- `go2rtc`,
- `backup`,
- `shopping_list`,
- `met`,
- `google_translate`,
- `radio_browser`.

Nie było zarejestrowanych encji MQTT/Tuya.

## Główne ryzyko

Kod `Exited (137)` i wysoki swap na Pi 3B+ oznaczają ryzyko problemów z pamięcią. Home Assistant może działać, ale trzeba go odpalać ostrożnie i obserwować przez kilka godzin.

## Docelowy kierunek

- Zostawić Debian + Docker jako bazę.
- Zostawić Mosquitto jako centralny broker.
- Zostawić `tuya-mqtt-bridge` jako lokalne źródło encji Tuya.
- Dodać integrację MQTT w Home Assistant.
- Dodać przełączniki i sensory MQTT dla gniazdek.
- Przenieść dashboard z `tuya-control-panel` do Home Assistanta dopiero po potwierdzeniu stabilności.
- `tuya-control-panel` usuwać lub wyłączać dopiero po testach HA.

## Decyzja do podjęcia

Sposób uruchomienia HA:

| Opcja | Plus | Minus |
| --- | --- | --- |
| `network_mode: host` | lepsze discovery, mDNS, Bluetooth, zgodne z typową architekturą HA Container | większa ekspozycja usług, trzeba pilnować portów |
| `127.0.0.1:8123` + Caddy | ostrożniejsza ekspozycja, łatwiejsze spięcie z proxy | discovery może działać gorzej, więcej ręcznej konfiguracji |

## Minimalny plan migracji

1. Backup `/opt/automation`, MQTT, HA config, Tuya bridge i plików `.env`.
2. Uruchomić Home Assistanta bez zmian w Tuya.
3. Monitorować RAM, swap i logi.
4. Dodać integrację MQTT do istniejącego Mosquitto.
5. Dodać encje `gniazdo1`, `gniazdo2`, `gniazdo3` jako MQTT switches.
6. Zbudować dashboard HA odpowiadający staremu panelowi.
7. Dopiero wtedy ograniczać albo usuwać `tuya-control-panel`.
