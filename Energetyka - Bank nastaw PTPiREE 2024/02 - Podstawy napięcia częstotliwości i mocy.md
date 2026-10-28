---
tags: [energetyka,podstawy,tutorial]
---
## Napięcie U
W instalacji jednofazowej punktem odniesienia użytym w tabeli typu A jest **Un = 230 V**. 0,85 Un = 195,5 V; 1,1 Un = 253 V; 1,15 Un = 264,5 V. W sieci SN i dla typu B **przeliczaj na właściwą bazę napięciową obiektu**.

## Częstotliwość f
W Polsce częstotliwość znamionowa sieci wynosi **50 Hz**. Gdy generacja i zużycie są niezrównoważone, częstotliwość zmienia się. Urządzenia mają zarówno funkcje regulacji mocy względem częstotliwości, jak i awaryjne progi częstotliwościowe.

## Moc — trzy symbole
- **P [kW]**: moc czynna, odpowiedzialna za przekazywanie użytecznej energii.
- **Q [kvar]**: moc bierna, związana z wymianą energii między polami elektrycznymi i magnetycznymi; może wpływać na napięcie w sieci.
- **S [kVA]**: moc pozorna; w uproszczeniu dla sinusoidalnego stanu ustalonego: **S² = P² + Q²**.
- **cos φ = P/S**: współczynnik mocy w typowych warunkach sinusoidalnych.

**Uwaga o znakach:** znak Q oraz nazewnictwo „pobór/generacja”, „indukcyjny/pojemnościowy” potrafią różnić się między producentami. Przed mapowaniem do rejestrów PLC sprawdź konwencję w dokumentacji konkretnego falownika.

## p.u. — per unit
Wartość względna = wartość mierzona / wartość bazowa. **1,08 p.u. = 108% bazy**; przy bazie 230 V daje 248,4 V. Przy bazie 15 kV daje 16,2 kV. Nie przeliczaj bez rozpoznania punktu i bazy pomiarowej.

## Czas zwłoki
Jeśli opis mówi „t=3 s”, nie znaczy to „każdy pik > progu natychmiast wyłącza”. Liczą się warunki, okno pomiarowe i sposób realizacji zabezpieczenia. **Dla U> 253 V dokument odsyła do 10-minutowej średniej w oknie przesuwnym** (EN 50160, PN-EN 50549); nie interpretuj tego tak jak próbki chwilowej.

## Minićwiczenia
- 1,06 × 230 V = **243,8 V**.
- 1,08 × 230 V = **248,4 V**.
- Q/P = 0,4843 odpowiada modułowi cos φ około **0,9**.

Powiązania: [[03 - Typ A - zabezpieczenia]], [[04 - Typ A - regulacja mocy]], [[Leksykon - Bank Nastaw]].

---
## Rozszerzony kurs: analiza krok po kroku

> [!info] Metoda
> Każda sekcja zawiera interpretację, diagnostykę i pytania sprawdzające. To materiał dydaktyczny na podstawie PTPiREE v1.1 (01.10.2024); wszystkie scenariusze mają charakter ilustracyjny, a nie zatwierdzonych nastaw.


### Studium 1: Wartość skuteczna RMS
**Definicja robocza.** opisuje równoważne działanie przebiegu zmiennego. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Wartość skuteczna RMS”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie jest wartością szczytową. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Wartość skuteczna RMS”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 2: Wartość chwilowa
**Definicja robocza.** u(t) może wahać się w każdej milisekundzie. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Wartość chwilowa”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie podstawiaj jej do progu 10-minutowego. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Wartość chwilowa”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 3: Napięcie fazowe i międzyfazowe
**Definicja robocza.** w układzie symetrycznym występuje czynnik √3. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Napięcie fazowe i międzyfazowe”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** 230 V i 400 V to inne odniesienia. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Napięcie fazowe i międzyfazowe”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 4: Per unit
**Definicja robocza.** U_pu=U/Ubase. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Per unit”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** 1,08 p.u. przy 230 V oznacza 248,4 V. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Per unit”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 5: Spadek napięcia w kablu
**Definicja robocza.** impedancja i prąd powodują różnicę napięć. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Spadek napięcia w kablu”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** PCC nie zawsze równa się zaciskowi. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Spadek napięcia w kablu”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 6: Częstotliwość systemowa
**Definicja robocza.** sieć znamionowo 50 Hz. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Częstotliwość systemowa”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** 50,2 Hz uruchamia LFSM-O a nie f>. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Częstotliwość systemowa”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 7: Pochodna częstotliwości
**Definicja robocza.** RoCoF≈Δf/Δt. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Pochodna częstotliwości”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** szum pomiarowy wymaga filtracji. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Pochodna częstotliwości”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 8: Moc czynna
**Definicja robocza.** P odpowiada za średni transport energii. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Moc czynna”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** kWh to energia, a nie moc. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Moc czynna”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 9: Moc bierna
**Definicja robocza.** Q wiąże się z przesunięciem fazowym. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Moc bierna”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** kvar nie jest kWh. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Moc bierna”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 10: Moc pozorna
**Definicja robocza.** S²=P²+Q² w standardowych sinusoidalnych warunkach. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Moc pozorna”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** przekroczenie S oznacza ograniczenie. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Moc pozorna”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 11: Współczynnik mocy
**Definicja robocza.** cosφ=P/S przy sinusoidzie. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Współczynnik mocy”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** cosφ=0,9 daje Q/P≈0,4843. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Współczynnik mocy”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 12: Znak mocy biernej
**Definicja robocza.** urządzenia różnie oznaczają pobór i generację. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Znak mocy biernej”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** sprawdzaj kierunki na analizatorze. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Znak mocy biernej”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 13: Trójfazowość
**Definicja robocza.** w symetrii P=√3 U_LL I cosφ. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Trójfazowość”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie zapominaj o jednostkach. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Trójfazowość”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 14: Składowe symetryczne
**Definicja robocza.** zgodna i przeciwna opisują asymetrię. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Składowe symetryczne”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** FRT B wymaga ich rozróżnienia. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Składowe symetryczne”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 15: Stany ustalone i przejściowe
**Definicja robocza.** regulacja statyczna różni się od dynamicznej odpowiedzi. W kontekście rozdziału „02 - Podstawy napięcia częstotliwości i mocy” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Stany ustalone i przejściowe”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** milisekundy i minuty nie są zamienne. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Stany ustalone i przejściowe”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

## Test końcowy — wykonaj samodzielnie
Przygotuj tabelę zawierającą: **pojęcie | wielkość fizyczna | jednostka | punkt pomiaru | wejście | oczekiwana reakcja | zależność od operatora | możliwy błąd interpretacji**. Wypełnij ją dla wszystkich pojęć opracowanych w tym rozdziale. Dla każdego przyporządkuj konkretny fragment oryginalnego dokumentu i sprawdź, czy mowa o wymaganiu, dopuszczalnej charakterystyce, czy przykładzie. Wątpliwości oznacz jako „do sprawdzenia”, zamiast dopisywać niepotwierdzone nastawy.

