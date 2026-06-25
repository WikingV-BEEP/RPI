---
tags:
  - automatyka-i-robotyka
  - automatyka-i-robotyka/programowanie
  - automatyka-i-robotyka/programowanie/ba2
  - beckhoff
  - dpad
  - plc
  - programowanie
  - zagniezdzenia
---

## Przykład hierarchii zagnieżdżeń DPAD (TwinCAT 3)

Poniższy przykład przedstawia kompletną 5‑poziomową strukturę obiektów typu BA (Building Automation) w oparciu o mechanizm `iParent`. Struktura ta odpowiada koncepcji DPAD i może być wykorzystana m.in. do wizualizacji w HMI lub organizacji danych projektowych.

## Opis poziomów

1. **Level 1** – Budynek (`FB_BA_View`)
2. **Level 2** – Punkt automatyzacji (`FB_AS1`)
3. **Level 3** – Instalacja (`FB_Anlage1`)
4. **Level 4** – Agregat (`FB_BA_View`)
5. **Level 5** – Podagregat / funkcja (`FB_BA_View`)

## Kod przykładowy

```iec
// Level 1 – Budynek
G : FB_BA_View := (
    sObjectName  := 'CaRel85',
    sDescription := 'CaRe Haller Straße 185'
);

// Level 2 – Punkt automatyzacji
AS01 : FB_AS1 := (
    iParent      := G,
    sObjectName  := '[AS01]',
    sDescription := '[Automationsstation 1]'
);

// Level 3 – Instalacje
Anlage1 : FB_Anlage1 := (
    iParent      := G,
    sObjectName  := '[A1]',
    sDescription := '[Anlage 1]'
);

Anlage2 : FB_BA_View := (
    iParent      := G,
    sObjectName  := '[A2]',
    sDescription := '[Anlage 2]'
);

// Level 4 – Agregat
Agg1 : FB_BA_View := (
    iParent      := Anlage1,
    sDescription := '[Aggregat 1]'
);

// Level 5 – Podagregaty
SubAgg1 : FB_BA_View := (
    iParent      := Agg1,
    sDescription := '[Subaggregat 1]'
);

SubAgg2 : FB_BA_View := (
    iParent      := Agg1,
    sDescription := '[Subaggregat 2]'
);

## Wizualna struktura hierarchii

```
CaRel85
├── AS01
├── A1
│   └── Aggregat 1
│       ├── Subaggregat 1
│       └── Subaggregat 2
└── A2
```

## Przykład pełnej ścieżki identyfikacyjnej

Zależnie od separatorów konfigurowanych w DPAD:

```
CaRel85.A1...Aggregat1-02
```

> [!note]  
> Identyfikacja obiektów w systemie bazuje na ich nazwach (`sObjectName`) oraz pozycji w hierarchii określanej przez `iParent`.

> [!tip]  
> Można rozszerzyć strukturę o kolejne poziomy, np. komponenty lub funkcje kontrolne, zachowując zasadę nadrzędności przez `iParent`.

```

Gotowe do przeklejenia do Obsidiana.

[99% | 08 10:20 | 5]
```