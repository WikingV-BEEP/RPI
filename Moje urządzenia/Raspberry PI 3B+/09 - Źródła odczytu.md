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
