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