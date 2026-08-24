---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - docker
  - kontenery
  - operacyjne
updated: 2026-08-24
---

# Uruchomione kontenery i role

## Status danych

Ta notatka jest operacyjną mapą kontenerów: co powinno działać i po co. Opiera się na ostatnich potwierdzonych odczytach z lipca 2026. Bieżący stan trzeba odświeżyć poleceniem typu `docker ps -a`, bo próby świeżego audytu 2026-08-24 nie zwróciły raportu workera.

Pełna techniczna rozpiska compose, portów, mountów i env bez sekretów jest w: [[Moje urządzenia/Raspberry PI 3B+/Docker i kontenery/Kontenery skonfigurowane|Kontenery skonfigurowane]].

## Najważniejsze uruchomione kontenery

| Kontener | Po co działa | Dostęp / port | Zależności | Uwagi operacyjne |
| --- | --- | --- | --- | --- |
| `automation-mosquitto` | Centralny broker MQTT dla automatyki, Tuya, MCP, Node-RED i przyszłego Home Assistanta | `1883` MQTT, `9001` WebSocket | plik hasła Mosquitto, sieć Docker/host | Krytyczny. Bez niego bridge, panel i MCP nie mają wspólnego busa. |
| `automation-node-red` | Node-RED do automatyzacji i ewentualnych flow | `127.0.0.1:1880`, przez Caddy jako `/nodered*` | `automation-mosquitto` | Ostatnio działał jako `healthy`, ale audyt nie widział realnych flow. |
| `tuya-mqtt-bridge` | Lokalny most Tuya LAN -> MQTT; czyta gniazdka i publikuje ich stan | `network_mode: host`, bez osobnego UI | Mosquitto, konfiguracja Tuya lokalna | Krytyczny dla gniazdek Tuya. Nie zapisuje sekretów w notatkach. |
| `tuya-control-panel` | Prosty panel WWW do sterowania gniazdkami Tuya przez MQTT | `0.0.0.0:8090` | Mosquitto, topic prefix `tuya` | Działał na Flask development serverze; docelowo może zostać zastąpiony przez Home Assistanta. |
| `mqtt-mcp-connector` | MCP do odczytu i kontrolowania MQTT/Tuya z ChatGPT/Codex | `0.0.0.0:8092/mcp` lokalnie, publicznie przez `/mqtt-mcp/mcp` | Mosquitto, auth proxy, Caddy | Używać ostrożnie; komendy `ON/OFF/SWITCH` faktycznie sterują gniazdkami. |
| `obsidian-mcp` | MCP do vaulta Obsidian: wyszukiwanie, czytanie i edycja notatek | `0.0.0.0:8765->8000` oraz auth proxy | vault Obsidian, git backup | Używane do pracy na notatkach. |
| `codex-mcp` | MCP do zlecania i sprawdzania zadań Codex workera na Raspberry Pi | `8000/tcp` wewnątrz sieci Docker | katalog `codex-worker` | Dzięki temu można uruchamiać audyty i prace na RPi jako zadania. |
| `obsidian-mcp-auth` | OAuth/auth proxy dla Obsidian MCP | przez Caddy na ścieżkach Obsidian MCP | `obsidian-mcp`, `.env` z OAuth | Chroni dostęp do Obsidian MCP. |
| `codex-mcp-auth` | OAuth/auth proxy dla Codex MCP | `/codex/*` | `codex-mcp`, `.env` z OAuth | Chroni dostęp do zadań Codex workera. |
| `mqtt-mcp-auth` | OAuth/auth proxy dla MQTT MCP | `/mqtt-mcp/*` | `mqtt-mcp-connector`, `.env` z OAuth | Publiczny endpoint końcowy: `https://wikingv.servehalflife.com/mqtt-mcp/mcp`. |
| `obsidian-mcp-caddy` | HTTPS reverse proxy dla usług MCP, Node-RED, MQTT WebSocket i fallbacku do Cockpit | `80`, `443` | Caddyfile, Cloudflare/tunel | Centralny punkt routingu publicznego. |
| `obsidian-mcp-tunnel` | Tunel Cloudflare do publicznego dostępu | Cloudflare Tunnel | token w `.env` | Nie notować tokena w Obsidianie. |

## Kontenery skonfigurowane, ale niekoniecznie uruchomione

| Kontener | Po co jest | Ostatni znany stan | Co zrobić przed startem |
| --- | --- | --- | --- |
| `automation-home-assistant` | Docelowy dashboard i silnik automatyzacji domowej | `Exited (137)` w audycie 2026-07-20 | Zrobić backup, sprawdzić RAM/swap, uruchomić ostrożnie i dodać MQTT. |

## Zależności w praktyce

```text
Tuya gniazdka
  -> tuya-mqtt-bridge
  -> automation-mosquitto
  -> tuya-control-panel / mqtt-mcp-connector / przyszły Home Assistant
```

```text
ChatGPT / Codex z zewnątrz
  -> Cloudflare Tunnel
  -> Caddy
  -> auth proxy OAuth
  -> backend MCP
```

```text
Obsidian notes
  -> obsidian-mcp
  -> auth proxy
  -> Caddy / Cloudflare
```

## Co aktualizować po każdym audycie

- [ ] Czy kontener jest `Up`, `Exited`, `Restarting` albo `Dead`.
- [ ] Od kiedy działa (`StartedAt`) i ile ma restartów.
- [ ] Czy ma healthcheck i jaki jest status.
- [ ] Jakie porty są widoczne na `0.0.0.0`, `127.0.0.1` albo tylko w sieci Docker.
- [ ] Czy zmieniła się rola kontenera lub ścieżka publiczna w Caddy.
- [ ] Czy kontener dostał nowe sekrety w `.env` - wartości nadal trzymać poza notatkami.

## Szybkie zasady

- Mosquitto, bridge Tuya, panel/MCP i Caddy traktować jako elementy jednego stosu automatyki.
- Nie testować komend sterujących MQTT bez świadomego wyboru konkretnego gniazdka.
- Nie odpalać Home Assistanta bez obserwacji RAM/swap na Pi 3B+.
- Po każdej większej zmianie dopisać tu krótką notatkę: data, kontener, co się zmieniło, czy był restart.
