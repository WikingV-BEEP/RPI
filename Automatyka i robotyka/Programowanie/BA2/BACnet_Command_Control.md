---
tags:
  - automatyka-i-robotyka
  - automatyka-i-robotyka/programowanie
  - automatyka-i-robotyka/programowanie/ba2
  - bacnet
  - beckhoff
  - command-control
  - plc
  - priority-array
  - programowanie
---

## BACnet – Command Control

### Jakie obiekty wspierają sterowanie poleceniami?

Obiekty typu:

- AO – Analog Output  
- AV – Analog Value  
- BO – Binary Output  
- BV – Binary Value  
- MSO – Multi-State Output  
- MSV – Multi-State Value  
- oraz niektóre obiekty prymitywne (primitive value objects)  

...mogą być wykorzystywane jako **obiekty sterujące** (command objects).

---

## Priority Array – sterowanie priorytetowe

Polecenia w obiektach BACnet są obsługiwane za pomocą **tablicy priorytetów** (*Priority Array*), która umożliwia nadanie różnym źródłom poleceń określonej ważności.

### Zasady działania:

- Najwyższy priorytet ma wartość **1**.
- Wartość **NULL** oznacza „brak wartości” dla danego priorytetu.
- Pozycja **17** to **Relinquish Default** – wartość przyjmowana, gdy żaden priorytet nie ma przypisanej wartości.
- Niektóre poziomy są **zarezerwowane**.

---

## Lista priorytetów

| **Priorytet** | **Zastosowanie**                                  |
| ------------- | ------------------------------------------------- |
| 1             | safety manual                                     |
| 2             | safety automatic                                  |
| 3             | free                                              |
| 4             | free                                              |
| 5             | global plant control                              |
| 6             | timer delay on/off (minimum On/Off time)          |
| 7             | free                                              |
| 8             | manual intervention                               |
| 9             | free                                              |
| 10            | free                                              |
| 11            | free                                              |
| 12            | free                                              |
| 13            | free                                              |
| 14            | free                                              |
| 15            | automatic program PLC                             |
| 16            | free                                              |
| 17            | relinquish default (wartość domyślna bez poleceń) |

> [!note]
> System wybiera najbliższy priorytet z przypisaną wartością od góry (1 → 17).

