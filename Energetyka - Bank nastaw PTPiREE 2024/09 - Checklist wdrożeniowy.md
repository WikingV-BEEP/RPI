---
tags: [energetyka,checklista,wdrozenie]
---
## Lista dla inżyniera — NIE jako gotowy projekt nastaw
- [ ] Potwierdzić **kwalifikację modułu do A/B** i podstawę formalną.
- [ ] Potwierdzić napięcie i miejsce przyłączenia **nn/SN**, wskazany punkt pomiarowy oraz Ubase/Un.
- [ ] Uzyskać aktualne **warunki przyłączenia i wymagania operatora**.
- [ ] Zweryfikować wersję dokumentu PTPiREE i certyfikaty urządzenia.
- [ ] Rozróżnić metody pomiaru: RMS, średnia 10-minutowa, chwilowy przebieg, df/dt.
- [ ] Zmapować zabezpieczenia U<, U>, U>>, f<, f>, RoCoF do odpowiednich funkcji producenta.
- [ ] Sprawdzić konflikty czasów i progów z **FRT** (zwłaszcza typ B).
- [ ] Potwierdzić parametry LFSM-O, Q(U), P(U) oraz ewentualny regulator nadrzędny.
- [ ] Dla B: sprawdzić obszar FRT, K1/K2, Utrig, Imax, tr, ts oraz odbudowę P.
- [ ] Sprawdzić zachowanie podczas i po odłączeniu (resynchronizacja).
- [ ] Zweryfikować **konwencję znaku Q** i mapowanie rejestrów SCADA/PLC.
- [ ] Sprawdzić zgodność każdej nastawy z dokumentacją producenta i projektem.
- [ ] Wykonać przewidziane przepisami/projektem testy odbiorowe przez uprawnioną osobę; zachować protokoły.
- [ ] Zarchiwizować zatwierdzony zestaw nastaw, wersję firmware oraz dokumentację powykonawczą.

## Praktyka dla automatyka
Do rejestracji zdarzeń PLC/SCADA warto rozdzielić sygnały:
- status sieci i częstotliwość;
- napięcie w zdefiniowanym punkcie pomiaru;
- P, Q i ograniczenia P;
- aktywny tryb: Q(U), P(U), LFSM-O, FRT;
- przyczynę i czas zaprzestania generacji;
- status ponownego przyłączenia i komunikaty błędów.

**Nie programuj własnej funkcji ochronnej PLC jako zamiennika certyfikowanych zabezpieczeń falownika** bez pełnej oceny bezpieczeństwa i zgodności.

Powiązania: [[Leksykon - Bank Nastaw]].