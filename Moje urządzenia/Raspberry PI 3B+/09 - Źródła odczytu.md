---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - zrodla
  - audyt
updated: 2026-08-24
---

# Źródła odczytu

## Źródła użyte do tej sekcji

Ta sekcja powstała na podstawie wcześniejszych bezpiecznych odczytów i zadań wykonanych na Raspberry Pi:

| Data | Zakres | Status |
| --- | --- | --- |
| 2026-07-16 | Diagnostyka niedostępnego MCP/MQTT, endpointy, Caddy/auth proxy, `mqtt-mcp-connector` | potwierdzone historycznie |
| 2026-07-16 | Aktualizacja schematu komend `set_tuya_plug`, weryfikacja `list_tools`, aliasy gniazdek | potwierdzone historycznie |
| 2026-07-20 | Ocena migracji Home Assistant, Docker, Mosquitto, Node-RED, Cockpit, zasoby RAM/swap | potwierdzone historycznie |
| 2026-08-24 | Próba świeżego audytu konfiguracji RPi | nieudana, brak raportu i logu końcowego |

## Typy sprawdzonych danych

W poprzednich audytach były sprawdzane między innymi:

- statusy usług systemd, np. Cockpit,
- stan kontenerów Docker,
- konfiguracja compose dla `/opt/automation`,
- rozmiary kluczowych katalogów,
- listener MCP na `0.0.0.0:8092`,
- publiczna ścieżka `/mqtt-mcp/mcp`,
- integracje i encje Home Assistanta,
- topic patterny MQTT/Tuya,
- schema narzędzia MCP `set_tuya_plug`.

## Ograniczenia danych

- Dane nie są pełnym audytem z 2026-08-24, bo zdalny worker nie zwrócił raportu.
- Lokalne IP, kernel, uptime, dyski, temperatura i aktualne kontenery wymagają odświeżenia.
- W notatkach celowo nie ma haseł, tokenów ani kluczy.
- Wartości z plików `.env` powinny być zawsze maskowane.

## Zasada aktualizacji tej sekcji

Po każdym większym audycie dopisać:

- datę odczytu,
- zakres sprawdzeń,
- czy były wykonywane zmiany,
- czy dotknięto usług sterujących urządzeniami,
- czy zapisano lub zobaczono sekrety, i czy wymagają rotacji.


## Uzupełnienie 2026-08-24

Na prośbę o dopisanie dostępów, OAuth MCP i kontenerów wykorzystano wcześniejsze potwierdzone odczyty z zadań:

- `20260710T230756Z-Napraw-LAN-bind-MQTT`,
- `20260710T230803Z-Deploy-Tuya-MQTT-bridge-on-Raspberry-Pi`,
- `20260710T223036Z-Deploy-MCP-worker-status-fix`,
- `20260714T210431Z-Build-MCP-MQTT-Connector-container`,
- `20260714T211629Z-Configure-MQTT-credentials-for-Tuya-services`,
- `20260714T223328Z-Convert-MQTT-MCP-connector-to-standard-remote-MCP-endpoint`,
- `20260714T225826Z-Audit-current-Tuya-MQTT-MCP-stack-readiness`,
- `20260714T225854Z-Quick-bounded-Tuya-MQTT-MCP-readiness-check`,
- `20260716T205531Z-Diagnostyka-niedost-pnego-MCP-MQTT`,
- `20260716T210334Z-Od-wie-schemat-SWITCH-MCP-MQTT`,
- `20260720T224312Z-Evaluate-Home-Assistant-migration`.

Dodatkowa próba świeżego odczytu `20260824T212145Z-Odczyt-kontenerow-i-OAuth-MCP-do-notatek` zakończyła się błędem workera bez raportu.
