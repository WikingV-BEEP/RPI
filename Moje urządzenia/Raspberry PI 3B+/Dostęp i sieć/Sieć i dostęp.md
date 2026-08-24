---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - siec
  - dostep
updated: 2026-08-24
---

# Sieć i dostęp

## Status danych

Ta notatka opiera się na potwierdzonych odczytach z 2026-07-16 i 2026-07-20. Lokalne adresy IP, DNS, brama i pełna lista interfejsów wymagają świeżego audytu.

## Znane punkty dostępu

| Port / ścieżka | Bind / ekspozycja | Usługa | Uwagi |
| --- | --- | --- | --- |
| `22` | do potwierdzenia | SSH | SSH działa według audytu usług; port wpisać po świeżym odczycie. |
| `80`, `443` | publiczne przez Caddy | Caddy / reverse proxy | Caddy obsługuje publiczne ścieżki, więc nowe usługi nie powinny przejmować tych portów bez zmiany proxy. |
| `9090` | `[::]:9090` | Cockpit | Panel administracyjny systemu, socket aktywny i nasłuchujący. |
| `1883` | `0.0.0.0:1883` | Mosquitto MQTT | Broker dostępny w LAN. |
| `9001` | `0.0.0.0:9001` | Mosquitto | Dodatkowy port brokera, prawdopodobnie WebSocket. |
| `1880` | `127.0.0.1:1880` | Node-RED | Dostęp lokalny, nie bezpośrednio z LAN. |
| `8123` | `127.0.0.1:8123` w obecnym compose | Home Assistant | Kontener był zatrzymany, więc UI nie było aktywne. |
| `8090` | `0.0.0.0:8090` | `tuya-control-panel` | Lekki panel Flask do gniazdek Tuya. |
| `8092/mcp` | `0.0.0.0:8092` | `mqtt-mcp-connector` | Backend MCP do MQTT/Tuya. |
| `/mqtt-mcp/mcp` | publiczne przez Caddy/auth proxy | MQTT MCP | Poprawna publiczna ścieżka MCP z prefiksem. |

## Ważna uwaga o adresach

`0.0.0.0` oznacza nasłuch na wszystkich interfejsach hosta. Przy usługach sterujących urządzeniami domowymi warto sprawdzić, czy dostęp ma być tylko lokalny, tylko przez proxy z autoryzacją, czy przez cały LAN.

## Poprawna publiczna ścieżka MQTT MCP

Wcześniejsza diagnoza potwierdziła, że poprawna ścieżka publiczna dla MQTT MCP ma prefiks:

```text
https://wikingv.servehalflife.com/mqtt-mcp/mcp
```

Samo `/mcp` trafia gdzie indziej i nie jest właściwym adresem dla MQTT MCP.

## Do odświeżenia

- Aktualny lokalny adres IP Raspberry Pi.
- Aktualna brama domyślna i DNS.
- Interfejs używany jako główny: Ethernet czy Wi-Fi.
- Czy `8090`, `8092`, `1883` i `9001` mają być widoczne w całej sieci LAN.
- Czy publiczne trasy Caddy mają healthchecki i sensowne limity dostępu.
