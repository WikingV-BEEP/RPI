---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - mqtt
  - tuya
updated: 2026-09-12
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

Bieżący sposób obsługi Tuya to MQTT. Nie używać osobnej integracji Tuya MCP ani komend `set_tuya_plug` jako głównego API.

### Topic patterny

| Cel | Topic |
| --- | --- |
| Stan gniazdka | `tuya/<alias>/RW/state` |
| Komenda do gniazdka | `tuya/<alias>/RW/state/set` |
| Dostępność | `tuya/<alias>/RO/availability` |
| Nazwa raportowana | `tuya/<alias>/RO/name` |
| Dane tylko do odczytu | `tuya/<alias>/RO/...` |
| Komenda grupowa | `tuya/all/RW/state/set` |

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

| Alias | State topic | Command topic |
| --- | --- | --- |
| `gniazdo1` | `tuya/gniazdo1/RW/state` | `tuya/gniazdo1/RW/state/set` |
| `gniazdo2` | `tuya/gniazdo2/RW/state` | `tuya/gniazdo2/RW/state/set` |
| `gniazdo3` | `tuya/gniazdo3/RW/state` | `tuya/gniazdo3/RW/state/set` |

## Komendy MQTT

Na topic `tuya/<alias>/RW/state/set` bridge przyjmuje tylko:

- `ON`,
- `OFF`,
- `RESTART`,
- `STATUS`.

Dla `tuya/all/RW/state/set` dozwolone są te same komendy:

- `ON`,
- `OFF`,
- `RESTART`,
- `STATUS`.

Komendy `SWITCH` i `ZMIEN_STAN` nie są obsługiwane przez bieżący bridge MQTT. Nie wpisywać ich do `tuya/<alias>/RW/state/set`.

Payload tekstowy może być małymi lub wielkimi literami, bo bridge normalizuje go do uppercase. Przykłady:

```text
ON
OFF
RESTART
STATUS
```

Payload JSON jest obsługiwany dla pola `command`; `duration` ma sens przy `RESTART`:

```json
{"command":"RESTART","duration":1.5}
```

Publikować komendy bez retain:

```text
retain=false
```

## Przykłady MQTT

Bezpieczne odświeżenie statusu:

```bash
mosquitto_pub -h 127.0.0.1 -t tuya/gniazdo3/RW/state/set -m STATUS
```

Wyłączenie `gniazdo3`:

```bash
mosquitto_pub -h 127.0.0.1 -t tuya/gniazdo3/RW/state/set -m OFF
```

Odczyt stanu:

```bash
mosquitto_sub -h 127.0.0.1 -t 'tuya/gniazdo3/#' -v
```

## Zasady bezpieczeństwa

- Najpierw używać `STATUS`, dopiero potem komend zmieniających stan.
- Nie używać komend grupowych, jeśli nie jest jasne, które urządzenia są podłączone.
- Nie zapisywać haseł MQTT w notatkach.
- Po zmianie bridge sprawdzić topic `tuya/<alias>/RW/state`, `tuya/<alias>/RO/name` i logi kontenera.

## Home Assistant

Bridge nie publikował payloadów Home Assistant MQTT Discovery. Dlatego encje w HA trzeba dodać ręcznie w YAML albo rozbudować bridge o Discovery.
