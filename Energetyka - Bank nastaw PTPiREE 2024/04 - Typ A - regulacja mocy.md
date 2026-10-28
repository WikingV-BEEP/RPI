---
tags: [energetyka,typ_a,regulacja]
---
## Po co regulacja, skoro są zabezpieczenia?
Zabezpieczenie jest rozwiązaniem ostatecznym. Funkcje regulacyjne próbują wcześniej ograniczyć wpływ falownika na sieć.

## LFSM-O — reakcja na wzrost częstotliwości
- próg aktywacji **50,2 Hz**;
- **statyzm 5%**;
- **Pref**: faktyczna moc czynna w chwili osiągnięcia 50,2 Hz.
Gdy częstotliwość rośnie ponad próg, falownik ogranicza P zgodnie z charakterystyką. Nie myl **50,2 Hz (start regulacji)** z **51,5 Hz (próg zabezpieczenia)**.

## Dopuszczalna redukcja P przy spadku f
- początek zakresu: **49 Hz**;
- dokument określa **2% mocy maksymalnej na każdy 1 Hz spadku poniżej 49 Hz** jako charakterystykę dopuszczalnej redukcji.
To nie jest polecenie natychmiastowego wyłączenia przy 49 Hz.

## Q(U) — moc bierna zależna od napięcia
Dla ogólnego profilu typu A:
- **Uprog1 = 0,94 p.u.**, **Uprog2 = 1,06 p.u.**;
- **Umin = 0,92 p.u.**, **Umax = 1,08 p.u.**;
- granice mocy biernej **±0,4843 × PD** (cos φ ≈ 0,9);
- statyzm **2,222**.
Dokument przewiduje też **specyficzny wariant Q(U) dla falowników jednofazowych**, w którym wskazano m.in. Uprog2 = 1,06 p.u. i Umax = 1,08 p.u. Nie zakładaj bez weryfikacji, że krzywa jednofazowa jest identyczna z pełną krzywą.

**Sens fizyczny:** moc bierna pomaga kształtować napięcie, a nie jest bezpośrednio „produkowaną energią” w kWh.

## P(U) — redukcja mocy czynnej przy wysokim U
- początek **Upr = 1,08 p.u.**; dla 230 V daje **248,4 V**;
- **statyzm 2,5**;
- **Pmax** w tym opisie to **maksymalna aktualnie dostępna moc generowana przez PV**, względem której wykonywana jest redukcja.
Nie myl P(U) przy 248,4 V z zabezpieczeniem U> przy 253 V. Wartości te mają inne role.

## Ponowne przyłączenie typu A
Po ustabilizowaniu sieci dokument podaje łączne warunki:
1. częstotliwość **49–50,05 Hz**;
2. **60 s** zwłoki od powrotu częstotliwości do zakresu;
3. narastanie generacji maksymalnie **10% mocy maksymalnej na minutę**.

Dodatkowe wymagania konkretnego falownika i operatora nadal obowiązują.

Źródło: PDF s. 3–6. Dalej: [[05 - Typ B - podstawy i zabezpieczenia]].