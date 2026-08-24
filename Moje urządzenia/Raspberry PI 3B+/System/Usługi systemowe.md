---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - systemd
  - uslugi
updated: 2026-08-24
---

# Usługi systemowe

## Potwierdzone usługi

Odczyt z 2026-07-20 wskazywał, że na Raspberry Pi działają albo są skonfigurowane między innymi:

- Docker,
- Cockpit socket,
- SSH,
- NetworkManager,
- Avahi,
- Bluetooth,
- Performance Co-Pilot,
- usługi Codex/Obsidian/MCP,
- Caddy jako reverse proxy,
- kontenery Docker z automatyki domowej.

## Cockpit

| Element | Stan z audytu |
| --- | --- |
| `cockpit.socket` | `enabled`, aktywny, nasłuch na `[::]:9090` |
| `cockpit.service` | usługa aktywowana na żądanie przez socket |
| Rola | panel administracji systemem: pakiety, storage, sieć, usługi, logi |

Cockpit nie jest zamiennikiem Home Assistanta i Home Assistant nie jest zamiennikiem Cockpita. HA może przejąć automatyzacje i dashboard, ale nie zastępuje pełnego panelu OS/admin.

## Docker

Docker jest podstawą obecnej architektury. Najważniejszy znany stos znajduje się w:

```text
/opt/automation/docker-compose.yml
```

Dodatkowe projekty działają też w katalogach użytkownika `admin`, między innymi dla Tuya, panelu sterowania i MCP.

## Caddy i proxy

Caddy obsługuje porty `80/443` oraz publiczne ścieżki. Przy dodawaniu Home Assistanta nie należy uruchamiać osobnego serwera na tych portach. Zamiast tego trzeba dodać albo zmienić routing w Caddy.

## Co sprawdzać po zmianach

- Czy `docker` nadal działa.
- Czy `cockpit.socket` nadal nasłuchuje na `9090`.
- Czy Caddy nie ma błędów konfiguracji.
- Czy usługi MCP odpowiadają na właściwych ścieżkach.
- Czy nie wzrosło zużycie RAM/swap po uruchomieniu Home Assistanta.
