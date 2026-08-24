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

Sekcja do dokumentowania konfiguracji Raspberry PI 3B+: system, dostęp, usługi, Docker, automatyka domowa, MQTT/Tuya, Home Assistant, backup, OAuth/MCP, kontenery i rzeczy do poprawy.

## Status danych

- Ostatni potwierdzony odczyt systemu i usług: 2026-07-20.
- Ostatni potwierdzony odczyt MQTT/MCP/Tuya: 2026-07-16.
- Próby świeżego odczytu 2026-08-24 nie zwróciły raportu z workera, więc część parametrów ma status `do odświeżenia`.
- W notatkach nie zapisuję jawnych haseł, tokenów, kluczy prywatnych ani wartości z plików `.env`.

## Główne notatki

### [[Moje urządzenia/Raspberry PI 3B+/01 - Metryczka urządzenia|Metryczka urządzenia]]

Podstawowe informacje o sprzęcie, systemie, zasobach i aktualnych ograniczeniach.

### [[Moje urządzenia/Raspberry PI 3B+/02 - Sieć i dostęp|Sieć i dostęp]]

Porty, punkty dostępu, Caddy, publiczne ścieżki i rzeczy do odświeżenia w sieci.

### [[Moje urządzenia/Raspberry PI 3B+/03 - Usługi systemowe|Usługi systemowe]]

Najważniejsze usługi systemd: SSH, Docker, Cockpit, NetworkManager, Avahi, Bluetooth, Caddy, Codex/Obsidian/MCP.

### [[Moje urządzenia/Raspberry PI 3B+/04 - Docker i automatyka domowa|Docker i automatyka domowa]]

Stos `/opt/automation`, kontenery, ścieżki projektów i stan usług automatyki.

### [[Moje urządzenia/Raspberry PI 3B+/05 - MQTT i Tuya|MQTT i Tuya]]

Mosquitto, topic patterny, aliasy gniazdek Tuya i zasady bezpiecznego sterowania.

### [[Moje urządzenia/Raspberry PI 3B+/06 - Home Assistant i migracja|Home Assistant i migracja]]

Stan Home Assistanta, ryzyko RAM, integracja z MQTT i plan migracji z panelu Tuya.

### [[Moje urządzenia/Raspberry PI 3B+/07 - Backup i bezpieczeństwo|Backup i bezpieczeństwo]]

Katalogi do kopii zapasowych, sekrety, rotacja haseł i zasady przed zmianami.

### [[Moje urządzenia/Raspberry PI 3B+/08 - TODO i ryzyka|TODO i ryzyka]]

Lista kolejnych kroków oraz miejsc wymagających aktualnego audytu.

### [[Moje urządzenia/Raspberry PI 3B+/09 - Źródła odczytu|Źródła odczytu]]

Skąd pochodzą dane w tej sekcji i które odczyty są historyczne.

### [[Moje urządzenia/Raspberry PI 3B+/10 - Dostępy i OAuth MCP|Dostępy i OAuth MCP]]

Mapa dostępów, użytkowników, lokalizacji sekretów, OAuth MCP, Caddy, Cloudflare i ścieżek auth proxy. Bez jawnych haseł.

### [[Moje urządzenia/Raspberry PI 3B+/11 - Kontenery skonfigurowane|Kontenery skonfigurowane]]

Szczegółowa rozpiska kontenerów Docker, compose, portów, mountów, env bez sekretów i ryzyk.

## Najważniejsze rzeczy do pamiętania

- Bazą jest Debian + Docker, a nie Home Assistant OS.
- Cockpit pełni rolę panelu administracji systemem i nie powinien być usuwany przed stabilną migracją do Home Assistanta.
- Mosquitto jest centralnym brokerem MQTT.
- Hasło MQTT jest używane przez bridge, panel i connector; wartości nie zapisywać w Obsidianie.
- OAuth MCP przechodzi przez auth proxy i Caddy; poprawny publiczny MQTT MCP endpoint to `https://wikingv.servehalflife.com/mqtt-mcp/mcp`.
- `tuya-mqtt-bridge` obsługuje lokalne gniazdka Tuya przez LAN.
- `mqtt-mcp-connector` wystawia kontrolę MQTT/MCP, ale komendy sterujące należy wykonywać ostrożnie.
- Home Assistant był zatrzymany z kodem `Exited (137)`, więc problem RAM/swap jest najważniejszym ryzykiem.

## Najbliższy rozwój

- Odświeżyć pełny audyt urządzenia, gdy worker na Pi zacznie zwracać raporty.
- Uzupełnić lokalny adres IP, kernel, uptime, partycje i aktualny stan kontenerów.
- Zrobić backup przed kolejnymi zmianami w `/opt/automation` i usługach MQTT/Tuya.
- Dopiąć integrację MQTT w Home Assistant albo dodać MQTT Discovery w `tuya-mqtt-bridge`.
- Przenieść jawne sekrety z notatek do managera haseł i rozważyć rotację OAuth/MQTT.
