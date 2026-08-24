---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - docker
  - automatyka-domowa
updated: 2026-08-24
---

# Docker i automatyka domowa

## Główny stos

Główny stos automatyki jest zdefiniowany w:

```text
/opt/automation/docker-compose.yml
```

Ostatni potwierdzony stan z 2026-07-20:

| Kontener | Obraz | Stan | Porty / ekspozycja | Uwagi |
| --- | --- | --- | --- | --- |
| `automation-mosquitto` | `eclipse-mosquitto:2` | działał | `0.0.0.0:1883`, `0.0.0.0:9001` | Centralny broker MQTT. |
| `automation-node-red` | `nodered/node-red:latest` | działał, `healthy` | `127.0.0.1:1880` | Brak widocznych flow w katalogu danych podczas audytu. |
| `automation-home-assistant` | `ghcr.io/home-assistant/home-assistant:stable` | zatrzymany, `Exited (137)` | w compose przewidziane `127.0.0.1:8123` | Konfiguracja istnieje, onboarding zakończony, brak MQTT/Tuya. |

## Powiązane katalogi

Rozmiary z audytu 2026-07-20:

| Ścieżka | Rozmiar | Rola |
| --- | --- | --- |
| `/opt/automation/home-assistant/config` | około `1.8M` | konfiguracja Home Assistanta |
| `/opt/automation/mosquitto` | około `156K` | konfiguracja/dane Mosquitto |
| `/opt/automation/node-red` | około `168K` | dane Node-RED |
| `/home/admin/tuya-mqtt-bridge` | około `104K` | most Tuya -> MQTT |
| `/home/admin/tuya-control-panel` | około `92K` | panel Flask do sterowania |
| `/home/admin/mqtt-mcp-connector` | około `100K` | MCP do MQTT/Tuya |

## Dodatkowe usługi poza `/opt/automation`

- `tuya-mqtt-bridge` działa z `network_mode: host` i komunikuje się lokalnie z urządzeniami Tuya oraz brokerem MQTT.
- `tuya-control-panel` wystawia UI na `0.0.0.0:8090`.
- `mqtt-mcp-connector` wystawia MCP na `0.0.0.0:8092/mcp`.
- `obsidian-mcp` i auth proxy obsługują zewnętrzny dostęp do narzędzi MCP przez Caddy.

## Zasada aktualizacji

- Najpierw backup katalogów i plików `.env`.
- Potem `docker compose config` dla walidacji składni.
- Następnie start jednej usługi i obserwacja logów.
- Na Pi 3B+ szczególnie obserwować RAM i swap.


## Szczegółowa rozpiska kontenerów

Pełna inwentaryzacja compose, kontenerów, portów, mountów i env bez sekretów jest w: [[Moje urządzenia/Raspberry PI 3B+/11 - Kontenery skonfigurowane|Kontenery skonfigurowane]].
