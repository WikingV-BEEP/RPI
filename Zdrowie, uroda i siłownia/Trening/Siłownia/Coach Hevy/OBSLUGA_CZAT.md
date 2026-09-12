# Coach Hevy — obsługa z czatu

## Gdzie działa
MCP hevy_coach jest zarejestrowane w Codex CLI na Raspberry Pi. W nowej sesji Codex na RPi można używać poniższych poleceń naturalnym językiem. Zwykły ChatGPT i Codex na Windows nie otrzymują tej integracji automatycznie. Nie wystawiono publicznego serwera MCP.

## Przykłady wiadomości
- „Sprawdź przez hevy_coach, co dzisiaj robimy, i uzasadnij wybór.”
- „Przełóż dzisiejszy trening na jutro i zapisz to w pamięci coacha.”
- „Dziś mam 60 minut, zmęczenie 4/10, bolesność nóg 6/10, spałem 7 godzin. Zapisz check-in i sprawdź plan.”
- „Pokaż najnowszy raport tygodniowy coacha.”
- „Zrobiłem dziś Pull, ale jeszcze nie zapisałem go w Hevy. Zapisz deklarację.”
- „Cofnij przełożenie, które wcześniej ustaliliśmy.” — coach sprawdza stan i wskazuje konkretną operację.

Codex powinien przeliczyć „dzisiaj” i „jutro” na daty w Europe/Warsaw. Przy niejasnym terminie dopytuje.
Po zapisie powinien potwierdzić datę i sesję. Może ponownie wywołać coach_plan.
Zgłoszenia przez czat nie wysyłają dodatkowego ntfy. Nie tworzą treningów w Hevy.

## Zasady narzędzi
Przed zmianą: coach_status → aktualna wersja pamięci → coach_update z unikalnym identyfikatorem.
Ponowienie tej samej operacji używa tego samego identyfikatora. Konflikt wersji wymaga ponownego odczytu stanu.
coach_plan pobiera aktualne Hevy. coach_knowledge czyta źródłowe notatki. coach_latest_report zwraca zapisany raport.

## Obsidian
Folder: Zdrowie, uroda i siłownia/Trening/Siłownia/Coach Hevy.
- USTALENIA.md — trwałe zasady i profil.
- Stan.md — pamięć, przełożenia i check-iny; najlepiej aktualizować przez MCP.
- Plan dnia.md i Plany/ — aktualny podgląd i historia.
- Raporty/ — sobotnie analizy.
- Wdrożenie.md — etap i sposób wznowienia.
- OBSLUGA_CZAT.md — ta instrukcja.

Oryginalne rozdziały o mięśniach pozostają materiałem źródłowym. Coach nie nadpisuje ich.
Na tym etapie zmiany programu są propozycjami, a Hevy pozostaje tylko do odczytu.

## Automatyczne wiadomości — aktywne od 2026-09-12
Codziennie 18:00: wybór treningu lub odpoczynku.
Sobota 09:00: analiza AI w Obsidianie, potem ntfy o raporcie.
Strefa Europe/Warsaw, uwzględnia zmianę czasu.
MCP stdio uruchamia się na żądanie; harmonogram uruchamia osobne, kończące się zadania.
Uruchomienia poza ustalonym oknem są pomijane. Po niepewnej wysyłce nie ma automatycznego duplikatu.
