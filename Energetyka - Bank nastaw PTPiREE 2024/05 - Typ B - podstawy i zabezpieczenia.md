---
tags: [energetyka,typ_b,zabezpieczenia]
---
## Co nowego w typie B?
Moduł typu B ma nie tylko umieć się odłączyć, ale też **przetrwać określone zakłócenia**, wspomagać sieć prądem biernym i **odbudować moc po zwarciu**. Dlatego koordynacja progów zabezpieczeń z FRT jest krytyczna.

Dokument oddzielnie opisuje typ B podłączony do **SN** (s. 12–20 PDF) i **nn** (s. 31–39 PDF). W obu tabelach poniższe podstawowe progi mają identyczne wartości, ale ich zastosowanie wymaga **weryfikacji obiektowej i uzgodnienia z właściwym operatorem**.

| Funkcja | Nastawa B (SN i nn) | Reakcja |
|---|---|---|
| U< | **0,8 Un**, t = **3 s** | zaprzestanie generacji |
| U> | **1,1 Un**, t = **3 s** | zaprzestanie generacji; przypis o średniej 10-minutowej |
| U>> | **1,15 Un**, t = **0,1 s** | zaprzestanie generacji |
| f< | **47,5 Hz**, t = **1 s** | zaprzestanie generacji |
| f> | **51,5 Hz**, t = **0,3 s** | zaprzestanie generacji |
| LoM / RoCoF | **2 Hz/s**, tmax = **0,2 s** | maksymalny czas reakcji |
| LFSM-O | **50,2 Hz**, statyzm **5%** | redukcja P wg charakterystyki |
| Dopuszczalna redukcja P przy niskim f | **49 Hz**, **2% Pmax/Hz** poniżej | zgodnie z charakterystyką |

## A vs B — zapamiętaj
- **U<**: A = 0,85 Un i 1,2 s; B = 0,8 Un i 3 s.
- **RoCoF**: A = 2,5 Hz/s / 0,5 s; B = 2 Hz/s / 0,2 s.
- **FRT i szybki prąd bierny**: szczegółowo określone dla B.

## Dlaczego nie można mechanicznie kopiować progów?
W typie B dokument wprost wymaga sprawdzenia nastaw napięciowych **dla danego obiektu**, aby nie powodowały konfliktu z wymaganym zakresem pracy w sieci oraz funkcją FRT. Odpowiednie punkty pomiarowe i relacje do progów operatora są tu kluczowe.

Dalej: [[06 - Typ B - FRT i prąd zwarciowy]].

---
## Rozszerzony kurs: analiza krok po kroku

> [!info] Metoda
> Każda sekcja zawiera interpretację, diagnostykę i pytania sprawdzające. To materiał dydaktyczny na podstawie PTPiREE v1.1 (01.10.2024); wszystkie scenariusze mają charakter ilustracyjny, a nie zatwierdzonych nastaw.


### Studium 1: Odrębność typu B
**Definicja robocza.** większy nacisk na utrzymanie pracy podczas zakłóceń. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Odrębność typu B”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie zakładaj identycznego zachowania A i B. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Odrębność typu B”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 2: B w SN
**Definicja robocza.** wymaga odniesienia progów do napięć obiektu i operatora. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „B w SN”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nominalna baza SN musi być znana. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „B w SN”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 3: B w nn
**Definicja robocza.** osobny bank w dokumencie. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „B w nn”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie wykluczaj typu B przy 400 V. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „B w nn”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 4: U< 0,8 Un
**Definicja robocza.** czas 3 s. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „U< 0,8 Un”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** inaczej niż A 0,85 Un i 1,2 s. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „U< 0,8 Un”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 5: U> 1,1 Un
**Definicja robocza.** czas 3 s. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „U> 1,1 Un”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** uwzględnij pomiar 10-minutowy. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „U> 1,1 Un”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 6: U>> 1,15 Un
**Definicja robocza.** czas 0,1 s. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „U>> 1,15 Un”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** ochrona działa szybciej przy wyższej wartości. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „U>> 1,15 Un”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 7: f< 47,5 Hz
**Definicja robocza.** czas 1 s. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „f< 47,5 Hz”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** to próg ochronny a nie LFSM. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „f< 47,5 Hz”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 8: f> 51,5 Hz
**Definicja robocza.** czas 0,3 s. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „f> 51,5 Hz”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie jest częstotliwością startu regulacji. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „f> 51,5 Hz”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 9: RoCoF 2 Hz/s
**Definicja robocza.** tmax 0,2 s. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „RoCoF 2 Hz/s”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** inna czułość niż A. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „RoCoF 2 Hz/s”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 10: FRT
**Definicja robocza.** wymóg pozostawania w pracy w określonym profilu. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „FRT”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie może być blokowany przypadkowym U<. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „FRT”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 11: Szybki prąd bierny
**Definicja robocza.** reakcja w dziesiątkach ms. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Szybki prąd bierny”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** to funkcja dynamiczna. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Szybki prąd bierny”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 12: Obiektowe nastawy napięcia
**Definicja robocza.** wymagają zgodności z obszarem pracy operatora. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Obiektowe nastawy napięcia”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** wartości tabelaryczne są punktem startowym. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Obiektowe nastawy napięcia”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 13: PCC
**Definicja robocza.** powiąż pomiary z punktem przyłączenia. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „PCC”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** nie pomyl zacisków falownika i punktu sieci. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „PCC”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 14: Koordynacja zabezpieczeń
**Definicja robocza.** testuj przebiegi zakłóceń zamiast pojedynczych progów. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Koordynacja zabezpieczeń”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** selektwyność i FRT mogą się konfliktować. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Koordynacja zabezpieczeń”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

### Studium 15: Warunki odbioru
**Definicja robocza.** dokumentuj wersję banku i firmware. W kontekście rozdziału „05 - Typ B - podstawy i zabezpieczenia” zacznij od rozpoznania, czy mówimy o pomiarze, poleceniu regulatora, wymaganiu zdolności sprzętowej czy zdarzeniu ochronnym. Ta klasyfikacja określa, jakie dane trzeba zebrać i jak zinterpretować wynik. Zapis samej liczby z PDF bez kontekstu nie jest jeszcze wiedzą techniczną.

**Prześledź przyczynę i skutek.** Zapisz najpierw warunki początkowe: jednostkę, wielkość bazową, punkt pomiaru, aktywny tryb urządzenia oraz zdolność do realizacji polecenia. Następnie nazwij zdarzenie: co zmienia się w sieci, czy jest to stan ustalony, zakłócenie, czy powrót po zakłóceniu. Dopiero wtedy przewiduj zachowanie źródła. W automatyce odpowiada to rozdzieleniu wejść procesu, funkcji logicznej i wyjścia sterującego. Wyjście falownika nie musi być równoznaczne ze stanem wyłącznika głównego.

**Przykład diagnostyczny.** Załóż, że rejestrator SCADA pokazuje zdarzenie związane z pojęciem „Warunki odbioru”. Nie rozstrzygaj na podstawie jednego alarmu. Odczytaj przebieg przed zdarzeniem i po nim, polecenia regulatora nadrzędnego, napięcie oraz częstotliwość, a następnie sprawdź status ograniczeń P/Q i kod przyczyny falownika. Dla wielkości liczonych względnie przelicz dane na p.u. *dopiero po potwierdzeniu bazy*. Dla zdarzeń szybkich wymagana jest odpowiednia rozdzielczość rejestracji.

**Typowa pułapka.** numer w menu nie wystarczy. Tego rodzaju pomyłka prowadzi do błędnej diagnozy nawet wtedy, gdy tabela nastaw została skopiowana poprawnie. Rozdziel literalne wymaganie PTPiREE od inżynierskiej interpretacji oraz od decyzji projektowej właściwego operatora.

**Zadanie aktywne.** Wyjaśnij bez zaglądania: (1) co fizycznie oznacza „Warunki odbioru”; (2) jaki pomiar albo zapis parametrów pozwoli to rozpoznać; (3) jaką rolę pełni funkcja w stabilności i bezpieczeństwie sieci; (4) co może pójść źle przy błędnej konfiguracji; (5) czego *nie wolno* wywnioskować z tej jednej pozycji tabeli. Na koniec narysuj strzałki: **stan sieci → pomiar → kryterium → reakcja → zapis zdarzenia**.

**Kontrola rozumienia.** Jeżeli nie potrafisz wskazać punktu pomiarowego i jednostki, wróć do [[02 - Podstawy napięcia częstotliwości i mocy]]. Jeżeli nie potrafisz odróżnić działania regulacyjnego od ochronnego, wróć do [[01 - Co reguluje Bank Nastaw]]. Nie traktuj rozważań diagnostycznych jako instrukcji zmiany parametrów działającej instalacji.

## Test końcowy — wykonaj samodzielnie
Przygotuj tabelę zawierającą: **pojęcie | wielkość fizyczna | jednostka | punkt pomiaru | wejście | oczekiwana reakcja | zależność od operatora | możliwy błąd interpretacji**. Wypełnij ją dla wszystkich pojęć opracowanych w tym rozdziale. Dla każdego przyporządkuj konkretny fragment oryginalnego dokumentu i sprawdź, czy mowa o wymaganiu, dopuszczalnej charakterystyce, czy przykładzie. Wątpliwości oznacz jako „do sprawdzenia”, zamiast dopisywać niepotwierdzone nastawy.

