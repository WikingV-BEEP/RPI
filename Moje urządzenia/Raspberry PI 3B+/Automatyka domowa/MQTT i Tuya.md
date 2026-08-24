---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - mqtt
  - tuya
updated: 2026-08-24
---

# MQTT i Tuya

## Broker

Brokerem MQTT jest Mosquitto z głównego stosu `/opt/automation`.

| Element | Wartość |
| --- | --- |
| Kontener | `automation-mosquitto` |
| Obraz | `eclipse-mosquitto:2` |
| MQTT | `0.0.0.0:1883` |
| Dodatkowy port | `0.0.0.0:9001` |
| Rola | centralny bus komunikatów dla automatyki |

Dane logowania istnieją w konfiguracji, ale nie są zapisywane w notatkach. W notatkach używać tylko formy `MQTT_PASSWORD=<ukryte>`.

## Bridge Tuya

`tuya-mqtt-bridge` steruje trzema lokalnymi gniazdkami Tuya przez LAN i publikuje stan do MQTT.

### Topic patterny

| Cel | Topic |
| --- | --- |
| Stan gniazdka | `tuya/<alias>/RW/state` |
| Komenda do gniazdka | `tuya/<alias>/RW/state/set` |
| Dostępność | `tuya/<alias>/RO/availability` |
| Dane tylko do odczytu | `tuya/<alias>/RO/...` |
| Komenda grupowa | `tuya/all/RW/state/set` |

## Encje / aliasy

Ostatni bezpieczny odczyt MCP z 2026-07-16 zwrócił:

| Alias | Nazwa | State topic | Command topic |
| --- | --- | --- | --- |
| `gniazdo1` | Lampa akwariowa | `tuya/gniazdo1/RW/state` | `tuya/gniazdo1/RW/state/set` |
| `gniazdo2` | Lampka akwariowa 2 | `tuya/gniazdo2/RW/state` | `tuya/gniazdo2/RW/state/set` |
| `gniazdo3` | Lampka Biurko | `tuya/gniazdo3/RW/state` | `tuya/gniazdo3/RW/state/set` |

W tamtym odczycie cache pokazywał `state: ON` i `availability: online` dla wszystkich trzech encji. To jest stan historyczny, nie bieżąca gwarancja.

## Komendy MCP

Dla pojedynczego aliasu akceptowane były:

- `ON`,
- `OFF`,
- `RESTART`,
- `STATUS`,
- `SWITCH`,
- `ZMIEN_STAN`.

Dla aliasu `all` dozwolone były tylko:

- `ON`,
- `OFF`,
- `RESTART`,
- `STATUS`.

## Zasady bezpieczeństwa

- Najpierw używać `STATUS`, dopiero potem komend zmieniających stan.
- Nie używać komend grupowych, jeśli nie jest jasne, które urządzenia są podłączone.
- Nie zapisywać haseł MQTT w notatkach.
- Po zmianie bridge lub connectora sprawdzić `list_tools`, ale nie wykonywać testowych przełączeń bez potrzeby.

## Home Assistant

Bridge nie publikował payloadów Home Assistant MQTT Discovery. Dlatego encje w HA trzeba dodać ręcznie w YAML albo rozbudować bridge o Discovery.
