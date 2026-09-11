# MQTT — połączenie zdalne

## Adres brokera

- Zdalnie, MQTT przez WebSocket z TLS: `wss://wikingv.servehalflife.com:443/mqtt`
- W sieci domowej, MQTT TCP: `192.168.1.37:1883`
- Broker: Mosquitto, kontener `automation-mosquitto` na Raspberry Pi 3B+.

## Ustawienia w formularzu MQTT Connection

| Pole | Wartość |
| --- | --- |
| Name | Dom MQTT (dowolna nazwa) |
| Protocol | `ws://` + włączone Encryption (tls); wynikowy protokół `wss://` |
| Host | `wikingv.servehalflife.com` |
| Port | `443` |
| Basepath | `mqtt` |
| Encryption (tls) | Włączone |
| Validate certificate | Włączone |
| Username | Login do brokera MQTT |
| Password | Hasło do brokera MQTT |

Host wpisujemy bez `/mqtt` i bez protokołu. W tym formularzu Basepath to `mqtt`, a port to `443` (nie `43`). Podgląd adresu powinien pokazać `wss://wikingv.servehalflife.com:443/mqtt`. Następnie klikamy **SAVE**.

Dane logowania są wymagane; w notatce nie zapisano hasła.

## Potwierdzony stan — 2026-09-07

- Caddy przekazuje ścieżkę `/mqtt` do `automation-mosquitto:9001` (WebSocket).
- Test z komputera przez publiczną domenę ustanowił połączenie WebSocket z TLS.
- Broker zwrócił MQTT CONNACK `20-02-00-05` dla próby bez danych logowania (brak autoryzacji). Potwierdza to odpowiedź brokera, ale nie poprawne logowanie użytkownika.
- Konfiguracja Mosquitto ma `allow_anonymous false`.
- Nie przeprowadzono testu z niezależnej sieci, np. LTE, ani testu publicznego dostępu do portu TCP `1883`.

## Powiązane

- [[MQTT i Tuya]]
- [[Sieć i dostęp]]
