---
tags: [energetyka,typ_b,moc_bierna]
---
## Powrót po odłączeniu ≠ odbudowa po FRT
To dwa różne procesy:
- **Po FRT** moduł mógł pozostać przyłączony i odbudowuje P po ustąpieniu zwarcia (zob. [[06 - Typ B - FRT i prąd zwarciowy]]).
- **Po odłączeniu od sieci** ponowne przyłączenie wymaga spełnienia warunków resynchronizacji.

## Resynchronizacja typu B — źródło s. 18 i 37 PDF
1. Częstotliwość sieci w przedziale **49–50,05 Hz**.
2. Napięcie w punkcie przyłączenia w zakresie dopuszczonym właściwymi regulacjami.
3. Zwłoka **co najmniej 60 s** po powrocie parametrów do dozwolonych zakresów.

W odróżnieniu od opisu typu A, w tych tabelach B nie ma analogicznie wypisanej reguły narastania **10% Pmax/min**. Nie przenoś jej automatycznie bez osobnej podstawy.

## Regulacja mocy biernej typu B
PTPiREE przedstawia graficznie wymagania **U–Q/Pmax** (możliwość regulacji Q przy określonym U) i **P–Q/Pmax** (zdolność do Q przy zmianie P).
- Punkt przyłączenia, nie dowolny punkt pomiarowy w szafie, jest istotny.
- Przy **P < 0,1 Pmax** moduł powinien udostępniać dostępną moc bierną zgodnie z możliwościami technicznymi.
- PPM powinien mieć techniczną zdolność kompensowania Q w punkcie przyłączenia **także bez generacji P**.
- Jeśli wymaga się pracy przy **U < 0,9 p.u.**, udostępnia się dostępną Q na miarę możliwości technicznych.

> [!warning]
> Odpowiednie rysunki w oryginale są niezbędne do odczytu **granic obszaru pracy**. Te notatki wyjaśniają sens, ale nie rekonstruują wszystkich punktów wykresów z tekstowego PDF.

## Co sprawdzić w projekcie?
- Czy pomiary P/Q/U są odniesione do **PCC**, a nie wyłącznie zacisków falownika?
- Czy urządzenie może produkować Q przy P≈0?
- Czy znaki i kierunki Q producenta są zgodne z konwencją projektu?
- Czy dostępne Imax i S pozwalają na jednoczesne P i Q?
- Czy regulator Q jest skoordynowany z nadrzędnym sterowaniem farmy i operatorem?

Źródło: PDF s. 18–20 i 37–39.

---
## Rozszerzony kurs: analiza krok po kroku

> [!info] Metoda
> Każda sekcja zawiera interpretację, diagnostykę i pytania sprawdzające. To materiał dydaktyczny na podstawie PTPiREE v1.1 (01.10.2024); wszystkie scenariusze mają charakter ilustracyjny, a nie zatwierdzonych nastaw.


### Studium 1: FRT i ponowne przyłączenie
**Definicja robocza.** podczas FRT możliwa jest ciągłość połączenia. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „FRT i ponowne przyłączenie”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** po otwarciu trzeba spełnić warunki startu. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „FRT i ponowne przyłączenie”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 2: Warunek częstotliwości
**Definicja robocza.** 49–50,05 Hz. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Warunek częstotliwości”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie myl z zabezpieczeniem 47,5 i 51,5 Hz. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Warunek częstotliwości”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 3: Warunek napięcia
**Definicja robocza.** dopuszczalny zakres wynikający z właściwych regulacji. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Warunek napięcia”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** dokument B nie narzuca jednej uniwersalnej liczby. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Warunek napięcia”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 4: Zwłoka minimum 60 s
**Definicja robocza.** czas od powrotu parametrów do zakresów. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Zwłoka minimum 60 s”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie licz od momentu utraty generacji. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Zwłoka minimum 60 s”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 5: Powrót mocy po FRT
**Definicja robocza.** 90% P sprzed zwarcia w określonym czasie. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Powrót mocy po FRT”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** to nie ta sama rampa co ponowne przyłączenie A. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Powrót mocy po FRT”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 6: Poniżej P=0,1 Pmax
**Definicja robocza.** należy udostępnić Q zgodnie z możliwościami. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Poniżej P=0,1 Pmax”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** zerowa P nie oznacza zerowej Q. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Poniżej P=0,1 Pmax”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 7: Q przy P=0
**Definicja robocza.** PPM ma mieć techniczną możliwość kompensacji w PCC. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Q przy P=0”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** mogą być potrzebne elementy dodatkowe. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Q przy P=0”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 8: Charakterystyka U-Q/Pmax
**Definicja robocza.** pokazuje zakres możliwości przy napięciu. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Charakterystyka U-Q/Pmax”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie jest pojedynczą wartością setpointu. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Charakterystyka U-Q/Pmax”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 9: Charakterystyka P-Q/Pmax
**Definicja robocza.** pokazuje zdolność Q przy różnej P. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Charakterystyka P-Q/Pmax”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** granica mocy pozornej nadal istnieje. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Charakterystyka P-Q/Pmax”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 10: Współczynnik cosφ
**Definicja robocza.** może ograniczać relację Q/P. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Współczynnik cosφ”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie definiuje pełnego obszaru U-Q. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Współczynnik cosφ”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 11: Napięcie poniżej 0,9 p.u.
**Definicja robocza.** dostępna Q powinna zostać udostępniona, gdy praca jest wymagana. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Napięcie poniżej 0,9 p.u.”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** zależne od możliwości technicznych. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Napięcie poniżej 0,9 p.u.”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 12: Kompensacja transformatora
**Definicja robocza.** PCC widzi również wpływ urządzeń pomiędzy źródłem i siecią. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Kompensacja transformatora”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** Q falownika ≠ Q w PCC. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Kompensacja transformatora”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 13: Prąd Imax
**Definicja robocza.** ogranicza łączne sterowanie P i Q. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Prąd Imax”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie zakładaj pełnych obu mocy naraz. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Prąd Imax”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 14: Tryby regulatora
**Definicja robocza.** lokalny falownik i regulator farmy mogą wysyłać różne cele. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Tryby regulatora”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** ustal priorytet i mastera. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Tryby regulatora”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 15: Jakość odbudowy
**Definicja robocza.** brak nietłumionych oscylacji jest wymogiem. W kontekście rozdziału „07 - Powrót do pracy i moc bierna typu B” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Jakość odbudowy”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** zapisuj czasową odpowiedź mocy. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Jakość odbudowy”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

## Test końcowy — wykonaj samodzielnie
Przygotuj tabelę zawierającą: **pojęcie | wielkość fizyczna | jednostka | punkt pomiaru | wejście | oczekiwana reakcja | zależność od operatora | możliwy błąd interpretacji**. Wypełnij ją dla wszystkich pojęć opracowanych w tym rozdziale. Dla każdego przyporządkuj konkretny fragment oryginalnego dokumentu i sprawdź, czy mowa o wymaganiu, dopuszczalnej charakterystyce, czy przykładzie. Wątpliwości oznacz jako „do sprawdzenia”, zamiast dopisywać niepotwierdzone nastawy.

