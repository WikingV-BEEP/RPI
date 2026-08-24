---
tags:
  - moje-urzadzenia
  - raspberry-pi
  - backup
  - bezpieczenstwo
updated: 2026-08-24
---

# Backup i bezpieczeństwo

## Katalogi do backupu przed zmianami

| Ścieżka | Dlaczego ważna |
| --- | --- |
| `/opt/automation` | główny stos Docker: Mosquitto, Home Assistant, Node-RED |
| `/opt/automation/docker-compose.yml` | definicja usług automatyki |
| `/opt/automation/mosquitto` | konfiguracja, hasła i dane brokera MQTT |
| `/opt/automation/home-assistant/config` | konfiguracja Home Assistanta, baza i `.storage` |
| `/opt/automation/node-red` | dane Node-RED |
| `/home/admin/tuya-mqtt-bridge` | konfiguracja i kod bridge Tuya/MQTT |
| `/home/admin/tuya-control-panel` | panel Flask do sterowania Tuya |
| `/home/admin/mqtt-mcp-connector` | connector MCP do MQTT/Tuya |
| `/home/admin/obsidian-mcp` | proxy/auth/obsidian/codex MCP i konfiguracja tras |
| pliki `.env` | wartości sekretów, których nie wolno przepisywać do notatek |

## Zasada notowania sekretów

W Obsidianie zapisywać tylko nazwy ustawień, nigdy wartości sekretów.

Przykład poprawnego zapisu:

```text
MQTT_USERNAME=<ustawione>
MQTT_PASSWORD=<ukryte>
TUYA_DEVICE_KEY=<ukryte>
```

## Rotacja sekretów

Do zrobienia: rozważyć rotację hasła MQTT i innych sekretów automatyki. W historii technicznej zadań pojawił się stdout z wartością hasła MQTT, więc lepiej potraktować je jako ujawnione i wymienić.

## Bezpieczna kolejność zmian

1. Backup katalogów i plików `.env`.
2. Sprawdzenie, czy backup da się odczytać.
3. Walidacja konfiguracji Docker/Caddy bez restartu.
4. Restart tylko jednej usługi naraz.
5. Sprawdzenie logów, portów, RAM i swap.
6. Dopiero potem kolejna zmiana.

## Punkty kontroli po backupie

- Czy backup zawiera pliki ukryte, np. `.storage`, `.env`, `.conf`.
- Czy backup nie jest trzymany wyłącznie na tej samej karcie SD.
- Czy hasła i tokeny są w backupie chronione.
- Czy znana jest procedura odtworzenia Mosquitto i HA.

## Dostępy i OAuth

Mapa kont, endpointów OAuth/MCP, lokalizacji sekretów i zależności MQTT jest w: [[Moje urządzenia/Raspberry PI 3B+/Dostęp i sieć/Dostępy i OAuth MCP|Dostępy i OAuth MCP]]. Jawnych haseł nie powielać w innych notatkach.
