# Wznowienie Hevy Coach

1. Przeczytaj STATUS.md, USTALENIA.md, DECYZJE.md i ODTWARZANIE.md.
2. Sprawdź git status; pozostaw wcześniejsze zmiany innych projektów.
3. Kod twórz lokalnie w hevy-coach; wdrażaj przez SSH do /home/admin/hevy-coach.
4. RPi admin, Ethernet 192.168.1.38, Wi-Fi 192.168.1.37. Dane logowania są w rozmowie; nie zapisuj ich do dokumentacji.
5. Odcisk SSH ED25519: SHA256:NxwDWuLwaRodKQSfWuLHF1t+dfjPY83v3ChwS02j3J8.
6. Najpierw sprawdź rzeczywisty stan, var/RUN_STATUS.md, var/analysis/usage.json i pokwitowania var/deliveries.
7. Historia Hevy została potwierdzona przez użytkownika jako kompletna 2026-09-12. Ostatni trening: 2026-08-04. Stan.md revision 1. Nie pytaj ponownie o tę samą informację.
8. Dzienny test ntfy dotarł do aplikacji — potwierdzenie użytkownika. Oba testy 2026-09-12 mają pokwitowania sent; nie wysyłaj ponownie.
9. Hevy READ ONLY, stdio. Bez nowych portów, zmian innych MCP i źródłowych notatek.
10. Użytkownik zaakceptował aktywację 2026-09-12. Harmonogram JEST AKTYWNY: codziennie 18:00, sobota 09:00 Europe/Warsaw. Nie aktywuj go ponownie i nie wyłączaj bez przyczyny lub polecenia.
11. Sprawdź var/activation.json oraz rzeczywisty stan timerów. Etapy 0–6 ukończone; dalsze zgłoszenia dotyczą obsługi działającego coacha. Przed zmianą config.json zachowaj kopię; nie nadpisuj sekretów ani pamięci.
12. Pełny MCP działa w Codex CLI na RPi. Nie obiecuj działania w zwykłym ChatGPT ani Windows, dopóki nie zostanie podłączony ten kanał.
13. Po każdym etapie aktualizuj STATUS.md lokalnie i na RPi oraz Wdrożenie.md w Obsidianie.

Windows sandbox zgłaszał helper_unknown_error; zatwierdzone operacje działały z require_escalated.
Przy wyczerpaniu limitu lub odrzuceniu kontroli zapisz dokładny ukończony etap i błąd. Nie resetuj pamięci i nie ponawiaj wysłanych powiadomień.
