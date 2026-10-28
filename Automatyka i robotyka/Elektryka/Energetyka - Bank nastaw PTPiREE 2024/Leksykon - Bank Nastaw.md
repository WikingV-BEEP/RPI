---
tags: [energetyka,leksykon,terminologia]
---
## Leksykon A–Z
| Termin | Znaczenie w praktyce |
|---|---|
| **Bank nastaw** | Zbiór progów zabezpieczeń i parametrów regulacji urządzenia dla określonych wymagań systemu. |
| **cos φ** | Współczynnik mocy; przy przebiegach sinusoidalnych P/S. |
| **df/dt** | Pochodna częstotliwości po czasie, w Hz/s. |
| **Droop / statyzm** | Opis, jak moc zmienia się przy zmianie wielkości regulowanej (np. częstotliwości). |
| **EN 50160** | Norma opisująca charakterystyki napięcia w publicznych sieciach elektroenergetycznych. |
| **f< / f>** | Zabezpieczenie pod- / nadczęstotliwościowe. |
| **FRT (Fault Ride Through)** | Wymagana zdolność pozostania w pracy podczas określonego zakłócenia sieci. |
| **Imax** | Maksymalny dopuszczalny prąd fazowy modułu. |
| **K1** | Wzmocnienie szybkiej odpowiedzi prądu biernego składowej zgodnej. |
| **K2** | Wzmocnienie szybkiej odpowiedzi prądu biernego składowej przeciwnej. |
| **LFSM-O** | Limited Frequency Sensitive Mode – Overfrequency; ograniczanie mocy czynnej przy nadmiernym wzroście częstotliwości. |
| **LoM (Loss of Mains)** | Utrata połączenia z siecią zasilającą; istotna dla ryzyka pracy wyspowej. |
| **MWE** | Moduł wytwarzania energii. |
| **nn** | Sieć niskiego napięcia. |
| **OS / OSD** | Operator Systemu / Operator Systemu Dystrybucyjnego. |
| **P [kW]** | Moc czynna. |
| **P(U)** | Charakterystyka regulacji mocy czynnej P w funkcji napięcia U. |
| **PCC** | Point of Common Coupling; punkt wspólnego przyłączenia/odniesienia sieciowego. Jego umiejscowienie trzeba określić z projektu. |
| **PD** | Projektowana moc czynna (oznaczenie w charakterystyce typu A). |
| **PGM** | Moduł wytwarzania energii (oznaczenie stosowane w dokumencie). |
| **PN-EN 50549** | Rodzina norm dotyczących przyłączania źródeł wytwórczych do sieci dystrybucyjnych. |
| **PPM** | Moduł parku energii (np. zespół jednostek wytwórczych ze wspólnym punktem przyłączenia). |
| **Pref** | Moc czynna odniesienia: rzeczywista P w chwili aktywacji LFSM-O. |
| **Pmax** | Moc maksymalna, ale jej ścisła definicja zależy od konkretnej charakterystyki/sekcji dokumentu. |
| **p.u.** | Wartość względna; 1,0 p.u. = 100% przyjętej bazy. |
| **Q [kvar]** | Moc bierna. |
| **Q(U)** | Regulacja mocy biernej względem napięcia. |
| **RoCoF** | Rate of Change of Frequency; szybkość zmiany częstotliwości. |
| **S [kVA]** | Moc pozorna; wiąże maksymalne zdolności prądowe falownika z P i Q. |
| **SN** | Sieć średniego napięcia. |
| **Składowa zgodna (U1)** | Składnik układu trójfazowego o prawidłowej kolejności faz, używany w analizie stanów niesymetrycznych. |
| **Składowa przeciwna (U2)** | Składnik o odwrotnej kolejności faz, istotny przy asymetrii. |
| **tr** | Czas odpowiedzi prądowej na zakłócenie, w dokumencie B max 30 ms. |
| **ts** | Czas ustalania odpowiedzi prądowej, w dokumencie B max 60 ms. |
| **U< / U> / U>>** | Zabezpieczenia: podnapięcie / nadnapięcie 1. stopnia / nadnapięcie 2. stopnia. |
| **Un** | Napięcie znamionowe właściwej bazy odniesienia. |
| **Upr** | Próg rozpoczęcia regulacji P(U). |
| **Uprog1 / Uprog2** | Progi charakterystyki Q(U). |
| **Uret / Uclear / Urec1 / Urec2** | Poziomy napięciowe określające profil FRT. |
| **Utrig** | Próg wyzwalania dodatkowej odpowiedzi prądu biernego. |
| **Zwłoka** | Czas wymagany przed wykonaniem reakcji, po spełnieniu kryterium zabezpieczenia/sterowania. |

## Pułapki pojęciowe
- **Nadczęstotliwość** ≠ natychmiastowe wyłączenie: LFSM-O może regulować moc już od 50,2 Hz.
- **Nadnapięcie** ≠ to samo dla P(U) i U>: to różne funkcje.
- **Niski U podczas zwarcia** ≠ zawsze natychmiastowy trip: zob. [[06 - Typ B - FRT i prąd zwarciowy]].
- **Wartość p.u.** nie ma sensu bez wybranej wartości bazowej.
- **Q i cos φ** wymagają jednoznacznej konwencji znaków.

Źródło podstawowe: [PTPiREE „Bank Nastaw dla Polski”, v1.1](https://ptpiree.pl/wp-content/uploads/2024/12/bn_2024_10_01-1.pdf); objaśnienia dydaktyczne uzupełniają źródło.

---
## Leksykon rozszerzony — interpretacja, pomiar i diagnostyka

### 1. Bank nastaw

**Znaczenie.** Zestaw parametrów wymaganej współpracy jednostki z siecią.

**Rozwinięcie techniczne.** Należy rozdzielić wartości progów ochronnych, zwłoki, charakterystyki regulacyjne i zdolności dynamiczne. W dokumentacji konfiguracji potrzebne są również wersja firmware, odniesienie do typu modułu oraz data uzgodnienia z operatorem.

**Przykład.** Przykład: bank zawiera U> 1,1 Un i U>> 1,15 Un. Nie oznacza to dwóch niezależnych falowników; są to dwa stopnie innej reakcji na wzrost napięcia.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „Bank nastaw” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 2. Typ A

**Znaczenie.** Klasa modułu wytwarzania energii rozpatrywana w pierwszej części dokumentu.

**Rozwinięcie techniczne.** Charakterystyczne są zabezpieczenia U/f/RoCoF oraz ograniczenia P w funkcji napięcia i częstotliwości. Dokument zawiera również Q(U) i procedurę ponownego przyłączenia. Klasyfikację typu potwierdza się na podstawie obowiązujących wymagań i warunków przyłączenia.

**Przykład.** Przykład: A ma podnapięciowe U< 0,85 Un przy zwłoce 1,2 s, a B inną charakterystykę.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „Typ A” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 3. Typ B

**Znaczenie.** Klasa modułu o rozbudowanych wymaganiach dynamicznych.

**Rozwinięcie techniczne.** Bank nastaw osobno opisuje przyłączenie przez sieć średniego i niskiego napięcia. Typ B ma m.in. zdolność FRT, szybkie dodatkowe prądy bierne w stanach zwarciowych i wymogi odbudowy mocy.

**Przykład.** Przykład: B przyłączone do nn nie przestaje być typem B tylko dlatego, że napięcie jest niskie.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „Typ B” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 4. U<

**Znaczenie.** Zabezpieczenie podnapięciowe.

**Rozwinięcie techniczne.** Monitoruje spadek napięcia względem bazy Un. Wymagana zwłoka pozwala rozróżniać krótkie zdarzenia od stanów kwalifikujących do zaprzestania generacji. W typie B kryterium trzeba skoordynować z FRT, aby ochrona nie podważyła wymogu pozostania w pracy.

**Przykład.** Przykład: dla A przy bazie 230 V 0,85 Un daje 195,5 V, a zwłoka wynosi 1,2 s.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „U<” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 5. U>

**Znaczenie.** Pierwszy stopień nadnapięciowy.

**Rozwinięcie techniczne.** W tabeli dokumentu istotny jest przypis związany z pomiarem średniej 10-minutowej. W praktyce diagnostycznej konieczne jest zatem odróżnienie chwilowej próbki z miernika od sygnału używanego przez zabezpieczenie. W przeciwnym razie łatwo błędnie uznać działanie za niewłaściwe.

**Przykład.** Przykład: wskazanie 254 V przez 2 s nie opisuje w pełni warunku U>.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „U>” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 6. U>>

**Znaczenie.** Drugi, szybki stopień nadnapięciowy.

**Rozwinięcie techniczne.** Przeznaczony jest do szybkiej reakcji na istotne przekroczenie napięcia. Wymaga rejestracji z odpowiednią rozdzielczością czasu; logger minutowy nie pozwala odtworzyć zdarzenia trwającego dziesiątki milisekund.

**Przykład.** Przykład: 1,15×230 V=264,5 V; dokument podaje zwłokę 0,1 s.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „U>>” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 7. f< i f>

**Znaczenie.** Zabezpieczenia podczęstotliwościowe i nadczęstotliwościowe.

**Rozwinięcie techniczne.** Różnią się od trybów proporcjonalnego zmniejszania P. Częstotliwość jest globalnym wskaźnikiem bilansu energii w systemie, a progi ochronne dotyczą skrajniejszych sytuacji niż zwykła regulacja.

**Przykład.** Przykład: ograniczanie P od 50,2 Hz nie oznacza, że urządzenie ma się odłączyć przy 50,2 Hz; f> jest ustawione na 51,5 Hz.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „f< i f>” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 8. RoCoF

**Znaczenie.** Rate of Change of Frequency — tempo zmiany częstotliwości.

**Rozwinięcie techniczne.** W jednostkach Hz/s opisuje pochodną f względem czasu. W praktyce numeryczne różniczkowanie wzmacnia szum, dlatego wynik zależy od filtracji i algorytmu estymacji, nie tylko od dwóch przypadkowych próbek. RoCoF jest używane w kontekście LoM.

**Przykład.** Przykład: skok 0,2 Hz w 0,1 s daje średnio 2 Hz/s, lecz to nie jest jeszcze dowód zadziałania zabezpieczenia.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „RoCoF” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 9. LoM

**Znaczenie.** Loss of Mains — utrata sieci zasilającej.

**Rozwinięcie techniczne.** Wykrywanie utraty połączenia ma przeciwdziałać niekontrolowanemu utrzymaniu wyspy zasilanej przez lokalną generację. Obserwuje się dostępne sygnały, m.in. zmiany częstotliwości, ale należy odróżnić stan sieci od błędu lub awarii czujnika.

**Przykład.** Przykład: alarm LoM bez przebiegu U i f nie wystarcza, by uznać, że fizycznie zanikło zasilanie.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „LoM” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 10. LFSM-O

**Znaczenie.** Limited Frequency Sensitive Mode – Overfrequency.

**Rozwinięcie techniczne.** Tryb ograniczenia mocy czynnej, gdy częstotliwość przekroczy określony próg. Istotne parametry to próg aktywacji, statyzm oraz punkt odniesienia Pref. Nie jest to wyzwalacz natychmiastowego zaprzestania generacji.

**Przykład.** Przykład: start przy 50,2 Hz i statyzm 5% w banku typu A/B.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „LFSM-O” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 11. Statyzm

**Znaczenie.** Parametr nachylenia charakterystyki regulacyjnej.

**Rozwinięcie techniczne.** Bez znajomości dokładnej definicji, osi wykresu i normalizacji nie wolno interpretować zapisu »5%« jako identycznego procentowego ubytku mocy w dowolnym punkcie. Statyzm łączy zmianę wielkości wejściowej z odpowiedzią regulatora.

**Przykład.** Przykład: wykres LFSM-O opisuje odpowiedź P(f), podczas gdy statyzm P(U) dotyczy innej zależności.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „Statyzm” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 12. Pref

**Znaczenie.** Moc odniesienia w regulacji LFSM-O.

**Rozwinięcie techniczne.** W przypadku opisanego profilu jest powiązana z rzeczywistą mocą w chwili aktywacji charakterystyki. Odmienne nasłonecznienie w dniu testu może zmienić moc odniesienia, nawet jeśli moc znamionowa instalacji nie zmieniła się.

**Przykład.** Przykład: przy częściowym zachmurzeniu Pref jest mniejsze niż moc osiągalna przy pełnym słońcu.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „Pref” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 13. P(U)

**Znaczenie.** Charakterystyka mocy czynnej względem napięcia.

**Rozwinięcie techniczne.** Przy wzroście U urządzenie może redukować P, aby ograniczyć podnoszenie napięcia. Rola jest regulacyjna, nie zastępuje stopni U> i U>>. Istotna jest pełna krzywa, definicja Pmax oraz warunki dostępności energii.

**Przykład.** Przykład: w typie A początek przy 1,08 p.u. odpowiada 248,4 V dla Un=230 V.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „P(U)” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 14. Q(U)

**Znaczenie.** Charakterystyka mocy biernej względem napięcia.

**Rozwinięcie techniczne.** Regulator dobiera Q w odpowiedzi na U, aby wspierać poziom napięcia. Występują progi, obszary przejściowe i granice mocy biernej. Należy zweryfikować konwencję znaków i to, czy wielkości są liczone w punkcie przyłączenia czy na zaciskach falownika.

**Przykład.** Przykład: w profilu typu A Uprog1=0,94 p.u. oraz Uprog2=1,06 p.u.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „Q(U)” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 15. P, Q, S

**Znaczenie.** Odpowiednio moc czynna, bierna i pozorna.

**Rozwinięcie techniczne.** Dla przebiegów sinusoidalnych S²=P²+Q². Moc pozorna i prąd falownika ograniczają jednoczesną zdolność do generacji P i wymiany Q. Bez rozpoznania tych ograniczeń można błędnie uznać, że regulator Q nie działa.

**Przykład.** Przykład: przy Smax=100 kVA oraz P=80 kW maksymalny moduł Q wynosi teoretycznie 60 kvar.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „P, Q, S” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 16. cosφ

**Znaczenie.** Współczynnik mocy w sinusoidalnym stanie ustalonym.

**Rozwinięcie techniczne.** Stosunek P do S. Zależność Q/P=tan(arccos(cosφ)) pozwala obliczyć dostępny poziom mocy biernej dla danego założenia. Przy zniekształceniach i zmiennych przebiegach rozróżniaj współczynnik mocy od samego przesunięcia fazowego.

**Przykład.** Przykład: cosφ=0,9 daje Q/P≈0,4843.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „cosφ” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 17. FRT

**Znaczenie.** Fault Ride Through — przejście przez zakłócenie.

**Rozwinięcie techniczne.** Wymagana zdolność pracy w obszarze napięcie–czas podczas zwarcia. Krzywą należy analizować jako całe U(t), a nie jako jedną granicę napięcia. Dla modułu B ochrona napięciowa i FRT muszą być skoordynowane.

**Przykład.** Przykład: spadek U do 0,05 p.u. przez krótki czas może należeć do obszaru wymaganego przetrwania; sprawdź oryginalny wykres.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „FRT” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 18. K1 i K2

**Znaczenie.** Wzmocnienia dynamicznego prądu biernego odpowiednio składowej zgodnej i przeciwnej.

**Rozwinięcie techniczne.** K1 opisuje szybką reakcję na zmianę napięcia składowej zgodnej, a K2 na składową przeciwną w zwarciach niesymetrycznych. Obie odpowiedzi są wektorowe i podlegają limitowi prądu urządzenia.

**Przykład.** Przykład: w banku B K1=2 oraz K2=2; nie oznacza to bezwarunkowo dwukrotnego przekroczenia prądu znamionowego.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „K1 i K2” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 19. Utrig

**Znaczenie.** Próg aktywacji szybkiego wsparcia prądem biernym.

**Rozwinięcie techniczne.** Przy obniżeniu napięcia do określonego poziomu aktywuje się funkcja dynamiczna. Nie jest synonimem granicznego napięcia krzywej FRT ani progu U<. Przed testem trzeba odróżnić trzy niezależne role.

**Przykład.** Przykład: dla B Utrig=0,85 Un, choć krzywa FRT obejmuje znacznie niższe napięcia.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „Utrig” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 20. Imax

**Znaczenie.** Maksymalny prąd fazowy wytwórczego urządzenia.

**Rozwinięcie techniczne.** Granica fizycznej zdolności układu przekształtnikowego. W zwarciu priorytet wymaganej odpowiedzi biernej może wymusić ograniczenie składowej czynnej prądu. Analiza powinna uwzględnić również ograniczenia termiczne i firmowe algorytmy sterowania.

**Przykład.** Przykład: polecenie regulatora większe niż limit nie oznacza, że falownik może dostarczyć dowolny prąd.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „Imax” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 21. PCC

**Znaczenie.** Point of Common Coupling — punkt wspólnego przyłączenia.

**Rozwinięcie techniczne.** Ważny punkt odniesienia wymagań dotyczących jakości i wymiany mocy. Pomiary po stronie falownika mogą różnić się od PCC wskutek impedancji przewodów i transformatora. Zidentyfikuj jednoznacznie granicę obiektu.

**Przykład.** Przykład: 0 kvar na zaciskach falownika nie musi oznaczać 0 kvar widzianych od strony sieci.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „PCC” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 22. Resynchronizacja

**Znaczenie.** Ponowna gotowość przyłączenia źródła do sieci.

**Rozwinięcie techniczne.** Wymaga odpowiedniego napięcia, częstotliwości oraz upływu okresu zwłoki. Nie wolno mylić tego z odbudową P po FRT, podczas której urządzenie mogło nie utracić połączenia.

**Przykład.** Przykład: w banku B częstotliwość 49–50,05 Hz i minimum 60 s oczekiwania są elementami warunków powrotu.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „Resynchronizacja” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?

### 23. Składowe symetryczne

**Znaczenie.** Matematyczny rozkład niesymetrycznego układu trójfazowego.

**Rozwinięcie techniczne.** Składowa zgodna opisuje normalną kolejność faz, a przeciwna odwrotną. Pomagają rozdzielić reakcję prądu przy zwarciu symetrycznym i niesymetrycznym. Należy rozumieć je jako wielkości wektorowe.

**Przykład.** Przykład: zwarcie jednej fazy do ziemi wytwarza asymetrię, której nie pokaże jeden średni procent napięcia.

**Jak to sprawdzić w praktyce?** Wskaż punkt pomiaru, wielkość bazową i jednostkę. Następnie odróżnij wartość rzeczywistą od zadanej i stan regulacji od stanu ochrony. Sprawdź właściwą tabelę A, B/SN lub B/nn i przypisy w dokumencie źródłowym. Zapisz własnymi słowami, w jaki sposób błędna interpretacja pojęcia „Składowe symetryczne” może zmienić diagnozę przyczyn odłączenia lub ograniczenia mocy.

**Pytanie kontrolne.** Co musiałbyś zobaczyć w przebiegu rejestratora, aby potwierdzić wystąpienie opisanego zjawiska, i jakie informacje nadal pozostałyby nieznane?


## Zadanie integracyjne
Odtwórz przepływ sygnałów: **PCC → pomiar U/f/P/Q → zabezpieczenia i regulatory → sterowanie prądem falownika → ograniczenia Imax/Smax → raportowanie SCADA**. Oznacz funkcje, które działają w milisekundach, sekundach i minutach. Zaznacz, gdzie producent może stosować inną konwencję znaków. Nie wykonuj zmian parametrów działających zabezpieczeń w ramach ćwiczenia.
