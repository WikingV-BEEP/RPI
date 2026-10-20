---
tags:
  - typ/dokumentacja
updated: 2026-10-20
---

# MCP Hevy Coach

Warstwa trenera korzysta z danych treningowych Hevy. Stan, ustalenia i raporty przechowuje w Obsidianie. coach_plan nie uruchamia raportu AI ani Codex.

Katalog widoczny we wtyczce Home Server MCP na 20.10.2026: **5 narzędzi**. Nazwy poniżej pomijają techniczny prefiks dostawcy mcp__home_server__.

| Narzędzie | Tryb | Opis |
| --- | --- | --- |
| `hevy__coach_status` | Odczyt | Odczyt stanu, rewizji, ustaleń i aktywacji trenera. |
| `hevy__coach_plan` | Odczyt | Pobranie bieżących danych Hevy i wyznaczenie planu PPL lub odpoczynku; bez zapisu i powiadomień. |
| `hevy__coach_knowledge` | Odczyt | Odczyt wiedzy treningowej z dozwolonego folderu Obsidiana. |
| `hevy__coach_latest_report` | Odczyt | Odczyt ostatniego raportu tygodniowego. |
| `hevy__coach_update` | Operacja wymagająca zgody / home:write | Zapis w Obsidianie: przełożenie treningu, cofnięcie przełożenia, check-in lub deklaracja wykonania. Nie zapisuje do Hevy. |

## Obsługa i ograniczenia

Przed coach_update odczytaj coach_status, przekaż expected_revision oraz stabilny operation_id. Daty zapisuj jawnie w ISO. Zapis nie wysyła ntfy. Po zmianie planu odczytaj coach_plan. Dostępność narzędzi nie potwierdza wykonania harmonogramu; ostatni odczyt pokazał production_enabled=true, 18:00 codziennie i sobotę 09:00 Europe/Warsaw.

Zmiany danych i sterowanie wymagają wyraźnego polecenia użytkownika oraz zgody OAuth home:write. Po niepewnym wyniku zapisu najpierw sprawdź stan, zamiast automatycznie ponawiać operację.

## Powiązania

- [[Moje urządzenia/Win 10 Home Automation/Lista indeksów|Dokumentacja serwera Windows 10]]
- [[Moje urządzenia/Win 10 Home Automation/Migracja RPi na Windows 10|Status migracji]]
- [[Moje urządzenia/Win 10 Home Automation/Testy MCP 2026-10-20|Wyniki testów]]
