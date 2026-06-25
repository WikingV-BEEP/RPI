---
tags:
  - programowanie
  - programowanie/plc
  - programowanie/plc/ba2
  - beckhoff
  - dpad
  - bacnet
---

## Przykład implementacji zagnieżdżenia

Każdy obiekt niższego poziomu przypisuje swój nadrzędny element za pomocą parametru `iParent`. Pozwala to tworzyć hierarchię zgodną z ustaloną strukturą DPAD.

Powiązane notatki:

- [[Struktura_zagniezdzen_DPAD]]
- [[Hierarchia_zagniezdzen_DPAD_przyklad]]
- [[Zasady_budowy_drzewa_BA_View]]

## Założenie

Przykład pokazuje logiczne zagnieżdżenie:

1. budynek,
2. stacja automatyki,
3. instalacja,
4. agregat / grupa funkcjonalna,
5. obiekt funkcyjny.

Najważniejsza zasada: dziecko wskazuje rodzica przez `iParent`.

## Przykład kodu

```iecst
// Level 1 – Budynek
G : FB_BA_View := (
    sObjectName    := 'CaRel85',
    sDescription   := 'CaRe Haller Straße 185'
);

// Level 2 – Automationsstation
AS01 : FB_AS1 := (
    iParent        := G,
    sObjectName    := 'AS01',
    sDescription   := 'Automationsstation 1'
);

// Level 3 – Instalacja / plant
AHU01_View : FB_BA_View := (
    iParent        := AS01,
    sObjectName    := 'AHU',
    sDescription   := 'Air Handling Unit'
);

// Level 4 – Agregat / grupa funkcjonalna
FAN_View : FB_BA_View := (
    iParent        := AHU01_View,
    sObjectName    := 'FAN',
    sDescription   := 'Supply fan'
);

// Level 5 – Funkcja / obiekt końcowy
FAN_StartCmd : FB_BA_BinaryOutput := (
    iParent        := FAN_View,
    sObjectName    := 'StartCmd',
    sDescription   := 'Fan start command'
);

FAN_RunFb : FB_BA_BinaryInput := (
    iParent        := FAN_View,
    sObjectName    := 'RunFb',
    sDescription   := 'Fan run feedback'
);

FAN_SpeedSp : FB_BA_AnalogOutput := (
    iParent        := FAN_View,
    sObjectName    := 'SpeedSp',
    sDescription   := 'Fan speed setpoint'
);
```

## Oczekiwana logika drzewa

```text
CaRel85
└── AS01
    └── AHU
        └── FAN
            ├── StartCmd
            ├── RunFb
            └── SpeedSp
```

## Jak czytać ten przykład

- `G` jest najwyższym widokiem i nie ma `iParent`.
- `AS01` jest podpięte pod `G`.
- `AHU01_View` jest podpięte pod `AS01`.
- `FAN_View` jest podpięte pod `AHU01_View`.
- Obiekty końcowe są podpięte pod `FAN_View`.

## Najczęstsze błędy

- brak `iParent` w obiekcie, który powinien być częścią drzewa,
- przypięcie obiektu do złego poziomu,
- mieszanie nazw technicznych z opisami użytkowymi,
- tworzenie zbyt płaskiej struktury,
- pomijanie widoków pośrednich, przez co HMI i BACnet Explorer robią się nieczytelne,
- stosowanie różnych konwencji nazw w jednym projekcie.

## Minimalna reguła projektowa

Jeżeli obiekt ma być czytelny w eksploratorze, HMI albo BACnet, powinien mieć:

- jednoznacznego rodzica `iParent`,
- krótkie `sObjectName`,
- czytelne `sDescription`,
- miejsce w strukturze zgodne z DPAD.

## Schemat do kopiowania

```iecst
Parent_View : FB_BA_View := (
    sObjectName  := 'Parent',
    sDescription := 'Parent description'
);

Child_View : FB_BA_View := (
    iParent      := Parent_View,
    sObjectName  := 'Child',
    sDescription := 'Child description'
);

Final_Object : FB_BA_AnalogValue := (
    iParent      := Child_View,
    sObjectName  := 'Value',
    sDescription := 'Measured value'
);
```

## Do sprawdzenia w konkretnym projekcie

- czy dany blok przyjmuje `iParent` jako referencję do widoku,
- czy używany typ bloku końcowego jest właściwy dla obiektu BACnet,
- czy nazwy są zgodne z konwencją klienta,
- czy wygenerowana ścieżka jest zgodna z [[Struktura_zagniezdzen_DPAD|DPAD]],
- czy obiekt jest widoczny w BA2 Explorer.
