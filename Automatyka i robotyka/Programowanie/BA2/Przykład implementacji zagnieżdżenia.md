---
tags:
  - automatyka-i-robotyka
  - automatyka-i-robotyka/programowanie
  - automatyka-i-robotyka/programowanie/ba2
---
## Przykład implementacji zagnieżdżenia

Każdy obiekt niższego poziomu przypisuje swój nadrzędny element za pomocą parametru `iParent`. Pozwala to tworzyć hierarchię zgodną z ustaloną strukturą DPAD.

### Przykład kodu

```iec
// Level 1 – Budynek
G : FB_BA_View := (
    sObjectName    := 'CaRel85',
    sDescription   := 'CaRe Haller Straße 185'
);

// Level 2 – Automationsstation
AS01 : FB_AS1 := (
    iParent        := G,
    sObjectName    := '[AS01]',
    sDescription   := '[Automationsstation 1]'
);
