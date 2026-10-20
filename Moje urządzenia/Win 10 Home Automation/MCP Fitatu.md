---
tags:
  - typ/dokumentacja
updated: 2026-10-20
---

# MCP Fitatu

Integracja z dziennikiem żywienia Fitatu. Obejmuje odczyt danych, obsługę posiłków, przepisów i pomiarów ciała.

Katalog widoczny we wtyczce Home Server MCP na 20.10.2026: **19 narzędzi**. Nazwy poniżej pomijają techniczny prefiks dostawcy mcp__home_server__.

| Narzędzie | Tryb | Opis |
| --- | --- | --- |
| `fitatu__get_current_user` | Odczyt | Odczyt profilu zalogowanego użytkownika. |
| `fitatu__get_body_measurement` | Odczyt | Odczyt pomiarów ciała dla daty lub ostatniego wpisu. |
| `fitatu__get_user_settings` | Odczyt | Odczyt celów energii i ustawień wody. |
| `fitatu__get_day_plan_items` | Odczyt | Odczyt posiłków i konkretnych wpisów z itemId oraz mealKey. |
| `fitatu__get_diet_summary` | Odczyt | Podsumowanie kalorii i składników w zakresie dat. |
| `fitatu__get_recipe` | Odczyt | Szczegóły przepisu, składniki, porcje i dostępne miary. |
| `fitatu__search_food` | Odczyt | Wyszukiwanie produktów i przepisów; zwraca identyfikatory i miary. |
| `fitatu__search_food_by_barcodes` | Odczyt | Wyszukiwanie produktów po kodach GTIN, maksymalnie 10 kodów. |
| `fitatu__search_recipes` | Odczyt | Wyszukiwanie aktywnych przepisów po nazwie. |
| `fitatu__add_meal_items` | Operacja wymagająca zgody / home:write | Dodanie produktów lub przepisów do posiłku; własny wpis tylko jako wariant zastępczy. |
| `fitatu__move_meal_item` | Operacja wymagająca zgody / home:write | Przeniesienie wpisu do innego posiłku lub dnia; zwraca nowy itemId. |
| `fitatu__remove_meal_items` | Operacja wymagająca zgody / home:write | Usunięcie konkretnych wpisów wskazanych przez mealKey i itemId. |
| `fitatu__replace_meal_item` | Operacja wymagająca zgody / home:write | Zastąpienie wpisu innym produktem, przepisem lub wpisem własnym. |
| `fitatu__update_meal_item` | Operacja wymagająca zgody / home:write | Zmiana ilości, miary, oznaczenia zjedzenia lub danych wpisu własnego. |
| `fitatu__create_recipe` | Operacja wymagająca zgody / home:write | Utworzenie przepisu ze składników, liczby porcji i kroków przygotowania. |
| `fitatu__update_recipe` | Operacja wymagająca zgody / home:write | Częściowa aktualizacja własnego przepisu; należy używać zwróconego recipeId. |
| `fitatu__delete_recipe` | Operacja wymagająca zgody / home:write | Miękkie usunięcie własnego przepisu po potwierdzeniu dokładnej nazwy; historia posiłków pozostaje. |
| `fitatu__save_body_measurement` | Operacja wymagająca zgody / home:write | Zapis pomiarów dla jawnej daty; pominięte wartości pozostają bez zmian. |
| `fitatu__update_user_settings` | Operacja wymagająca zgody / home:write | Zmiana celu kalorii i makroskładników lub wielkości porcji wody. |

## Obsługa i ograniczenia

W operacjach na jadłospisie kopiuj mealKey i itemId z get_day_plan_items. productId/recipeId identyfikują definicję jedzenia, nie konkretny wpis do usunięcia. Wybieraj miary zwrócone przez wyszukiwarkę. Przeniesienie lub zastąpienie może nadać nowe itemId.

Zmiany danych i sterowanie wymagają wyraźnego polecenia użytkownika oraz zgody OAuth home:write. Po niepewnym wyniku zapisu najpierw sprawdź stan, zamiast automatycznie ponawiać operację.

## Powiązania

- [[Moje urządzenia/Win 10 Home Automation/Lista indeksów|Dokumentacja serwera Windows 10]]
- [[Moje urządzenia/Win 10 Home Automation/Migracja RPi na Windows 10|Status migracji]]
- [[Moje urządzenia/Win 10 Home Automation/Testy MCP 2026-10-20|Wyniki testów]]
