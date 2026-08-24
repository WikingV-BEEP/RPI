---
tags:
  - indeks
  - moje-urzadzenia
  - raspberry-pi
  - raspberry-pi-3b-plus
  - rpi
updated: 2026-08-24
---

# Raspberry PI 3B+

## Zakres folderu

Sekcja do dokumentowania konfiguracji Raspberry PI 3B+: system, sieć, dostęp, Docker, kontenery, automatyka domowa, MQTT/Tuya, Home Assistant, backup, OAuth/MCP i utrzymanie.

## Status danych

- Ostatni potwierdzony odczyt systemu i usług: 2026-07-20.
- Ostatni potwierdzony odczyt MQTT/MCP/Tuya: 2026-07-16.
- Próby świeżego odczytu 2026-08-24 nie zwróciły raportu z workera, więc część parametrów ma status `do odświeżenia`.
- W tej sekcji są świadomie zapisane jawne sekrety dostępu do RPi i OAuth MCP. Przy kolejnej porządkowej rundzie najlepiej przenieść je do managera haseł.

## Foldery

### [[Moje urządzenia/Raspberry PI 3B+/System/Lista indeksów|System]]

Sprzęt, system operacyjny, zasoby i usługi systemowe.

### [[Moje urządzenia/Raspberry PI 3B+/Dostęp i sieć/Lista indeksów|Dostęp i sieć]]

Punkty dostępu, porty, OAuth/MCP, Caddy, Cloudflare, MQTT login i jawnie dopisane dane dostępu.

### [[Moje urządzenia/Raspberry PI 3B+/Docker i kontenery/Lista indeksów|Docker i kontenery]]

Mapa działających kontenerów, role, compose, porty, mounty i ryzyka.

### [[Moje urządzenia/Raspberry PI 3B+/Automatyka domowa/Lista indeksów|Automatyka domowa]]

MQTT, Tuya, Home Assistant i plan migracji automatyki.

### [[Moje urządzenia/Raspberry PI 3B+/Utrzymanie/Lista indeksów|Utrzymanie]]

Backup, TODO, ryzyka, źródła odczytu i procedury aktualizacji notatek.

## Najważniejsze rzeczy do pamiętania

- Bazą jest Debian + Docker, a nie Home Assistant OS.
- Cockpit pełni rolę panelu administracji systemem i nie powinien być usuwany przed stabilną migracją do Home Assistanta.
- Mosquitto jest centralnym brokerem MQTT.
- OAuth MCP przechodzi przez auth proxy i Caddy; poprawny publiczny MQTT MCP endpoint to `https://wikingv.servehalflife.com/mqtt-mcp/mcp`.
- `tuya-mqtt-bridge` obsługuje lokalne gniazdka Tuya przez LAN.
- `mqtt-mcp-connector` wystawia kontrolę MQTT/MCP, ale komendy sterujące należy wykonywać ostrożnie.
- Home Assistant był zatrzymany z kodem `Exited (137)`, więc problem RAM/swap jest najważniejszym ryzykiem.

## Najbliższy rozwój

- Odświeżyć pełny audyt urządzenia, gdy worker na Pi zacznie zwracać raporty.
- Uzupełnić lokalny adres IP, kernel, uptime, partycje i aktualny stan kontenerów.
- Zrobić backup przed kolejnymi zmianami w `/opt/automation` i usługach MQTT/Tuya.
- Dopiąć integrację MQTT w Home Assistant albo dodać MQTT Discovery w `tuya-mqtt-bridge`.
- Po każdej zmianie w Dockerze aktualizować [[Moje urządzenia/Raspberry PI 3B+/Docker i kontenery/Uruchomione kontenery i role|Uruchomione kontenery i role]].
