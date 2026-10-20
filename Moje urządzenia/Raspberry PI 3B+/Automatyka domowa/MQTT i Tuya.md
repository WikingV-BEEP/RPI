---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - mqtt
  - tuya
updated: 2026-09-13
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
| Czas przerwy przy `RESTART` | `tuya/<alias>/RW/restart_duration` |
| Ustawienie czasu przerwy przy `RESTART` | `tuya/<alias>/RW/restart_duration/set` |
| Dostępność | `tuya/<alias>/RO/availability` |
| Nazwa raportowana | `tuya/<alias>/RO/name` |
| Dane tylko do odczytu | `tuya/<alias>/RO/...` |
| Komenda grupowa | `tuya/all/RW/state/set` |
| Grupowe ustawienie czasu przerwy | `tuya/all/RW/restart_duration/set` |

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

| Alias | State topic | Command topic | Restart duration topic | Restart duration set topic |
| --- | --- | --- | --- | --- |
| `gniazdo1` | `tuya/gniazdo1/RW/state` | `tuya/gniazdo1/RW/state/set` | `tuya/gniazdo1/RW/restart_duration` | `tuya/gniazdo1/RW/restart_duration/set` |
| `gniazdo2` | `tuya/gniazdo2/RW/state` | `tuya/gniazdo2/RW/state/set` | `tuya/gniazdo2/RW/restart_duration` | `tuya/gniazdo2/RW/restart_duration/set` |
| `gniazdo3` | `tuya/gniazdo3/RW/state` | `tuya/gniazdo3/RW/state/set` | `tuya/gniazdo3/RW/restart_duration` | `tuya/gniazdo3/RW/restart_duration/set` |

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

Payload JSON jest obsługiwany dla pola `command`; `duration` podaje czas w sekundach, a `duration_ms` czas w milisekundach. Oba mają sens przy `RESTART` i działają jako jednorazowe nadpisanie czasu przerwy:

```json
{"command":"RESTART","duration":1.5}
```

```json
{"command":"RESTART","duration_ms":1000}
```

Publikować komendy bez retain:

```text
retain=false
```

## Czas restartu

Bridge publikuje aktualną przerwę używaną przy `RESTART` w topicu:

```text
tuya/<alias>/RW/restart_duration
```

Wartość jest liczbą milisekund jako tekst. Ustawienie osobnej wartości dla urządzenia:

```text
tuya/<alias>/RW/restart_duration/set
```

Przykład dla `gniazdo3`: payload `1000` ustawia przerwę na 1000 ms, czyli 1 sekundę.

```bash
mosquitto_pub -h 127.0.0.1 -t tuya/gniazdo3/RW/restart_duration/set -m '1000'
```

Ustawienie dla wszystkich aliasów:

```bash
mosquitto_pub -h 127.0.0.1 -t tuya/all/RW/restart_duration/set -m '1000'
```

`RW/restart_duration/set` nie włącza ani nie wyłącza gniazdka. Zmieniona wartość będzie użyta przy kolejnych komendach `RESTART`, jeśli w samej komendzie nie podano `duration` albo `duration_ms`.

Po restarcie bridge wartość wraca do domyślnej konfiguracji `RESTART_DURATION_SECONDS`.

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

Odczyt ustawionej przerwy restartu:

```bash
mosquitto_sub -h 127.0.0.1 -t 'tuya/gniazdo3/RW/restart_duration' -v
```

## Zasady bezpieczeństwa

- Najpierw używać `STATUS`, dopiero potem komend zmieniających stan.
- Nie używać komend grupowych, jeśli nie jest jasne, które urządzenia są podłączone.
- Nie zapisywać haseł MQTT w notatkach.
- Po zmianie bridge sprawdzić topic `tuya/<alias>/RW/state`, `tuya/<alias>/RO/name`, `tuya/<alias>/RW/restart_duration` i logi kontenera.

## Home Assistant

Bridge nie publikował payloadów Home Assistant MQTT Discovery. Dlatego encje w HA trzeba dodać ręcznie w YAML albo rozbudować bridge o Discovery.
