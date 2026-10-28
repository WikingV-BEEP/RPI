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