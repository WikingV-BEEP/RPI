---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - docker
  - kontenery
  - compose
updated: 2026-08-24
---

# Kontenery skonfigurowane

## Status danych

Ta notatka opisuje konfigurację potwierdzoną w zadaniach z 2026-07-10, 2026-07-14, 2026-07-16 i 2026-07-20. Dwie próby świeżego odczytu 2026-08-24 zakończyły się błędem workera bez raportu, więc bieżący `docker ps -a` trzeba jeszcze odświeżyć.

## Stos `/opt/automation`

Plik:

```text
/opt/automation/docker-compose.yml
```

### `automation-mosquitto`

| Pole | Wartość |
| --- | --- |
| Service | `mosquitto` |
| Container | `automation-mosquitto` |
| Image | `eclipse-mosquitto:2` |
| Restart | `unless-stopped` |
| Porty | `1883:1883`, `9001:9001` |
| Stan z audytu | działał 2026-07-20 |
| Healthcheck | brak potwierdzonego healthchecka |

Mounty:

| Host | Kontener | Tryb |
| --- | --- | --- |
| `/opt/automation/mosquitto/config/mosquitto.conf` | `/mosquitto/config/mosquitto.conf` | `ro` |
| `/opt/automation/mosquitto/config/passwd` | `/mosquitto/config/passwd` | `ro` |
| `/opt/automation/mosquitto/data` | `/mosquitto/data` | `rw` |
| `/opt/automation/mosquitto/log` | `/mosquitto/log` | `rw` |

Konfiguracja brokera:

```text
listener 1883
listener 9001
protocol websockets
allow_anonymous false
password_file /mosquitto/config/passwd
```

### `automation-home-assistant`

| Pole | Wartość |
| --- | --- |
| Service | `home-assistant` |
| Container | `automation-home-assistant` |
| Image | `ghcr.io/home-assistant/home-assistant:stable` |
| Restart | `unless-stopped` |
| Privileged | `true` |
| Port | `127.0.0.1:8123:8123` |
| Stan z audytu | `Exited (137)` 2026-07-20 |
| Healthcheck | brak potwierdzonego healthchecka |

Mounty:

| Host | Kontener | Tryb |
| --- | --- | --- |
| `/opt/automation/home-assistant/config` | `/config` | `rw` |
| `/etc/localtime` | `/etc/localtime` | `ro` |

Uwagi:

- onboarding HA był zakończony,
- brak skonfigurowanego MQTT integration,
- brak encji Tuya,
- kod `Exited (137)` oznacza ryzyko RAM/OOM na Pi 3B+.

### `automation-node-red`

| Pole | Wartość |
| --- | --- |
| Service | `node-red` |
| Container | `automation-node-red` |
| Image | `nodered/node-red:latest` |
| Restart | `unless-stopped` |
| Port | `127.0.0.1:1880:1880` |
| Depends on | `mosquitto` |
| Stan z audytu | działał, `healthy` 2026-07-20 |

Mounty:

| Host | Kontener |
| --- | --- |
| `/opt/automation/node-red/data` | `/data` |

## Stos `/home/admin/tuya-mqtt-bridge`

Plik:

```text
/home/admin/tuya-mqtt-bridge/docker-compose.yml
```

### `tuya-mqtt-bridge`

| Pole | Wartość |
| --- | --- |
| Container | `tuya-mqtt-bridge` |
| Image | `tuya-mqtt-bridge-tuya-mqtt-bridge` |
| Build | lokalny Dockerfile, Python `3.12-slim` |
| Command | `python /app/bridge.py ...` |
| Network | `host` |
| Restart | `unless-stopped` |
| Healthcheck | brak |
| Stan z audytu | `Up`, połączony z MQTT |

Env bez sekretów:

```text
MQTT_HOST=127.0.0.1
MQTT_PORT=1883
MQTT_TLS=false
MQTT_TLS_INSECURE=false
MQTT_USERNAME=admin
MQTT_PASSWORD=<ukryte>
MQTT_TOPIC_PREFIX=tuya
MQTT_HEARTBEAT_SECONDS=10
POLL_INTERVAL_SECONDS=30
RESTART_DURATION_SECONDS=0.5
LOG_LEVEL=INFO
```

Mounty:

| Host | Kontener | Tryb |
| --- | --- | --- |
| `/home/admin/tuya-mqtt-bridge/config` | `/app/config` | `ro` |

Rola:

- lokalna komunikacja z trzema gniazdkami Tuya przez LAN,
- publikacja stanu pod `tuya/<alias>/RO/...` i `tuya/<alias>/RW/state`,
- odbiór komend z `tuya/<alias>/RW/state/set` oraz `tuya/all/RW/state/set`.

## Stos `/home/admin/tuya-control-panel`

Plik:

```text
/home/admin/tuya-control-panel/docker-compose.yml
```

### `tuya-control-panel`

| Pole | Wartość |
| --- | --- |
| Container | `tuya-control-panel` |
| Image | `tuya-control-panel-tuya-control-panel` |
| Command | `python -m app.app` |
| Network | `host` |
| HTTP | `0.0.0.0:8090` |
| Restart | do odświeżenia z compose |
| Healthcheck | brak; `/health` zwracał `404` |
| Stan z audytu | `Up`, panel zwracał HTML `200` |

Env bez sekretów:

```text
MQTT_HOST=127.0.0.1
MQTT_PORT=1883
MQTT_TLS=false
MQTT_TLS_INSECURE=false
MQTT_USERNAME=admin
MQTT_PASSWORD=<ukryte>
MQTT_TOPIC_PREFIX=tuya
HTTP_HOST=0.0.0.0
DRY_RUN=false
```

Uwagi:

- aplikacja działa jako lekki panel Flask,
- logi ostrzegały, że to Flask development server,
- panel może zostać zastąpiony dashboardem Home Assistant po migracji.

## Stos `/home/admin/mqtt-mcp-connector`

Plik:

```text
/home/admin/mqtt-mcp-connector/docker-compose.yml
```

### `mqtt-mcp-connector`

| Pole | Wartość |
| --- | --- |
| Container | `mqtt-mcp-connector` |
| Image | `mqtt-mcp-connector:local` |
| Build | lokalny Dockerfile, Python `3.13-slim` |
| Command | `python server.py --transport streamable-http ...` |
| Network | `host` |
| Listener | `0.0.0.0:8092/mcp` |
| Restart | `unless-stopped` |
| Healthcheck | brak |
| Stan z audytu | `Up`, MCP tools/list działał |

Env bez sekretów:

```text
MQTT_HOST=127.0.0.1
MQTT_PORT=1883
MQTT_TLS=false
MQTT_TLS_INSECURE=false
MQTT_USERNAME=admin
MQTT_PASSWORD=<ukryte>
MQTT_CLIENT_ID=mqtt-mcp-connector
MQTT_CONNECT_TIMEOUT_SECONDS=10
MQTT_STATE_WAIT_SECONDS=2
MCP_HOST=0.0.0.0
MCP_PORT=8092
MCP_PATH=/mcp
```

Mounty:

| Host | Kontener | Tryb |
| --- | --- | --- |
| `/home/admin/mqtt-mcp-connector/config` | `/app/config` | `ro` |

Wystawione narzędzia:

- `list_entities`,
- `get_entity_state`,
- `publish_mqtt`,
- `set_tuya_plug`,
- `refresh_states`.

Subskrypcje z logów:

```text
tuya/+/RO/#
tuya/+/RW/state
tuya/bridge/RO/#
```

## Stos `/home/admin/obsidian-mcp`

Plik:

```text
/home/admin/obsidian-mcp/docker-compose.yml
```

### `obsidian-mcp`

| Pole | Wartość |
| --- | --- |
| Container | `obsidian-mcp` |
| Image | `obsidian-mcp-obsidian-mcp` |
| Port | `0.0.0.0:8765->8000/tcp` |
| Rola | MCP do vaulta Obsidian |
| Stan z audytu | `Up` |

### `codex-mcp`

| Pole | Wartość |
| --- | --- |
| Container | `codex-mcp` |
| Image | `obsidian-mcp-codex-mcp` |
| Port | `8000/tcp` wewnątrz sieci Docker |
| Rola | MCP do zlecania i odczytu zadań Codex worker |
| Stan z audytu | `Up` |

### `obsidian-mcp-auth`

| Pole | Wartość |
| --- | --- |
| Container | `obsidian-mcp-auth` |
| Image | `obsidian-mcp-auth-proxy` |
| Command | `python auth_proxy.py` |
| Upstream | `http://obsidian-mcp:8000` |
| Rola | OAuth/auth proxy dla Obsidian MCP |
| Sekret | `OAUTH_PASSWORD=<ukryte>` w `.env` |

### `codex-mcp-auth`

| Pole | Wartość |
| --- | --- |
| Container | `codex-mcp-auth` |
| Image | `obsidian-mcp-codex-auth-proxy` |
| Command | `python auth_proxy.py` |
| Path prefix | `/codex` |
| Upstream | `http://codex-mcp:8000` |
| Rola | OAuth/auth proxy dla Codex MCP |

### `mqtt-mcp-auth`

| Pole | Wartość |
| --- | --- |
| Container | `mqtt-mcp-auth` |
| Image | `obsidian-mcp-auth-proxy` |
| Command | `python auth_proxy.py` |
| Public base URL | `https://wikingv.servehalflife.com/mqtt-mcp` |
| Path prefix | `/mqtt-mcp` |
| Upstream | `http://172.18.0.1:8092` |
| Rola | OAuth/auth proxy dla MQTT MCP |

### `obsidian-mcp-caddy`

| Pole | Wartość |
| --- | --- |
| Container | `obsidian-mcp-caddy` |
| Image | `caddy:2` |
| Porty | `80:80`, `443:443`, `443/udp` |
| Config | `/home/admin/obsidian-mcp/Caddyfile -> /etc/caddy/Caddyfile:ro` |
| Volumes | `caddy-data`, `caddy-config` |
| Rola | HTTPS reverse proxy |

Trasy Caddy potwierdzone w konfiguracji:

```text
/codex/*      -> codex-auth-proxy:8010
/mqtt-mcp/*   -> mqtt-mcp-auth-proxy:8010
/sse, /messages/*, /.well-known/*, /oauth/*, /register, /token -> auth-proxy:8010
/nodered*     -> automation-node-red:1880
/mqtt*        -> automation-mosquitto:9001
fallback      -> https://172.18.0.1:9090
```

### `obsidian-mcp-tunnel`

| Pole | Wartość |
| --- | --- |
| Container | `obsidian-mcp-tunnel` |
| Image | `cloudflare/cloudflared:latest` |
| Command | `tunnel --no-autoupdate run --token ${CLOUDFLARE_TUNNEL_TOKEN}` |
| Sekret | `CLOUDFLARE_TUNNEL_TOKEN=<ukryte>` w `.env` |
| Rola | tunel Cloudflare do publicznego dostępu |

## Ryzyka kontenerów

- `tuya-mqtt-bridge`, `tuya-control-panel` i `mqtt-mcp-connector` używają `network_mode: host`.
- `8090`, `8092`, `1883` i `9001` były widoczne na `0.0.0.0` w audytach.
- Dla kilku kontenerów brak healthchecków.
- `tuya-control-panel` działa na Flask development serverze.
- Home Assistant był zatrzymany z kodem `137`, więc przed startem trzeba pilnować RAM/swap.

## Do odświeżenia

- [ ] Aktualny `docker ps -a` z 2026-08-24 lub nowszy.
- [ ] Aktualny `docker inspect` dla wszystkich kontenerów.
- [ ] Aktualne healthchecki, restart count i started_at.
- [ ] Czy po nieudanych zadaniach z 2026-08-24 istnieją ślady kontenera `mqtt-scheduler`.
