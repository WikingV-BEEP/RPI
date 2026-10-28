---
tags: [energetyka,cwiczenia,tutorial]
---
## Przykład 1: napięcie
Nominalne jednofazowe **230 V**. Ile wynosi 1,08 p.u.? **248,4 V**. Funkcja P(U) typu A może od tego progu redukować P zgodnie z charakterystyką, **nie musi jeszcze wystąpić U>>**.

## Przykład 2: częstotliwość 50,4 Hz
Czy przekroczono próg LFSM-O? **Tak**, bo 50,4 > 50,2 Hz. Czy przekroczono zabezpieczenie f>? **Nie**, bo 50,4 < 51,5 Hz. Wniosek: **redukcja P bez automatycznego awaryjnego wyłączenia z samej wartości f**.

## Przykład 3: szybkie zanikanie sieci
RoCoF = 2,2 Hz/s. Dla typu B przekracza próg 2 Hz/s; dla A **nie** przekracza progu 2,5 Hz/s. To dydaktyczne porównanie progów, nie wystarczająca podstawa do stwierdzenia faktycznego zadziałania urządzenia bez uwzględnienia pomiaru, czasu i implementacji LoM.

## Przykład 4: symetryczne zwarcie
Napięcie spada do 0,5 p.u. Na czas zgodny z wymaganym obszarem FRT moduł B **nie powinien bezrefleksyjnie odłączyć się**, a jego funkcja wsparcia prądem biernym może zareagować. O tym, czy zdarzenie mieści się w FRT, decyduje **cały przebieg U(t)**, a nie jedna wartość napięcia.

## Pytania samokontrolne (odpowiedz bez zaglądania)
1. Jak odróżnisz zabezpieczenie U> od P(U)?
2. Czy 50,2 Hz jest progiem wyłączenia?
3. Co oznacza **p.u.**?
4. Co jest inne w RoCoF dla A i B?
5. Czy falownik B przyłączony do nn jest możliwy?
6. Czym różni się powrót po odłączeniu od odbudowy mocy po FRT?
7. Dlaczego **Q przy P=0** ma znaczenie?
8. Czym różni się „wymagana nastawa” od „minimalnego zakresu nastawczego”?

## Odpowiedzi
1. U> kończy generację po kryterium; P(U) ją ogranicza.
2. Nie, jest progiem LFSM-O; f> wynosi 51,5 Hz.
3. Wartość względna do zdefiniowanej bazy.
4. A: 2,5 Hz/s i max 0,5 s; B: 2 Hz/s i max 0,2 s.
5. Tak — dokument ma taką osobną sekcję.
6. Po FRT można nie tracić synchronizmu; po odłączeniu trzeba spełnić warunki ponownego przyłączenia.
7. Pozwala wspomagać napięcie bez wytwarzania P.
8. Nastawa to konkretna wartość; zakres — możliwości konfiguracyjne.

Dalej: [[09 - Checklist wdrożeniowy]].