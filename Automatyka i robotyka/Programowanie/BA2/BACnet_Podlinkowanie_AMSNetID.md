---
tags:
  - bacnet
  - programowanie
  - plc
  - beckhoff
  - amsnetid
  - komunikacja
  - io_linking
---

## BACnet – Podlinkowanie `AMSNetID` w konfiguracji urządzenia

Aby poprawnie skonfigurować komunikację pomiędzy sterownikiem PLC a urządzeniem BACnet w TwinCAT 3, konieczne jest **podlinkowanie identyfikatora `AMSNetId`** w konfiguracji urządzenia.

---

## Gdzie to ustawić?

W drzewie projektu:

I/O → Devices → Device 1 (BACnet IP) → Inputs → Device Status → AmsNetId

---

## Dlaczego to ważne?

- `AMSNetId` musi wskazywać **adres logiczny PLC**, z którym urządzenie BACnet ma się komunikować.  
- Jeśli nie zostanie podlinkowany do odpowiedniej zmiennej w kodzie sterownika (np. `stComParams.AmsNetId`), komunikacja może nie działać poprawnie.  
- Poprawne przypisanie pozwala na automatyczne sterowanie i odczyt danych między warstwą I/O a logiką sterownika.

---

## Jak to zrobić?

1. Przejdź do `Device Status → AmsNetId`.  
2. Kliknij prawym przyciskiem myszy → `Link to...`.  
3. Wybierz zmienną typu `T_AmsNetId` w kodzie sterownika, np.:  
   `stComParams : ST_BACnetParams;`  
   (gdzie `ST_BACnetParams` zawiera pole `AmsNetId : T_AmsNetId`)  
4. Zatwierdź powiązanie i przebuduj projekt.

---

## Efekt

Po poprawnym podlinkowaniu i uruchomieniu sterownika, urządzenie BACnet będzie wiedziało, z którym PLC ma się komunikować, a TwinCAT umożliwi pełną wymianę danych.

> [!note]
> W systemach rozproszonych lub z wieloma sterownikami to pole staje się **kluczowe** dla prawidłowej identyfikacji w magistrali ADS.
