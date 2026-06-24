---
tags:
  - programowanie
  - plc
  - beckhoff
  - ba_view
  - struktura_drzewa
  - dpad
---

## Zasady budowy drzewa struktury w oparciu o `FB_BA_View`

W systemie automatyki budynkowej (BA) w środowisku TwinCAT struktura drzewa oparta jest na blokach typu `FB_BA_View`. Ich odpowiednie użycie i pozycjonowanie w kodzie wpływa na poziom oraz możliwość dalszego zagnieżdżania obiektów.

---

## Kluczowe zasady

### 1. `FB_BA_View` jako podstawowy element struktury

- Obiekty typu `FB_BA_View` odpowiadają **widokom logicznym** i **hierarchicznym**.
- Element `FB_BA_View` **może zawierać** inne obiekty, np.:
  - `FB_AS1`
  - `FB_Anlage1`
  - inne `FB_BA_View` (zagnieżdżenia)

### 2. Poziom `Level 1`

- Najwyżej w hierarchii znajdujący się blok `FB_BA_View` (bez `iParent`) jest automatycznie traktowany jako **poziom 1** (root – np. budynek).
- Od niego odchodzą wszystkie kolejne poziomy drzewa (`iParent := ...`).

### 3. `FB_BA_View` jako węzeł pośredni

- Aby zagnieździć obiekt w drzewie, **musi** on być podpięty pod inny `FB_BA_View`.
- To oznacza, że:
  - **widoki (`BA_View`) mogą zawierać obiekty funkcjonalne**,  
  - **obiekty funkcjonalne (np. `FB_Anlage1`) nie mogą zawierać widoków (`BA_View`)**.

> [!important]
> Struktura może być budowana wyłącznie przez `FB_BA_View`, jeśli zależy nam na wielopoziomowej hierarchii.

---

## Przykład logiczny
```iec
// Level 1 – Budynek
G : FB_BA_View := ( ... ); // root – automatycznie Level 1

// Level 2 – Instalacja
A1 : FB_Anlage1 := (
    iParent := G,
    ...
);

// Level 3 – Agregat
Agg1 : FB_BA_View := (
    iParent := A1, // nieprawidłowe! Anlage1 nie może być parentem dla BA_View
    ...
);
```