---
tags:
  - programowanie/plc
  - programowanie/plc/ba2
  - beckhoff
  - dpad
  - plc
  - programowanie
  - struktura-zagniezdzen
---
## DPAD – Struktura i zasady budowy hierarchii obiektów w BA

System automatyki budynkowej (BA) w środowisku TwinCAT 3 umożliwia budowę hierarchicznej struktury obiektów z wykorzystaniem koncepcji DPAD (`FB_BA_CarelessDPAD`) oraz bloku `FB_BA_View`.

---

## 1. Poziomy i separatory w strukturze DPAD

Struktura `DPAD` definiuje poziomy zagnieżdżeń, ich typy oraz separatory stosowane do budowy ścieżek identyfikacyjnych obiektów.

### Definicja poziomów:

| **Level** | **Typ węzła (`eNodeType`)**    | **Separator obiektu** | **Separator opisu** | **Cyfry indeksu** |
|-----------|-------------------------------|------------------------|---------------------|-------------------|
| 1         | eBuilding                     | `.`                    | ` - `               | 0                 |
| 2         | eTrade                        | `.`                    | ` - `               | 0                 |
| 3         | ePlant                        | `..`                   | ` - `               | 0                 |
| 4         | eAggregate                    | `...`                  | ` - `               | 0                 |
| 5         | eFunction                     | `-`                    | ` - `               | 2                 |

---

## 2. Przykład konfiguracji tablicy `aIdentifier`

aIdentifier := [  
(eNodeType:=E_BA_NodeType.eBuilding,  sSeparator_ObjectName:='.',   sSeparator_Description:=' - ', nIndexDigits:=0),  
(eNodeType:=E_BA_NodeType.eTrade,     sSeparator_ObjectName:='.',   sSeparator_Description:=' - ', nIndexDigits:=0),  
(eNodeType:=E_BA_NodeType.ePlant,     sSeparator_ObjectName:='..',  sSeparator_Description:=' - ', nIndexDigits:=0),  
(eNodeType:=E_BA_NodeType.eAggregate, sSeparator_ObjectName:='...', sSeparator_Description:=' - ', nIndexDigits:=0),  
(eNodeType:=E_BA_NodeType.eFunction,  sSeparator_ObjectName:='-',   sSeparator_Description:=' - ', nIndexDigits:=2)  
]

---

## 3. Przykład ścieżki identyfikacyjnej

`B1.HVAC..AHU...MIXER-01`

Gdzie:
- `B1` – budynek (Level 1),
- `HVAC` – branża (Level 2),
- `AHU` – instalacja (Level 3),
- `MIXER` – agregat (Level 4),
- `01` – funkcja z indeksem (Level 5)

---

## 4. Zasady budowy drzewa z `FB_BA_View`

### Kluczowe reguły:

- Obiekty `FB_BA_View` są **podstawowymi węzłami** struktury.  
- Widok znajdujący się najwyżej (bez `iParent`) jest automatycznie traktowany jako **Level 1**.  
- `FB_BA_View` może zawierać inne `FB_BA_View` oraz obiekty funkcyjne (np. `FB_Anlage1`, `FB_AS1`, itp.).
- Obiekty funkcyjne **nie mogą zawierać widoków**.

### Przykład poprawnego budowania drzewa:

G : FB_BA_View := (sObjectName := 'CaRel85');  
A1_View : FB_BA_View := (iParent := G);  
A1 : FB_Anlage1 := (iParent := A1_View);  
Agg1 : FB_BA_View := (iParent := A1_View);  
Func1 : FB_Motor := (iParent := Agg1);

---

## 5. Podsumowanie

- Struktura DPAD oparta jest o 5 poziomów logicznych.
- Widoki (`FB_BA_View`) są niezbędne do budowy hierarchii – stanowią podstawę drzewa.
- Tylko `FB_BA_View` może być rodzicem dla innych elementów.
- Separator i typ węzła są konfigurowalne w tablicy `aIdentifier`.

> [!tip]
> Prawidłowe stosowanie `FB_BA_View` i `DPAD` pozwala tworzyć uporządkowane, zrozumiałe i skalowalne struktury automatyki budynkowej – zarówno na poziomie kodu, jak i wizualizacji HMI.

