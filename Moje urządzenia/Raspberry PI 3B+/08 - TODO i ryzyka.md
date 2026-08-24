---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - todo
  - ryzyka
updated: 2026-08-24
---

# TODO i ryzyka

## Pilne

- [ ] Odświeżyć pełny audyt urządzenia: kernel, uptime, IP, DNS, brama, dyski, partycje, temperatura, throttling.
- [ ] Sprawdzić aktualny stan kontenerów Docker po 2026-07-20.
- [ ] Zrobić backup `/opt/automation` i projektów w `/home/admin` przed kolejnymi zmianami.
- [ ] Rozważyć rotację hasła MQTT i sekretów automatyki.

## Home Assistant

- [ ] Ustalić, czy HA ma działać w `network_mode: host`, czy przez `127.0.0.1:8123` + Caddy.
- [ ] Uruchomić HA i obserwować RAM/swap przez kilka godzin.
- [ ] Dodać integrację MQTT do istniejącego Mosquitto.
- [ ] Dodać encje Tuya jako MQTT switches/sensors.
- [ ] Zdecydować: ręczne YAML czy MQTT Discovery w `tuya-mqtt-bridge`.
- [ ] Dopiero po testach ograniczać `tuya-control-panel`.

## MQTT / MCP / Tuya

- [ ] Dodać healthcheck dla `mqtt-mcp-connector`.
- [ ] Dodać healthcheck dla auth proxy, jeśli go nie ma.
- [ ] Uporządkować dokumentację publicznych ścieżek: `/mqtt-mcp/mcp`, OAuth metadata, Caddy.
- [ ] Nie wykonywać testów `ON/OFF/SWITCH` bez świadomego wyboru urządzenia.

## Ryzyka

| Ryzyko | Skutek | Co zrobić |
| --- | --- | --- |
| Mało RAM na Pi 3B+ | HA może się zatrzymywać albo mocno używać swap | monitorować RAM/swap, ograniczyć usługi, rozważyć lżejszą konfigurację |
| Porty `0.0.0.0` | usługi są widoczne z LAN | potwierdzić, które porty mają być publiczne w LAN |
| Sekrety w `.env` | przypadkowe ujawnienie przy logach/notatkach | maskować wartości, rozważyć rotację |
| Brak MQTT Discovery | HA nie zobaczy automatycznie gniazdek Tuya | ręczny YAML albo rozbudowa bridge |
| Cockpit usunięty za wcześnie | utrata wygodnej administracji OS | zostawić Cockpit do końca migracji |
| Brak testu restore | backup może być bezużyteczny | po backupie wykonać próbę odczytu/odtworzenia na boku |

## Do uzupełnienia po świeżym audycie

- [ ] Lokalne IP Raspberry Pi.
- [ ] Aktualny rozmiar i zajętość partycji.
- [ ] Aktualne wersje obrazów Docker.
- [ ] Aktualne logi Home Assistanta po starcie.
- [ ] Aktualna konfiguracja Caddy bez sekretów.
