---
tags: [energetyka,ptpiree,bank_nastaw,tutorial]
source: "https://ptpiree.pl/wp-content/uploads/2024/12/bn_2024_10_01-1.pdf"
version: "1.1"
---
## Cel kursu
Zrozumieć, **dlaczego falownik oddaje energię, zmniejsza moc lub odłącza się od sieci**, i jak czytać dokument PTPiREE „Bank Nastaw dla Polski” (wersja 1.1 z 01.10.2024, stosowana od 01.11.2024).

> [!important] Granice zastosowania
> To kurs objaśniający, nie instrukcja samodzielnego zmieniania zabezpieczeń w pracującej instalacji. Ostateczne parametry zależą od typu modułu, punktu przyłączenia, operatora systemu i dokumentacji urządzenia. **Nie wgrywaj przykładowych nastaw bez weryfikacji projektu i wymagań operatora.**

## Ścieżka nauki — czytaj kolejno
1. [[01 - Co reguluje Bank Nastaw|Dlaczego istnieje bank nastaw]]
2. [[02 - Podstawy napięcia częstotliwości i mocy|Napięcie, częstotliwość, moc P i Q]]
3. [[03 - Typ A - zabezpieczenia|Zabezpieczenia typu A]]
4. [[04 - Typ A - regulacja mocy|Regulacja typu A]]
5. [[05 - Typ B - podstawy i zabezpieczenia|Wymagania typu B]]
6. [[06 - Typ B - FRT i prąd zwarciowy|Praca podczas zwarcia]]
7. [[07 - Powrót do pracy i moc bierna typu B|Resynchronizacja i moc bierna]]
8. [[08 - Przykłady i ćwiczenia|Przykłady i samosprawdzenie]]
9. [[09 - Checklist wdrożeniowy|Lista kontrolna do projektu]]
10. [[Leksykon - Bank Nastaw|Leksykon]] — zaglądaj przy każdym nowym terminie.

## Jak czytać źródło
- Strony 1–6 PDF: wymagania dla typu A po polsku.
- Strony 7–11: odpowiednik dla typu A po angielsku.
- Strony 12–20: typ B w sieci SN po polsku.
- Strony 21–30: typ B w sieci SN po angielsku.
- Strony 31–39: typ B w sieci nn po polsku.
- Strony 40–51: typ B w sieci nn po angielsku.
Numeracja powyżej jest numeracją stron pliku PDF, liczona od 1. **Rysunki i dokładne granice charakterystyk należy oglądać w oryginalnym PDF.**

## Jednozdaniowa mapa
**Zabezpieczenia** mówią *kiedy przestać generować*, **charakterystyki regulacyjne** mówią *jak zmieniać P i Q*, a **FRT** mówi *kiedy trzeba pozostać w pracy mimo zakłócenia*.

Źródło: [Bank nastaw PTPiREE, PDF](https://ptpiree.pl/wp-content/uploads/2024/12/bn_2024_10_01-1.pdf).

---
## Rozszerzony kurs: analiza krok po kroku

> [!info] Metoda
> Każda sekcja zawiera interpretację, diagnostykę i pytania sprawdzające. To materiał dydaktyczny na podstawie PTPiREE v1.1 (01.10.2024); wszystkie scenariusze mają charakter ilustracyjny, a nie zatwierdzonych nastaw.


### Studium 1: Jak wybrać właściwą część normy
**Definicja robocza.** najpierw zidentyfikuj typ modułu i poziom napięcia. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Jak wybrać właściwą część normy”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** A i B nie są synonimem nn i SN. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Jak wybrać właściwą część normy”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 2: Rola operatora systemu
**Definicja robocza.** warunki przyłączenia definiują ograniczenia obiektowe. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Rola operatora systemu”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie kopiuj nastaw bez uzgodnienia. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Rola operatora systemu”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 3: Co oznacza próg U>
**Definicja robocza.** reakcja ochronna nie jest tym samym co regulacja. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Co oznacza próg U>”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** 253 V nie stanowi progu startu P(U). Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Co oznacza próg U>”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 4: Po co występuje zwłoka
**Definicja robocza.** filtruje przejściowe zakłócenia zależnie od funkcji. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Po co występuje zwłoka”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** 0,1 s i 3 s służą innym zjawiskom. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Po co występuje zwłoka”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 5: Dlaczego używa się p.u.
**Definicja robocza.** ta sama krzywa może dotyczyć różnych poziomów U. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Dlaczego używa się p.u.”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** bazę trzeba zapisać w dokumentacji. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Dlaczego używa się p.u.”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 6: Kiedy potrzebna jest składowa zgodna
**Definicja robocza.** pozwala opisywać trójfazowy przebieg w FRT. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Kiedy potrzebna jest składowa zgodna”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** wartości chwilowe i składowe to nie to samo. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Kiedy potrzebna jest składowa zgodna”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 7: Napięcie przed zwarciem
**Definicja robocza.** jest odniesieniem dla dodatkowego prądu biernego. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Napięcie przed zwarciem”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** dokument odwołuje się do średniej za minutę. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Napięcie przed zwarciem”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 8: Punkt przyłączenia
**Definicja robocza.** pomiary w PCC różnią się od zacisków falownika. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Punkt przyłączenia”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** straty i impedancja kabla mają znaczenie. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Punkt przyłączenia”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 9: Co czytać w charakterystyce
**Definicja robocza.** osie, jednostki, punkty przegięcia i obszar pracy. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Co czytać w charakterystyce”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie odczytuj krzywej tylko z jednego punktu. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Co czytać w charakterystyce”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 10: Jak rozumieć status generacji
**Definicja robocza.** zaprzestanie generacji nie jest uniwersalnym opisem stanu łącznika. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Jak rozumieć status generacji”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** ustal jak urządzenie raportuje stan. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Jak rozumieć status generacji”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 11: Co sprawdzić u producenta
**Definicja robocza.** parametry menu mogą mieć inne nazwy i jednostki. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Co sprawdzić u producenta”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** opis funkcji ważniejszy niż nazwa rejestru. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Co sprawdzić u producenta”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 12: Dlaczego badać przebiegi
**Definicja robocza.** piki, średnie i RMS odpowiadają na różne pytania. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Dlaczego badać przebiegi”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** mierz z zadanym interwałem. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Dlaczego badać przebiegi”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 13: Jak uczyć się z ćwiczeń
**Definicja robocza.** najpierw przewiduj, potem licz i sprawdzaj. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Jak uczyć się z ćwiczeń”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** sam wynik nie dowodzi zrozumienia. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Jak uczyć się z ćwiczeń”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 14: Jak interpretować wyjątki
**Definicja robocza.** przypis operatora może zmienić zastosowanie. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Jak interpretować wyjątki”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** warunki obiektu są kluczowe. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Jak interpretować wyjątki”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 15: Zależność P oraz Q
**Definicja robocza.** prąd przekształtnika ogranicza dostępne sterowanie. W kontekście rozdziału „00 - START” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Zależność P oraz Q”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** żądana Q może zmniejszać dostępną P. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Zależność P oraz Q”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

## Test końcowy — wykonaj samodzielnie
Przygotuj tabelę zawierającą: **pojęcie | wielkość fizyczna | jednostka | punkt pomiaru | wejście | oczekiwana reakcja | zależność od operatora | możliwy błąd interpretacji**. Wypełnij ją dla wszystkich pojęć opracowanych w tym rozdziale. Dla każdego przyporządkuj konkretny fragment oryginalnego dokumentu i sprawdź, czy mowa o wymaganiu, dopuszczalnej charakterystyce, czy przykładzie. Wątpliwości oznacz jako „do sprawdzenia”, zamiast dopisywać niepotwierdzone nastawy.

