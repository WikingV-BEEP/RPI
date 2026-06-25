---
tags:
  - praca
  - praca/templatki-maili
---
W celu zresetowania sterownika do ustawień fabrycznych należy wymienić obraz systemu.

Po wymianie obrazu na nowy, login oraz hasło zrestartują się do ustawień domyślny, tj: Administrator, 1

Najnowszy obraz: [http://ftp.beckhoff.com/download/software/embPC-Control/CX90xx/CX9020/CE/TC3](http://ftp.beckhoff.com/download/software/embPC-Control/CX90xx/CX9020/CE/TC3 "http://ftp.beckhoff.com/download/software/embpc-control/cx90xx/cx9020/ce/tc3")

**Przed wymianą obrazu zalecam utworzenie kopii zapasowej obrazu istniejącego!**

W celu wymiany obrazu należy:

- Przy wyłączonym zasilaniu sterownika należy wyciągnąć kartę MicroSD
- Zawartość karty po utworzeniu kopii należy wyczyścić (proszę **nie formatować** karty)
- Zawartość folderu pochodzącego z pliku .zip należy wyeksportować bezpośrednio na kartę micro SD
- Następnie kartę należy ponownie włożyć do sterownika oraz należy uruchomić sterownik

Pierwsze uruchomienie nowego obrazu sterownika może potrwać dodatkowe parę sekund. Poprawnie uruchomiony nowy obraz powinien przełączyć diodę Power oraz TC na zielono.

Gdyby miał Pan jakieś pytania, proszę o kontakt.