# Hevy Coach — stan wdrożenia

Aktualizacja: 2026-09-12, 10:23 Europe/Warsaw. Etapy 0–6 ukończone. Użytkownik zaakceptował aktywację.
Harmonogram AKTYWNY: production_enabled=true, oba timery enabled/active.
Najbliższy plan: 2026-09-12 o 18:00. Najbliższy raport: 2026-09-19 o 09:00.
Strefa Europe/Warsaw; harmonogram przetrwa wylogowanie i restart dzięki enabled oraz Linger=yes.

## Ukończone etapy
- [x] 0: rozpoznanie; dokumentacja lokalnie, na RPi i w Obsidianie.
- [x] 1: live Hevy READ, trwała pamięć, planner PPL, źródła Obsidiana.
- [x] 2: MCP hevy_coach w Codex na RPi; pięć narzędzi, restart, idempotencja i konflikty.
- [x] 3: podgląd dzienny; ntfy dotarło do aplikacji — potwierdzenie użytkownika.
- [x] 4: pełny raport AI, zapis w Obsidianie, odczyt przez MCP; test powiadomienia przyjęty przez ntfy.
- [x] 5: 29 testów lokalnie i na RPi; wyłączone usługi nie wysyłają; brak pozostałych procesów ręcznych.
- [x] 6: użytkownik zaakceptował przykłady i aktywację; oba timery włączone, następne terminy zweryfikowane.

## HEVY MCP STATUS
Node.js: v22.23.1
npm/npx: 10.9.8
Codex CLI: 0.153.4
MCP hevy: działa, stdio
MCP hevy_coach: działa, pięć narzędzi, rozpoznany przez Codex
Hevy API: autoryzacja i odczyt poprawne
health-check: ok
Liczba treningów: 142
Ostatni trening: Morning workout ☀️ — 2026-08-04, 10:05 Europe/Warsaw
Narzędzia analityczne: porównanie okresów, regularność, objętość, progres, pomiary — odczyty bez błędów

## Pamięć i podglądy
Użytkownik potwierdził kompletność historii 2026-09-12. Stan.md revision 1.
history_confirmed_at: 2026-09-12T07:54:03.646080+00:00.
Podgląd dzienny z 10:10: Pull, istniejąca routine PULL ⛴ THE ⛴ BOATS - V2.
Po potwierdzeniu historii coach nie pyta o to ponownie; zaznacza powrót po długiej przerwie.
Raport odświeżony z pamięcią revision 1; ostatnie 7 i 28 dni bez treningów, rekompozycja obecnie nieocenialna.
Siedem linków do materiałów źródłowych zweryfikowanych.
Folder Obsidiana: Zdrowie, uroda i siłownia/Trening/Siłownia/Coach Hevy.
Lokalne podglądy: var/review/2026-09-12-raport.md i var/review/2026-09-12-plan.md.

## Powiadomienia — NIE POWTARZAĆ
test-daily-2026-09-12: sent, id GeFbPdm569Lh; odbiór potwierdzony przez użytkownika.
test-weekly-2026-09-12: sent, id wjPV3eE0m1W2; przyjęcie przez serwer potwierdzone.
Korekta raportu i planu została zapisana BEZ kolejnej wysyłki.

## Walidacja
29 testów przeszło w Windows i na RPi (włącznie z ResourceWarning jako błędem).
Izolowane testy obejmują przełożenie przez stdio, restart procesu i odczyt pamięci, powtórzenie zapisu, konflikt wersji, zmianę stanu podczas AI, ograniczenie kontekstu, heurystyki planowania i deduplikację.
Rzeczywisty odczyt coach_status oraz coach_latest_report przez MCP działa.
Konfiguracja Codex po odjęciu nowego hevy_coach jest identyczna z kopią sprzed zmiany.
Obie usługi uruchomione z production_enabled=false zakończyły się poprawnie, bez zmiany pokwitowań ntfy.
Linger=yes, jednostki systemd zweryfikowane. Nie wykonywano fizycznego restartu RPi.
Klucz Hevy pozostał poza projektem na RPi, uprawnienia 600. Brak nowych publicznych portów.
Źródłowe notatki, inne MCP i wszystkie dane Hevy pozostają bez zmian.

## Tokeny i wznowienie
Pierwszy raport: wejście 55981, wyjście 2385.
Poprawiony raport: wejście 36326, wyjście 2279; o ok. 35% mniej wejścia.
To użycie samej analizy raportu, nie suma całej rozmowy/wdrożenia.
W tym wznowieniu nie wystąpił kolejny błąd limitu; aktywacja została zakończona.
W razie przerwy przeczytaj WZNOWIENIE.md; nie ponawiaj ukończonych testów ntfy.
Kopie przed aktualizacjami: backups/<release>. Przetestowany kod: release 20260912-100531; późniejszy release uzupełnia dokumentację.

## Aktywacja i dalsza obsługa
Aktywacja: 2026-09-12T10:23:15+02:00, po jednoznacznym „tak jest” użytkownika.
Kopia konfiguracji: backups/config-before-activation-20260912-102310.json (600).
Potwierdzenie: var/activation.json. Zmieniono wyłącznie production_enabled i stan dwóch dedykowanych timerów.
Plan codziennie 18:00, raport sobota 09:00, Europe/Warsaw.
Przy aktywacji nie wysłano żadnych powiadomień i nie powtórzono dzisiejszego raportu.
Nie aktywować ponownie ani nie wykonywać dodatkowych testów wysyłki podczas wznowienia.
Pierwsza produkcyjna wysyłka ma nastąpić w terminie harmonogramu; w tej sesji nie oczekiwano do 18:00.
Wyłączenie na życzenie: disable --now dwóch timerów, potem production_enabled=false; zachować pamięć i pokwitowania.

## Ograniczenia wersji
Pełne sterowanie jest dostępne w Codex CLI na RPi. Zwykły ChatGPT i Windows nie mają automatycznie podłączonego tego MCP.
Propozycje zmian programu wymagają oceny użytkownika; ta wersja nie zapisuje routines w Hevy.
Planner używa jawnych heurystyk, nie mierzy regeneracji. Źródłowe materiały mogą zawierać błędy, które raport oznacza.
