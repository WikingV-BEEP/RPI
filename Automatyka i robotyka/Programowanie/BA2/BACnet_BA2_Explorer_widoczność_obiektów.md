---
tags:
  - automatyka-i-robotyka
  - automatyka-i-robotyka/programowanie
  - automatyka-i-robotyka/programowanie/ba2
  - ba2
  - bacnet
  - beckhoff
  - dpad
  - explorer
  - programowanie
  - visualisation
---

## Narzędzie: Building Automation Site Explorer

`Building Automation Site Explorer` to narzędzie diagnostyczne pozwalające na **wizualne przeglądanie struktury obiektów BA** wygenerowanych w TwinCAT 3.0 z użyciem dodatku **BA2 (Building Automation 2)**.

---

## Kluczowe cechy

- Wyświetlana jest **pełna hierarchia DPAD** utworzona za pomocą `FB_BA_View`, `FB_BA_CarelessDPAD`, klas zdarzeń, itp.
- Pokazuje:
  - nazwy obiektów (`Object Name`),
  - opisy (`Description`),
  - symbole (`Symbol path`, `Symbol name`),
  - poziom zagnieżdżenia (`DPAD Level`),
  - indeksy i identyfikatory,
  - typy funkcjonalne (`Node type`, `Purpose`, `Operational type`).

---

## Widoczność obiektów

- **Widoczne są tylko obiekty BACnet** wygenerowane przez **dodatek BA2**.
- Obiekty stworzone ręcznie (standardowe bloki bez `FB_BA_View`, bez DPAD) **nie są wyświetlane**.
- Narzędzie nie pokazuje zmiennych globalnych, wewnętrznych, ani danych spoza zdefiniowanej struktury BA.

---

## Przykład – struktura:

127.0.0.1:1:851  
├── G (`CaRe185`)  
│   └── AS01 (`Automationsstation 1`)  
│       └── Device  
│           └── Project  
│               └── Meldeklassen  
│                   ├── LifeSafety (NC_10)  
│                   ├── SafetyMsg (NC_20)  
│                   ├── AlarmMsg (NC_30)  
│                   └── …  

---

## Właściwości obiektu (np. AlarmMsg)

- **DPAD Level:** 4  
- **Identifier:** EC:30  
- **Subject info:** `Hash`, `Index`, `Type hash`  
- **Symbol:** `AS01.MAIN.AS01.Meldeklassen.AlarmMsg`  
- **Description:** `Notification-Class-Objekt`  

---

## Wnioski

- `Site Explorer` to doskonałe narzędzie do weryfikacji poprawności struktury BACnet generowanej w projekcie.
- Umożliwia szybką diagnostykę poprawności przypisania DPAD, identyfikatorów oraz klas alarmowych.
- Wymaga poprawnego przygotowania projektu w oparciu o bloki z BA2 – **obiekty niezgodne nie będą widoczne**.

> [!tip]
> Jeśli obiekt nie pojawia się w `Site Explorer`, sprawdź czy jest zdefiniowany w `FB_BA_View`, ma przypisany `iParent`, oraz czy znajduje się wewnątrz struktury `DPAD`.

