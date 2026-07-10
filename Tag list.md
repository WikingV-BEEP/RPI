---
tags:
  - indeks
  - tagi
  - vault
  - standard-tagowania
---

# Tag list

## Status

Ten plik jest standardem tagowania vaulta, nie pełnym katalogiem każdego historycznego tagu. Pełna lista tagów po migracji zawierała setki pozycji i była zanieczyszczona literówkami oraz tagami skopiowanymi ze ścieżek folderów.

Aktualna zasada: tag ma pomagać filtrować notatki, a nie powtarzać całej lokalizacji pliku.

## Reguła główna

- Folder mówi, gdzie notatka mieszka.
- Link mówi, o czym lub z czym notatka jest powiązana.
- Tag mówi, jak notatkę filtrować.
- Indeks mówi, jak po dziale nawigować.

## Format tagów

Tagi zapisujemy wyłącznie w YAML frontmatter:

```yaml
---
tags:
  - indeks
  - gotowanie/przepis
  - typ/deser
  - status/do-przetestowania
---
```

Standard zapisu:

- lowercase,
- ASCII bez polskich znaków,
- kebab-case,
- hierarchia przez `/`,
- bez spacji,
- bez inline tagów w treści.

## Prefiksy kanoniczne

### Administracja vaulta

- `indeks`
- `vault`
- `tagi`
- `standard-tagowania`

### Typ i status

- `typ/...`
- `status/...`

Przykłady:

- `typ/deser`
- `typ/obiad`
- `typ/kolacja`
- `status/do-przetestowania`
- `status/przetestowane`
- `status/do-poprawy`

### Gotowanie

- `gotowanie/przepis`
- `gotowanie/skladnik`
- `gotowanie/technika`
- `gotowanie/makro-i-odzywianie`
- `dieta/...`
- `kuchnia/...`
- `pora/...`
- `skladnik/...`
- `technika/...`
- `makro/...`

Uwaga: dla gotowania obowiązuje lokalna notatka [[Gotowanie/System tagów]].

### Główne działy

Tag działu może istnieć, ale nie powinien bez potrzeby kopiować całej ścieżki folderu.

Dopuszczalne tagi działów:

- `akwarium-i-ryby`
- `automatyka-i-robotyka`
- `fotografia-i-montaz`
- `gotowanie`
- `gry`
- `ksiazki`
- `medycyna`
- `moda-i-styl`
- `motocykle`
- `ogrodnictwo`
- `praca`
- `programowanie`
- `rolnictwo`
- `rozwoj-osobisty`
- `stolarstwo`
- `zdrowie-uroda-i-silownia`

## Czego nie robić

- Nie tworzyć tagów tylko dlatego, że istnieje folder o takiej nazwie.
- Nie dublować tej samej informacji w folderze, linku i tagu naraz.
- Nie tworzyć nowych wariantów literówek, np. `siownia`, `skadniki`, `posiki`.
- Nie linkować do folderu, jeśli nie ma w nim notatki indeksowej.
- Nie używać `[[Lista indeksów]]` bez ścieżki, bo takich plików jest dużo.

## Linkowanie

Linki służą do bytów, pojęć i relacji. Jeśli notatka może mieć własną treść albo ma być używana w wielu miejscach, link jest właściwy.

Przykłady dobrych linków:

- `[[Gotowanie/Składniki/Tłuszcze i oleje/Margaryna|margaryny]]`
- `[[Programowanie/PLC/Lista indeksów|PLC]]`
- `[[Automatyka i robotyka/Elektryka/Terminologia/Lista indeksów|terminologia elektryczna]]`

Przykłady problematyczne:

- `[[Lista indeksów]]`
- `[[../Makro i odżywianie]]`
- `[[oliwa z oliwek]]`, jeśli taka notatka składnika jeszcze nie istnieje

## Znane długi migracyjne

Według audytu read-only vault miał:

- 386 notatek Markdown,
- 385 notatek z frontmatterem,
- 376 unikalnych tagów,
- 2528 wikilinków,
- 961 potencjalnie broken wikilinków,
- 108 orphan notes po pominięciu indeksów.

Najważniejsze długi:

- znormalizować `zdrowie-uroda-i-siownia` do `zdrowie-uroda-i-silownia`,
- znormalizować literówki typu `skadniki`, `posiki`, `sodkie`, `pompa-ciepa`, `materiay`,
- zdecydować, która lokalizacja BA2/BACnet jest kanoniczna,
- uzupełnić indeksy kategorii przepisów,
- przejrzeć największe skupiska broken linków: marynaty, kable elektryczne, ryż smakowy.

## Zasada aktualizacji tego pliku

Dopisywać tutaj tylko reguły, prefiksy i świadome wyjątki. Nie dopisywać automatycznie wszystkich tagów znalezionych w vaultcie.
