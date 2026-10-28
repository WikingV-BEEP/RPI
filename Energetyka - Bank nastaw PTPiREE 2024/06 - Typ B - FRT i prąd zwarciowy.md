---
tags: [energetyka,typ_b,frt,zwarcie]
---
## FRT (Fault Ride Through) — sedno
Wyobraź sobie krótkie zwarcie w sieci i gwałtowny spadek napięcia. Gdyby wiele źródeł jednocześnie się odłączyło, problem systemowy mógłby się pogłębić. **FRT określa obszar napięcie–czas, w którym moduł powinien zachować pracę podczas zakłócenia**, zamiast bezwarunkowo wyłączać się od samego spadku napięcia.

## Kluczowe punkty krzywej B
W tabeli PTPiREE dla B (SN i nn) widnieją:
| Parametr napięciowy | p.u. | Parametr czasowy | s |
|---|---:|---|---:|
| Uret | 0,05 | tclear | 0,25 |
| Uclear | 0,05 | trec1 | 0,25 |
| Urec1 | 0,05 | trec2 | 0,25 |
| Urec2 | 0,85 | trec3 | 2,50 |

**Odczytaj koniecznie oryginalny wykres na s. 15 lub 33 PDF**. Same liczby nie pokazują całego kształtu i przebiegu przejściowego. W uproszczeniu wykres ilustruje pozostawanie w pracy przy bardzo niskim napięciu przez krótkie zakłócenie, a następnie rosnącą granicę napięcia podczas odbudowy.

## Szybki dodatkowy prąd bierny
Przy zwarciu poza instalacją wewnętrzną modułu parku energii dokument wymaga zdolności do generacji **dodatkowego prądu biernego**.
- Dla zwarcia symetrycznego: odpowiedź w składowej zgodnej proporcjonalna do zmiany napięcia i wzmocnienia **K1 = 2**.
- Próg aktywacji: **Utrig = 0,85 Un**; zakres nastawczy Utrig: 0,8–1,0 Un.
- Dla zwarcia niesymetrycznego: dodatkowo składowa przeciwna i **K2 = 2**.
- Wymagane zakresy nastawcze K1 i K2: **2–6 co 0,5**.
- Reakcja na spadek napięcia: **tr ≤ 30 ms**; ustalenie **ts ≤ 60 ms**; tolerancja **−10% do +20%**.
- Poniżej **0,15 Un** dokument nie definiuje wymagań w zakresie wsparcia prądem biernym.
- Obowiązuje ograniczenie prądu fazowego **Imax**; przy odpowiednich zwarciach priorytet ma prąd bierny.
- Musi istnieć możliwość zablokowania funkcji wsparcia z PPM.

## Wzory — jak o tym myśleć
W ujęciu względnym: **ΔIQ1 = K1 × ΔU1**. Znak i dokładna interpretacja wyjściowego prądu zależą od przyjętej konwencji i wektorowej implementacji; **nie mapuj wartości bezpośrednio na nastawę falownika bez dokumentacji producenta**.
Dla niesymetrii dodaje się wektorową odpowiedź składowej przeciwnej: **ΔIQ2 = K2 × ΔU2**. Sumowanie jest **wektorowe**, nie zwykłe dodawanie modułów.

## Powrót mocy po zwarciu
- proces rozpoczyna się po powrocie napięcia do co najmniej **0,9 Un**;
- maksymalny czas odbudowy **5 s od usunięcia zwarcia**;
- cel: **90% mocy sprzed zwarcia**, jeśli jest dostępne źródło energii pierwotnej;
- tolerancja uchybu ustalonego **10%**, bez nietłumionych oscylacji.

Źródło: PDF s. 15–18 (B/SN), s. 33–36 (B/nn).