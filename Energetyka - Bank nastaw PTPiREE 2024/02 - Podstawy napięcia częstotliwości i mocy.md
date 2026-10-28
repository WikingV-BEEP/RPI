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