---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - mqtt
  - tuya
updated: 2026-09-11
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

| Cel                   | Topic                          |
| --------------------- | ------------------------------ |
| Stan gniazdka         | `tuya/<alias>/RW/state`        |
| Komenda do gniazdka   | `tuya/<alias>/RW/state/set`    |
| Dostępność            | `tuya/<alias>/RO/availability` |
| Dane tylko do odczytu | `tuya/<alias>/RO/...`          |
| Komenda grupowa       | `tuya/all/RW/state/set`        |

## Encje / aliasy

Identyfikacja wykonana 2026-09-11 na Raspberry Pi na podstawie lokalnej konfiguracji bridge, MQTT i tablicy `ip neigh`. IP i MAC potwierdzone z wysoką pewnością.

| Alias | Opis / przeznaczenie | Nazwa raportowana przez MQTT | IP | MAC | Device ID | Producent / hostname |
| --- | --- | --- | --- | --- | --- | --- |
| `gniazdo1` | Lampa akwariowa | `Gniazdo 1` | `192.168.1.41` | `00:33:7a:8e:46:2c` | `bf615201c3e5d108b3nedp` | nieustalone |
| `gniazdo2` | Lampka akwariowa 2 | `Lampka nocna` | `192.168.1.42` | `00:33:7a:8e:3a:c5` | `bf8a4ee13ec93ca850dc04` | nieustalone |
| `gniazdo3` | Lampka Biurko | `Lampka Biurko` | `192.168.1.40` | `00:33:7a:8e:27:b3` | `bfda9f05176299255dgsmw` | nieustalone |

### Uwagi identyfikacyjne

- `gniazdo1` i `gniazdo2` mają w MQTT nazwy inne niż opis przeznaczenia zapisany wcześniej w notatkach; aliasy i adresy sieciowe są jednak potwierdzone.
- Dla prefiksu MAC `00:33:7A` nie udało się lokalnie wiarygodnie potwierdzić producenta/OUI.
- Nie znaleziono wiarygodnych hostname'ów dla tych urządzeń.
- `gniazdo3` miało w logach okresowe problemy z łącznością, ale podczas identyfikacji 2026-09-11 wszystkie trzy urządzenia raportowały `online`.

### Topic per urządzenie

| Alias      | State topic              | Command topic                |
| ---------- | ------------------------ | ---------------------------- |
| `gniazdo1` | `tuya/gniazdo1/RW/state` | `tuya/gniazdo1/RW/state/set` |
| `gniazdo2` | `tuya/gniazdo2/RW/state` | `tuya/gniazdo2/RW/state/set` |
| `gniazdo3` | `tuya/gniazdo3/RW/state` | `tuya/gniazdo3/RW/state/set` |

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
