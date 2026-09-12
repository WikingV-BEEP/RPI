---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - dostepy
  - oauth
  - mcp
  - sekrety
updated: 2026-09-12
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
- `/home/admin/tuya-control-panel/.env` jako `MQTT_PASSWORD=<ukryte>`.

Do zrobienia: rozważyć rotację hasła MQTT, bo w historii prac technicznych mogło zostać wypisane w stdout. W notatkach nie powielać tej wartości.

## Tuya LAN

| Element | Gdzie jest | Uwagi |
| --- | --- | --- |
| Konfiguracja urządzeń | `/home/admin/tuya-mqtt-bridge/config/tuya-local.json` | zawiera lokalne dane urządzeń Tuya, nie kopiować wartości kluczy |
| Device IDs | w konfiguracji bridge i topicach RO | traktować jako techniczne identyfikatory, nie hasła |
| Local keys | w konfiguracji bridge | `local_key=<ukryte>` |
| Sterowanie | MQTT | używać `tuya/<alias>/RW/state/set`, bez integracji Tuya MCP |

Aliasami roboczymi są:

- `gniazdo1` - Lampa akwariowa,
- `gniazdo2` - Lampka akwariowa 2,
- `gniazdo3` - Lampka Biurko.

Bieżąca obsługa Tuya odbywa się przez MQTT i panel WWW. Nie utrzymywać osobnej integracji Tuya MCP ani narzędzia `set_tuya_plug` jako używanej ścieżki sterowania.

Obsługiwane komendy MQTT dla `tuya/<alias>/RW/state/set`:

- `ON`,
- `OFF`,
- `RESTART`,
- `STATUS`.

Nie używać `SWITCH` ani `ZMIEN_STAN` na topicach MQTT Tuya.

## OAuth MCP - wspólny model

MCP jest wystawiane przez reverse proxy i auth proxy. Klient najpierw przechodzi OAuth, potem łączy się z właściwym endpointem MCP.

Typowe elementy:

| Element | Rola |
| --- | --- |
| `auth_proxy.py` | obsługa OAuth i proxy do backendu MCP |
| `OAUTH_PASSWORD` | hasło/autoryzacja w `.env`, wartość `<ukryte>` |
| `PUBLIC_BASE_URL` | publiczny URL widziany przez klienta |
| `PATH_PREFIX` | prefiks ścieżki w Caddy, np. `/codex` |
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
- [ ] Zaktualizować `MQTT_PASSWORD=<ukryte>` w `.env` bridge i panelu.
- [ ] Zrestartować zależne kontenery po rotacji MQTT.

## Jawny dostęp do Raspberry Pi

> Uwaga: to jest jawnie zapisany sekret. Docelowo najlepiej przenieść go do managera haseł i w tej notatce zostawić tylko informację, gdzie go znaleźć.

| Element | Wartość |
| --- | --- |
| Użytkownik | `admin` |
| Hasło | `wiktor123` |
| Dotyczy | Raspberry Pi / dostęp administracyjny |
| Dodano | 2026-08-24 |

## Jawny OAuth do MCP

> Uwaga: to jest jawnie zapisany sekret OAuth. Docelowo najlepiej przenieść go do managera haseł, a po rotacji zostawić tutaj tylko informację o lokalizacji.

| Element | Wartość |
| --- | --- |
| Klient | `chatgpt` |
| OAuth / hasło MCP | `wtl1F5A6a8UhTgDbrN7YVH` |
| Dotyczy | MCP / auth proxy |
| Dodano | 2026-08-24 |
