---
tags:
  - typ/dokumentacja
updated: 2026-10-20
---

# MCP MQTT i Tuya

Warstwa MCP łączy się z brokerem MQTT oraz mostkiem Tuya. Katalog encji nie stanowi dowodu bieżącej osiągalności fizycznych gniazdek.

Katalog widoczny we wtyczce Home Server MCP na 20.10.2026: **5 narzędzi**. Nazwy poniżej pomijają techniczny prefiks dostawcy mcp__home_server__.

| Narzędzie | Tryb | Opis |
| --- | --- | --- |
| `mqtt__list_entities` | Odczyt | Lista skonfigurowanych encji oraz ostatnich stanów z pamięci. |
| `mqtt__get_entity_state` | Odczyt | Ostatni znany stan jednej encji wskazanej przez entity_id. |
| `mqtt__refresh_states` | Operacja wymagająca zgody / home:write | Aktywne odpytanie mostka Tuya i zwrot odświeżonej pamięci stanów. |
| `mqtt__publish_mqtt` | Operacja wymagająca zgody / home:write | Publikacja payloadu wyłącznie na temat z listy dozwolonej; opcjonalne retain. |
| `mqtt__set_tuya_plug` | Operacja wymagająca zgody / home:write | Wysłanie komendy do aliasu gniazdka; kontrakt deklaruje ON, OFF, RESTART, STATUS, SWITCH, ZMIEN_STAN. |

## Obsługa i ograniczenia

Aliasy: gniazdo1 — lampa akwariowa; gniazdo2 — lampka akwariowa 2 (MQTT raportuje „Lampka nocna”); gniazdo3 — lampka biurkowa. Dla aliasu all kontrakt set_tuya_plug dopuszcza tylko ON, OFF, RESTART, STATUS.

Tematy: stan `tuya/<alias>/RW/state`, komenda `tuya/<alias>/RW/state/set`, dostępność `tuya/<alias>/RO/availability`. Komendy publikuj bez retain. Starsza dokumentacja mostka potwierdza jedynie ON/OFF/RESTART/STATUS; SWITCH i ZMIEN_STAN są zadeklarowane w MCP, ale ich działanie na aktualnym mostku nie zostało sprawdzone. Nie traktuj deklaracji schematu jako potwierdzenia obsługi przez urządzenie.

W teście 20.10.2026 pamięć wskazywała gniazdo1 i gniazdo3 offline z Device Unreachable, a gniazdo2 online. Nie wywoływano refresh_states ani komend sterujących.

Zmiany danych i sterowanie wymagają wyraźnego polecenia użytkownika oraz zgody OAuth home:write. Po niepewnym wyniku zapisu najpierw sprawdź stan, zamiast automatycznie ponawiać operację.

## Powiązania

- [[Moje urządzenia/Win 10 Home Automation/Lista indeksów|Dokumentacja serwera Windows 10]]
- [[Moje urządzenia/Win 10 Home Automation/Migracja RPi na Windows 10|Status migracji]]
- [[Moje urządzenia/Win 10 Home Automation/Testy MCP 2026-10-20|Wyniki testów]]
