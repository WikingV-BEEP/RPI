---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - dostepy
  - oauth
  - mcp
  - sekrety
updated: 2026-08-24
---

# Dostępy i OAuth MCP

## Zasada bezpieczeństwa

Nie zapisywać w tej notatce jawnych haseł, tokenów, kluczy Tuya ani prywatnych kluczy SSH. Vault Obsidian może być synchronizowany, indeksowany i backupowany, więc trzymać tu tylko mapę dostępu: nazwy kont, ścieżki sekretów, endpointy i procedury.

Wartości sekretów zapisywać jako:

```text
<ukryte>
```

## Dostęp administracyjny do Raspberry Pi

| Dostęp | Co wiadomo | Sekret / hasło |
| --- | --- | --- |
| SSH | usługa działa na porcie `22`; główny katalog roboczy i projekty są pod `/home/admin` | hasło/klucz poza notatkami |
| Cockpit | panel admin systemu na `9090`; publiczny fallback Caddy prowadzi do `https://172.18.0.1:9090` | logowanie kontem systemowym, hasło poza notatkami |
| Docker/Compose | używany lokalnie na RPi, część operacji wymaga `sudo` | hasło sudo poza notatkami |

## MQTT

| Element | Wartość |
| --- | --- |
| Broker | `automation-mosquitto` |
| Protokół | MQTT `1883`, WebSocket `9001` |
| Użytkownik | `admin` |
| Hasło | `<ukryte>` |
| Plik hasła brokera | `/opt/automation/mosquitto/config/passwd` |
| Konfiguracja brokera | `/opt/automation/mosquitto/config/mosquitto.conf` |
| Anonymous | `allow_anonymous false` |

Hasło MQTT jest używane przez:

- `/home/admin/tuya-mqtt-bridge/.env` jako `MQTT_PASSWORD=<ukryte>`,
- `/home/admin/tuya-control-panel/.env` jako `MQTT_PASSWORD=<ukryte>`,
- `/home/admin/mqtt-mcp-connector/.env` jako `MQTT_PASSWORD=<ukryte>`.

Do zrobienia: rozważyć rotację hasła MQTT, bo w historii prac technicznych mogło zostać wypisane w stdout. W notatkach nie powielać tej wartości.

## Tuya LAN

| Element | Gdzie jest | Uwagi |
| --- | --- | --- |
| Konfiguracja urządzeń | `/home/admin/tuya-mqtt-bridge/config/tuya-local.json` | zawiera lokalne dane urządzeń Tuya, nie kopiować wartości kluczy |
| Device IDs | w konfiguracji bridge i topicach RO | traktować jako techniczne identyfikatory, nie hasła |
| Local keys | w konfiguracji bridge | `local_key=<ukryte>` |

Aliasami roboczymi są:

- `gniazdo1` - Lampa akwariowa,
- `gniazdo2` - Lampka akwariowa 2,
- `gniazdo3` - Lampka Biurko.

## OAuth MCP - wspólny model

MCP jest wystawiane przez reverse proxy i auth proxy. Klient najpierw przechodzi OAuth, potem łączy się z właściwym endpointem MCP.

Typowe elementy:

| Element | Rola |
| --- | --- |
| `auth_proxy.py` | obsługa OAuth i proxy do backendu MCP |
| `OAUTH_PASSWORD` | hasło/autoryzacja w `.env`, wartość `<ukryte>` |
| `PUBLIC_BASE_URL` | publiczny URL widziany przez klienta |
| `PATH_PREFIX` | prefiks ścieżki w Caddy, np. `/codex` lub `/mqtt-mcp` |
| `UPSTREAM_MCP_URL` | wewnętrzny adres backendu MCP |
| `/.well-known/oauth-protected-resource` | metadata chronionego zasobu OAuth |
| `/.well-known/openid-configuration` | metadata serwera autoryzacji |
| `/oauth/authorize` | początek logowania OAuth |
| `/oauth/token` | wymiana kodu na token |

## Obsidian MCP

| Element | Wartość |
| --- | --- |
| Backend | `obsidian-mcp` |
| Auth proxy | `obsidian-mcp-auth` |
| Publiczne ścieżki Caddy | `/sse`, `/messages/*`, `/.well-known/*`, `/oauth/*`, `/register`, `/token` |
| Upstream auth proxy | `http://obsidian-mcp:8000` |
| Lokalny port hosta | `0.0.0.0:8765->8000/tcp` dla `obsidian-mcp` |
| Sekret OAuth | `/home/admin/obsidian-mcp/.env`, `OAUTH_PASSWORD=<ukryte>` |

Uwaga: w `_Templates/OAuth key MCP.md` istnieje stara notatka z jawnym hasłem OAuth dla klienta ChatGPT. Nie kopiować go dalej; lepiej przenieść sekret do bezpiecznego managera i potem zmienić/usunąć tę notatkę albo zostawić tam tylko placeholder.

## Codex MCP

| Element | Wartość |
| --- | --- |
| Backend | `codex-mcp` |
| Auth proxy | `codex-mcp-auth` |
| Publiczna ścieżka | `/codex/*` |
| Caddy | `handle_path /codex/* -> codex-auth-proxy:8010` |
| Upstream | `http://codex-mcp:8000` |
| Rola | zlecanie i odczyt zadań Codex workera na Raspberry Pi |
| Sekret OAuth | przez `.env` auth proxy, wartość `<ukryte>` |

## MQTT MCP

| Element | Wartość |
| --- | --- |
| Backend | `mqtt-mcp-connector` |
| Lokalny MCP URL | `http://127.0.0.1:8092/mcp` albo `http://<rpi-ip>:8092/mcp` w LAN |
| Auth proxy | `mqtt-mcp-auth` |
| Publiczny URL | `https://wikingv.servehalflife.com/mqtt-mcp/mcp` |
| `PUBLIC_BASE_URL` | `https://wikingv.servehalflife.com/mqtt-mcp` |
| `PATH_PREFIX` | `/mqtt-mcp` |
| `UPSTREAM_MCP_URL` | `http://172.18.0.1:8092` |
| Caddy | `handle_path /mqtt-mcp/* -> mqtt-mcp-auth-proxy:8010` |
| Transport | `streamable-http` |

Narzędzia MCP wystawiane przez `mqtt-mcp-connector`:

- `list_entities`,
- `get_entity_state`,
- `publish_mqtt`,
- `set_tuya_plug`,
- `refresh_states`.

Komendy dla `set_tuya_plug`:

- pojedyncze gniazdko: `ON`, `OFF`, `RESTART`, `STATUS`, `SWITCH`, `ZMIEN_STAN`,
- alias `all`: tylko `ON`, `OFF`, `RESTART`, `STATUS`.

## Cloudflare / Caddy

| Element | Wartość |
| --- | --- |
| Caddy | `obsidian-mcp-caddy`, obraz `caddy:2` |
| Publiczne porty | `80`, `443` |
| Tunnel | `obsidian-mcp-tunnel`, obraz `cloudflare/cloudflared:latest` |
| Token tunelu | `/home/admin/obsidian-mcp/.env`, `CLOUDFLARE_TUNNEL_TOKEN=<ukryte>` |
| Caddyfile | `/home/admin/obsidian-mcp/Caddyfile` |

## Rotacja sekretów - checklist

- [ ] Przenieść jawny OAuth secret z `_Templates/OAuth key MCP.md` do bezpiecznego managera haseł.
- [ ] Zmienić `OAUTH_PASSWORD` w `/home/admin/obsidian-mcp/.env`.
- [ ] Zrestartować odpowiednie auth proxy po zmianie OAuth.
- [ ] Zmienić hasło MQTT w `/opt/automation/mosquitto/config/passwd`.
- [ ] Zaktualizować `MQTT_PASSWORD=<ukryte>` w `.env` bridge, panelu i connectora.
- [ ] Zrestartować zależne kontenery po rotacji MQTT.
