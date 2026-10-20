---
tags:
  - typ/dokumentacja
updated: 2026-10-20
---

# MCP Obsidian

Warstwa MCP udostępnia vault RaspberryPiHome. Nazwa vaulta i linuksowe ścieżki w kontenerach mogą pozostać po migracji; nie oznaczają, że usługa nadal działa na RPi.

Katalog widoczny we wtyczce Home Server MCP na 20.10.2026: **13 narzędzi**. Nazwy poniżej pomijają techniczny prefiks dostawcy mcp__home_server__.

| Narzędzie | Tryb | Opis |
| --- | --- | --- |
| `obsidian__search` | Odczyt | Wyszukiwanie notatek po słowach kluczowych. |
| `obsidian__fetch` | Odczyt | Odczyt pełnej notatki po identyfikatorze. |
| `obsidian__list_dir` | Odczyt | Bezpośrednia zawartość folderu; pusta ścieżka oznacza katalog główny. |
| `obsidian__tree` | Odczyt | Drzewo folderów i notatek. |
| `obsidian__list_notes` | Odczyt | Rekurencyjna lista identyfikatorów notatek bez czytania treści. |
| `obsidian__file_info` | Odczyt | Rozmiar i metadane pliku lub folderu. |
| `obsidian__batch_file_info` | Odczyt | Metadane maksymalnie 100 plików lub folderów. |
| `obsidian__create_note` | Operacja wymagająca zgody / home:write | Utworzenie nowej notatki. |
| `obsidian__create_folder` | Operacja wymagająca zgody / home:write | Utworzenie nowego folderu. |
| `obsidian__append_note` | Operacja wymagająca zgody / home:write | Dopisanie tekstu do istniejącej notatki; wcześniej kopia Git. |
| `obsidian__write_note` | Operacja wymagająca zgody / home:write | Zastąpienie treści istniejącej notatki; wcześniej kopia Git. |
| `obsidian__delete_note` | Operacja wymagająca zgody / home:write | Usunięcie notatki; wcześniej kopia Git. |
| `obsidian__delete_folder` | Operacja wymagająca zgody / home:write | Rekurencyjne usunięcie folderu; wcześniej kopia Git. |

## Obsługa i ograniczenia

Kopie Git są deklarowane przed modyfikacją/usunięciem istniejących danych. create_note i create_folder nie tworzą kopii wcześniejszej treści. Po zapisie odczytaj notatkę i sprawdź treść. Nie zapisuj haseł, tokenów ani kluczy API w dokumentacji.

Zmiany danych i sterowanie wymagają wyraźnego polecenia użytkownika oraz zgody OAuth home:write. Po niepewnym wyniku zapisu najpierw sprawdź stan, zamiast automatycznie ponawiać operację.

## Powiązania

- [[Moje urządzenia/Win 10 Home Automation/Lista indeksów|Dokumentacja serwera Windows 10]]
- [[Moje urządzenia/Win 10 Home Automation/Migracja RPi na Windows 10|Status migracji]]
- [[Moje urządzenia/Win 10 Home Automation/Testy MCP 2026-10-20|Wyniki testów]]
