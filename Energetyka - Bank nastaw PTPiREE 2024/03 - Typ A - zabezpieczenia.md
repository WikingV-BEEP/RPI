---
tags: [energetyka,typ_a,zabezpieczenia]
---
## Nastawy typu A — według PTPiREE, s. 2 PDF
| Funkcja | Wymagany próg | Czas / reakcja |
|---|---|---|
| U< podnapięcie | 0,85 Un = **195,5 V** | **1,2 s** → zaprzestanie generacji |
| U> nadnapięcie I | 1,10 Un = **253 V** | **3 s** → zaprzestanie generacji; patrz przypis o średniej 10-minutowej |
| U>> nadnapięcie II | 1,15 Un = **264,5 V** | **0,1 s** → zaprzestanie generacji |
| f< podczęstotliwość | **47,5 Hz** | **1 s** |
| f> nadczęstotliwość | **51,5 Hz** | **0,3 s** |
| LoM / RoCoF | **2,5 Hz/s** | maksymalny czas działania **0,5 s** |

W tabeli dokumentu minimalny wymagany zakres nastawczy czasów wynosi **0–10 s**. **Nie oznacza to, że należy ustawiać 0–10 s jako opóźnienie.**

## Interpretacja inżynierska
- **U<**: sieć ma zbyt małe napięcie.
- **U> oraz U>>**: dwa poziomy reakcji na zbyt wysokie napięcie — im wyższy próg, tym szybsza reakcja.
- **f< i f>**: sieć wychodzi poza skrajne granice częstotliwości.
- **RoCoF**: szybkość zmiany częstotliwości może wskazywać odłączenie od zasilającej sieci (LoM, utrata sieci).

## Przykład
Pomiary przez instalatorów: 254 V na fazie. **Nie wyciągaj automatycznie wniosku**, że zabezpieczenie U> powinno odłączyć falownik po trzech sekundach ciągłego wskazania 254 V na prostym mierniku. Dla U> istotna jest metoda 10-minutowej średniej oraz zasady pomiaru opisane w normach.

## Uwaga
Bank nastaw nie zastępuje analizy selektywności, certyfikatów urządzenia ani lokalnych wymagań operatora.

Dalej: [[04 - Typ A - regulacja mocy]].

---
## Rozszerzony kurs: analiza krok po kroku

> [!info] Metoda
> Każda sekcja zawiera interpretację, diagnostykę i pytania sprawdzające. To materiał dydaktyczny na podstawie PTPiREE v1.1 (01.10.2024); wszystkie scenariusze mają charakter ilustracyjny, a nie zatwierdzonych nastaw.


### Studium 1: Podnapięcie 195,5 V
**Definicja robocza.** 0,85×230 V; t=1,2 s. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Podnapięcie 195,5 V”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** najpierw ustal typ mierzonego napięcia. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Podnapięcie 195,5 V”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 2: Nadnapięcie U> 253 V
**Definicja robocza.** 1,1×230 V; t=3 s. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Nadnapięcie U> 253 V”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** ważny przypis o 10-minutowej średniej. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Nadnapięcie U> 253 V”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 3: Nadnapięcie U>> 264,5 V
**Definicja robocza.** 1,15×230 V; t=0,1 s. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Nadnapięcie U>> 264,5 V”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** jest szybsze od U>. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Nadnapięcie U>> 264,5 V”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 4: Podczęstotliwość 47,5 Hz
**Definicja robocza.** t=1 s. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Podczęstotliwość 47,5 Hz”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie pomyl z granicą ponownego przyłączenia 49 Hz. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Podczęstotliwość 47,5 Hz”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 5: Nadczęstotliwość 51,5 Hz
**Definicja robocza.** t=0,3 s. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Nadczęstotliwość 51,5 Hz”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** LFSM-O startuje wcześniej przy 50,2 Hz. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Nadczęstotliwość 51,5 Hz”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 6: RoCoF 2,5 Hz/s
**Definicja robocza.** tmax=0,5 s. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „RoCoF 2,5 Hz/s”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** zmiana f w czasie to nie wartość f. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „RoCoF 2,5 Hz/s”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 7: Zakres 0–10 s
**Definicja robocza.** minimalny wymagany zakres regulacji czasu. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Zakres 0–10 s”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie wpisuj go jako wartości nastawy. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Zakres 0–10 s”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 8: Kumulacja zdarzeń
**Definicja robocza.** kilka funkcji może być aktywnych jednocześnie. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Kumulacja zdarzeń”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** priorytet ochrony ustala implementacja. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Kumulacja zdarzeń”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 9: Metoda pomiaru
**Definicja robocza.** chwilowe, RMS i średnie są różne. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Metoda pomiaru”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** loguj właściwy sygnał. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Metoda pomiaru”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 10: Czas próbkowania
**Definicja robocza.** zbyt wolny logger może nie zobaczyć U>>. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Czas próbkowania”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** 0,1 s wymaga odpowiedniej rozdzielczości. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Czas próbkowania”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 11: Fałszywa diagnoza odłączenia
**Definicja robocza.** brak produkcji może wynikać także z braku irradiancji lub błędu. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Fałszywa diagnoza odłączenia”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** czytaj kod przyczyny. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Fałszywa diagnoza odłączenia”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 12: Praca wyspowa
**Definicja robocza.** LoM przeciwdziała niekontrolowanej wyspie. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Praca wyspowa”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** sam RoCoF nie potwierdza awarii. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Praca wyspowa”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 13: Test na progu
**Definicja robocza.** załączenie kryterium i zwłoka powinny zostać rozdzielone. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Test na progu”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** testy prowadzi uprawniony personel. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Test na progu”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 14: Histereza
**Definicja robocza.** powrót spod progu to nie zawsze natychmiastowy restart. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Histereza”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** odróżnij warunki startu. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Histereza”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 15: Diagnostyka
**Definicja robocza.** zapisuj U, f, RoCoF, czas i przyczynę. W kontekście rozdziału „03 - Typ A - zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Diagnostyka”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** jeden punkt pomiarowy nie wystarczy. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Diagnostyka”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

## Test końcowy — wykonaj samodzielnie
Przygotuj tabelę zawierającą: **pojęcie | wielkość fizyczna | jednostka | punkt pomiaru | wejście | oczekiwana reakcja | zależność od operatora | możliwy błąd interpretacji**. Wypełnij ją dla wszystkich pojęć opracowanych w tym rozdziale. Dla każdego przyporządkuj konkretny fragment oryginalnego dokumentu i sprawdź, czy mowa o wymaganiu, dopuszczalnej charakterystyce, czy przykładzie. Wątpliwości oznacz jako „do sprawdzenia”, zamiast dopisywać niepotwierdzone nastawy.

