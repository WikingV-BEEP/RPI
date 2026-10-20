---
tags:
  - typ/dokumentacja
updated: 2026-10-20
---

# Migracja RPi na Windows 10

## Stan na 20.10.2026

Usługi Home Server przeniesiono na mini PC z Windows 10, host DESKTOP-T7FENTB. Zapis migracji potwierdza Docker Desktop, kontekst desktop-linux i silnik docker-desktop w WSL 2, architekturę x86_64. Ubuntu nie uczestniczyło w uruchamianiu usług; przejście na Debiana było omawiane, bez potwierdzenia wdrożenia.

To zestawienie łączy zapis wcześniejszej migracji, istniejącą dokumentację bramy i testy MCP z tego czatu. Nie jest nowym audytem procesów Windows ani zawartości RPi.

## Co przeniesiono

| Element | Status i dowód | Ograniczenia |
| --- | --- | --- |
| Obsidian MCP i vault | Migracja na Windows opisana w historii; odczyt folderów i notatki sprawdzony przez wtyczkę | Pełny katalog 13 narzędzi opisuje osobna notatka |
| Fitatu MCP | Backend w stosie Windows; profil i jadłospis odczytane przez wtyczkę | Pozostałe funkcje nie zostały przetestowane w tym czacie |
| MQTT, Mosquitto i Tuya bridge | Zapis migracji potwierdza testy MQTT; obecny odczyt zwraca trzy encje | Dwa gniazdka były nieosiągalne także z Windows; odczyt cache nie jest testem sterowania |
| Hevy Coach | Przeniesione stan i 35 rekordów deduplikacji według historii migracji; status i bieżący plan działają przez MCP | Nie sprawdzono tutaj wykonania zadania o 18:00 ani sobotniego raportu |
| Obsidian Sync | Historia migracji potwierdza test synchronizacji; starszy test bramy: ob sync-status kod 0 | Nie wykonano nowego testu pełnej synchronizacji |
| HTTPS i OAuth | Caddy i proxy w stosie Windows; wspólny endpoint /mcp i osobny stan OAuth bramy | Bieżący test narzędzi potwierdza połączenie używane przez ten czat, nie każdy wariant dostępu sieciowego |
| Home Server MCP Gateway i adapter Hevy | Osobny projekt home-server-gateway dodany na Windows | Pierwotnie 22 odczyty; obecnie katalog wtyczki pokazuje 42 narzędzia |

## Co pozostało poza migracją

Według końcowego przekazania wcześniejszej migracji Node-RED, TwinCAT, Filebrowser i panel Tuya pozostały poza zakresem i na RPi. Ich bieżącego stanu nie sprawdzano. Home Assistant nie ma tutaj potwierdzonej migracji — starsze notatki RPi opisują go jako skonfigurowany, niekoniecznie uruchomiony.

Stare harmonogramy Hevy i Obsidian Sync na RPi zostały według historii wyłączone, aby uniknąć podwójnego wykonania. Nie oznacza to wyłączenia wszystkich kontenerów RPi. Zachowaj kartę SD i stare dane jako możliwość powrotu do czasu sprawdzenia harmonogramów i kopii zapasowych.

## Architektura po migracji

Klient → HTTPS /mcp → Home Server Gateway z OAuth → oddzielne backendy Obsidian, Fitatu, MQTT i adapter Hevy. Broker MQTT i mostek Tuya obsługują komunikację z fizycznymi gniazdkami w LAN. Dane i harmonogramy są oddzielnym zagadnieniem od katalogu narzędzi MCP.

Ścieżki /home/admin/... w kontenerach oraz nazwa vaulta RaspberryPiHome są zachowane po migracji; same w sobie nie wskazują hosta RPi.

Przy końcu migracji odnotowano 11 kontenerów głównego stosu; następnie brama dodała dwa kolejne. Nie wykonano teraz docker ps, więc 13 nie jest świeżym pomiarem liczby działających kontenerów.

## Utrzymanie i sprawy do potwierdzenia

- Autostart Docker Desktop przy logowaniu był ostatnim wskazanym krokiem ręcznym. Kontenery miały politykę restartu; włączenie autostartu nie jest potwierdzone w odczytanej historii.
- Sprawdzić pierwsze rzeczywiste powiadomienie Hevy i sobotni raport po migracji; produkcja aktywna w coach_status nie dowodzi wykonania harmonogramu.
- Wyjaśnić łączność gniazdo1 i gniazdo3 oraz rozbieżności nazw gniazdo2.
- Oddzielnie potwierdzić obsługę SWITCH/ZMIEN_STAN przez mostek; patrz [[Moje urządzenia/Win 10 Home Automation/MCP MQTT i Tuya]].
- Utrzymywać kopie wolumenów danych i OAuth. Nie usuwać wolumenów podczas zwykłego restartu.
- Przed odtwarzaniem routingu uwzględnić ostrzeżenia o kopiach konfiguracji Caddy w notatce bramy.

## Źródła

- [[Moje urządzenia/Win 10 Home Automation/Home Server MCP Gateway]] — wdrożenie, transporty, operacje odtworzenia i historyczne testy.
- Czat „Przygotuj migrację usług z RPi (2)” — końcowe wyniki migracji i zakres wyłączeń, odczytany 20.10.2026.
- [[Moje urządzenia/Win 10 Home Automation/Testy MCP 2026-10-20]] — testy z aktualnego połączenia.
- [[Moje urządzenia/Raspberry PI 3B+/Docker i kontenery/Uruchomione kontenery i role]] — historyczna mapa stosu RPi.
