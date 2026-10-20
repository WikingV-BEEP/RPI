
Wdrożono 20.10.2026 (Europe/Warsaw) na DESKTOP-T7FENTB, Docker Desktop / desktop-linux. Gotowy endpoint: **https://wikingv.servehalflife.com/mcp**. Połączenie w interfejsie ChatGPT pozostaje do wykonania przez użytkownika; nie potwierdzono dostępności dodawania własnych integracji na jego koncie ani połączenia z infrastruktury ChatGPT. Próba otwarcia publicznych metadanych przez narzędzie WWW zakończyła się błędem narzędzia, więc nie stanowi potwierdzenia dostępu spoza LAN.

## Dodanie do ChatGPT

Aktualna [oficjalna instrukcja OpenAI](https://developers.openai.com/api/docs/guides/custom-mcp-server) opisuje ChatGPT w przeglądarce: Plugins → plus → Add custom MCP server. Nazwy i dostępność funkcji zależą od konta i zasad workspace. Jeśli dostępna jest starsza sekcja Aplikacje / Connectors z opcją tworzenia własnego połączenia, użyj tej opcji.

| Pole                             | Wartość                                           |
| -------------------------------- | ------------------------------------------------- |
| Nazwa                            | Home Server                                       |
| Opis                             | Odczyt Obsidian, Fitatu, MQTT/Tuya i Hevy Coach   |
| Server URL                       | `https://wikingv.servehalflife.com/mcp`           |
| Authentication                   | OAuth (wyłącznie OAuth, bez trybu mieszanego)     |
| Zakresy, jeśli jest takie pole   | `home:read offline_access`                        |
| Rejestracja klienta              | DCR; brak wymaganego client secret                |
| Alternatywny statyczny client ID | `chatgpt` (publiczny klient, client secret pusty) |

W formularzu **Home Server MCP** użyj dotychczasowego hasła logowania OAuth do Obsidian MCP. Hasło nie jest wyświetlane w dokumentacji. Jest to jedno logowanie do bramy. Loginy i klucze Fitatu oraz Hevy pozostają na serwerze i nie są przekazywane do ChatGPT.

Po utworzeniu zainstaluj integrację i wybierz ją w rozmowie. Test: „Użyj Home Server: wywołaj mqtt__list_entities oraz hevy__coach_status. Nie zmieniaj danych ani urządzeń”. Powinno być widocznych 22 narzędzia. Do potwierdzenia połączenia zachowaj stare integracje. Brak opcji dodania własnego MCP lub błąd połączenia wymaga informacji od użytkownika; nie oznacza automatycznie awarii serwera. Telefon poza Wi-Fi może dodatkowo sprawdzić publiczny URL metadanych `https://wikingv.servehalflife.com/.well-known/oauth-protected-resource/mcp`.

## Architektura i zakres

Osobny projekt Compose `home-server-gateway`, dwa nowe kontenery na istniejącej sieci `home-server-migration_migration`. Brama oraz adapter nie publikują żadnego portu Windows. Caddy otrzymał cztery dodatkowe trasy i został przeładowany bez restartu. Stare proxy, trasy, klienci i bazy OAuth pozostały aktywne.

| Prefiks | Backend / transport | Udostępnione odczyty |
| --- | --- | --- |
| `obsidian__` | `http://obsidian-mcp:8000/sse`, SSE | search, fetch, list_dir, tree, list_notes, file_info, batch_file_info (7) |
| `fitatu__` | `http://fitatu-mcp:8000/mcp`, Streamable HTTP | get_current_user, get_body_measurement, get_user_settings, get_day_plan_items, get_diet_summary, search_food, search_food_by_barcodes, get_recipe, search_recipes (9) |
| `mqtt__` | `http://mqtt-mcp-connector:8092/mcp`, Streamable HTTP; wartości odczytane z aktualnego Compose | list_entities, get_entity_state (2) |
| `hevy__` | prywatny `hevy-mcp-adapter:8080/mcp` → `python3 /home/admin/hevy-coach/server.py`, stdio | coach_status, coach_plan, coach_knowledge, coach_latest_report (4) |

To świadomie zakres **odczytu**. Nie obejmuje zapisów Obsidian/Fitatu/Hevy, workerów Codex, komend Tuya, publikacji MQTT, refresh_states, powiadomień ani analiz AI. `coach_plan` jest deterministycznym planowaniem na podstawie odczytu API Hevy; nie uruchamia Codex ani raportu AI. W testach wywołano tylko `coach_status` z narzędzi Hevy.

Adapter korzysta z aktywnego wolumenu danych, montowanego wyłącznie do odczytu. Ma dostęp do kodu/stanu coacha, vaulta oraz istniejącego klucza API Hevy. Dostaje minimalny plik konfiguracji zawierający tylko klienta Hevy. Nie montuje auth.json, prywatnej konfiguracji Codex, socketu Docker ani potoku Windows. Obraz bazowy Hevy zawiera też wcześniejsze programy, ale adapter uruchamia wyłącznie serwer transportowy i dozwolone odczyty stdio; nie uruchamia schedulera.

Publiczny transport to bezstanowy Streamable HTTP. Każda operacja tworzy osobną sesję backendu, bez współdzielonych połączeń MCP. Identyfikatory sesji dostarczone przez klienta są usuwane. Brama agreguje wyłącznie narzędzia; nie przekazuje resources, prompts, sampling ani elicitation. Nie gwarantuje kontynuacji sesji SSE, trwałych subskrypcji ani wznawiania strumienia po zerwaniu połączenia. Schematy wejściowe i wyniki pochodzą z backendów; brama pomija ich outputSchema ze względu na starsze, niespójne deklaracje formatów wyników.

Lista narzędzi pobierana jest równolegle z limitem 25 s na backend. Niedostępny backend jest pomijany, inne pozostają dostępne. Wywołanie niedostępnego narzędzia zwraca `isError`, nie dane zastępcze. Limit całej operacji wynosi 45 s; długi odczyt Hevy może wymagać ponowienia. Powrót backendu jest widoczny przy kolejnym tools/list; ChatGPT może wymagać odświeżenia listy narzędzi.

## OAuth i prywatność

- Osobny issuer: `https://wikingv.servehalflife.com/gateway`; resource/audience: `https://wikingv.servehalflife.com/mcp`.
- Protected Resource Metadata: `/.well-known/oauth-protected-resource/mcp`; AS metadata: `/.well-known/oauth-authorization-server/gateway`. Wyzwanie 401 wskazuje również działający alias `/gateway/.well-known/oauth-protected-resource`.
- Authorization Code + PKCE S256, DCR i obsługa dozwolonych dokumentów klienta ChatGPT. Formularz hasła ma ochronę CSRF. Redirect URI są ograniczone do callbacków ChatGPT i lokalnych callbacków OAuth.
- JWT: oddzielny losowy klucz podpisu, kontrola podpisu, issuer, audience, czasu i `home:read`. Access token ważny 1 godzinę, refresh 30 dni od pierwotnego udzielenia dostępu. Refresh tokeny są rotowane i jednorazowe; ich klucze w SQLite są haszowane. Klienci, kody i refresh tokeny są trwałe.
- Stan w osobnym wolumenie `home-server-gateway_gateway-oauth`, baza `/data/oauth.sqlite3`, prawa 0600. Nie używaj `down -v`. Utrata wolumenu lub klucza JWT wymaga ponownego logowania. Uruchamiaj jeden proces bramy, bez wielu workerów współdzielących tę bazę.
- Token klienta, cookies i inne nagłówki klienta nie trafiają do backendów. Adapter ma własny losowy klucz używany tylko wewnętrznie. Bezpośrednie usługi nadal są prywatne, a stare publiczne trasy mają własny OAuth.
- Brak logów dostępu bramy; komunikaty błędów nie drukują odpowiedzi, tokenów ani argumentów narzędzi. Pliki `target/`, `source/`, archiwa i `*.private.*` są wykluczone z repozytorium. Nie drukuj pełnego `docker inspect`, `compose config` ani konfiguracji Caddy.

Wybrano oficjalny Python SDK **mcp==1.30.0**, linię 1.x używaną już przez Obsidian. [Dokumentacja linii 1.x](https://github.com/modelcontextprotocol/python-sdk/tree/v1.x) potwierdza transporty stdio/SSE/Streamable HTTP i dalsze wsparcie istniejących wdrożeń. FastMCP nie jest dodatkową zależnością bramy. [Dokumentacja FastMCP](https://gofastmcp.com/servers/providers/proxy) opisuje m.in. domyślne przekazywanie nagłówków w proxy; własna mała warstwa klienta pozwala tu jawnie nie przekazywać żadnych nagłówków klienta. Wymagania audience i zakaz token passthrough: [specyfikacja autoryzacji MCP](https://modelcontextprotocol.io/specification/latest/basic/authorization). Zależności bezpośrednie są przypięte w `gateway/requirements.txt`; tagi bazowych obrazów i zależności przechodnie nie stanowią pełnego lockfile.

## Obsługa i odtworzenie wdrożenia

```powershell
Set-Location C:\Users\User\codex-migration-work
docker compose -f target/gateway-compose.private.json ps -a
docker compose -f target/gateway-compose.private.json up -d
```

Przy zmianie kodu lub odtworzeniu wygenerowanych plików:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\Prepare-Gateway.ps1 -Deploy
docker run --rm --user 0 --mount 'type=bind,src=C:\Users\User\codex-migration-work,dst=/workspace' --entrypoint python home-server/mcp-gateway:1.0.0 /workspace/gateway/route_caddy.py
docker exec home-server-migration-caddy-1 caddy validate --config /etc/caddy/caddy.json
docker exec home-server-migration-caddy-1 caddy reload --config /etc/caddy/caddy.json
```

Generator bramy odczytuje aktywny prywatny Compose, zachowuje istniejące nowe klucze z `gateway-compose.private.json`, generuje osobne pliki i nigdy nie uruchamia głównego generatora migracji. `gateway/auth.py` jest generowane z istniejącego kodu OAuth; nie zawiera sekretów. Regeneracja głównego Compose nadal wymaga `Publish` i `Persistent`. Regeneracja Caddy wcześniejszym `Prepare-Caddy.py` usuwa dodatkowe trasy bramy; po niej trzeba ponownie zastosować `gateway/route_caddy.py`. Skrypt tras bazuje na zapisanej konfiguracji sprzed bramy, więc nie używać go po innych, późniejszych zmianach Caddy bez aktualizacji kopii bazowej.

Prywatne pliki: `target/gateway-compose.private.json`, `target/gateway-backends.json`, `target/hevy-adapter-backends.json`, `target/caddy-before-gateway.private.json`. Do pełnego odtworzenia zachowaj prywatny Compose z kluczem JWT oraz spójną kopię wolumenu OAuth. Bazy starych proxy i dane usług nie są częścią nowego wolumenu.

Powrót: `powershell -NoProfile -ExecutionPolicy Bypass -File .\Rollback-Gateway.ps1`. Skrypt przywraca Caddy sprzed bramy, waliduje i przeładowuje go, następnie zatrzymuje tylko dwa nowe kontenery. Nie usuwa żadnych wolumenów ani starych proxy. Konfiguracja z bramą jest zachowana w `target/caddy-with-gateway.private.json`. Nie wykonano przełączenia produkcyjnego wstecz; kopia i mechanizm walidacji są przygotowane. Przywrócenie wymaga uwzględnienia wszelkich późniejszych zmian Caddy.

## Wyniki testów

Testy `gateway/test_gateway.py`, fazy initial, refresh, outage i ponownie refresh po przebudowie:

- Start i zdrowie bramy, HTTPS z weryfikacją certyfikatu, discovery resource i issuer; 401 na nowym oraz trzech starych endpointach bez tokena.
- Pełny przepływ DCR → formularz hasła → kod → token, CSRF, state, PKCE S256; odrzucenie błędnego PKCE, resource, zakresu i jednokrotnego ponownego użycia kodu.
- JWT: odrzucenie niepoprawnego audience, zakresu i wygasłego tokena.
- Dokładnie 22 unikalne narzędzia, odczyty `obsidian__list_dir`, `fitatu__get_current_user`, `mqtt__list_entities`, `hevy__coach_status`. Wyniki osobiste nie zostały wydrukowane.
- Bezwarunkowa odmowa wywołań poza allowlistą, w tym MQTT publish/set, zadania Codex, zapis Fitatu i aktualizacja coacha.
- Równoległe niezależne połączenia MCP działają. Po restarcie/przebudowie zachowane klient DCR i refresh token; rotacja odrzuca ponowne użycie starego refresh tokena.
- Zatrzymano wyłącznie nowy adapter Hevy: lista zachowała 18 narzędzi pozostałych usług, ich odczyty działały, Hevy zwróciło kontrolowany błąd. Adapter przywrócono, ponownie potwierdzono 22 narzędzia i odczyty.
- `Test-Services.py`: stare endpointy bezpośrednio i z autoryzacją przez HTTPS nadal listują 17/19/5 narzędzi. `hevy-check.py`: revision=1, 35 rekordów deduplikacji, odczyt health API i coach_status OK. Nie wysłano powiadomień, nie uruchomiono AI ani workerów.
- `ob sync-status --path /home/admin/obsidian-vault`: kod 0. Kontenery schedulera Hevy i Obsidian Sync nie były restartowane w tym zadaniu. Harmonogram Hevy pozostaje codziennie 18:00, sobota 09:00 Europe/Warsaw.
- Dwa nowe kontenery bez port bindings, system plików read-only, bez dodatkowych capabilities, no-new-privileges. Brama około 58 MiB, adapter około 40 MiB w bezczynności po testach. vmmem około 3,3 GiB; ustawień pamięci i WSL nie zmieniano.

Pełne powtórzenie: `powershell -NoProfile -ExecutionPolicy Bypass -File .\Test-Gateway.ps1`. Skrypt restartuje wyłącznie bramę, chwilowo zatrzymuje nowy adapter, przywraca go w `finally`, sprawdza powrót wszystkich backendów i usuwa własnego klienta oraz refresh token z bazy. Raport bez sekretów: `target/gateway-tests.log`. Uruchomienie pojedynczej fazy initial tworzy lokalnego klienta OAuth i prywatny plik `target/gateway-test.private.json`, potrzebny do kolejnych faz; samodzielne fazy wymagają potem cleanup. Nie drukować tego pliku. Testy nie potwierdzają jeszcze działania interfejsu ChatGPT ani dostępu spoza LAN.
