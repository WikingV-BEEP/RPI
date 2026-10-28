---
tags: [energetyka,typ_b,moc_bierna]
---
## Powrót po odłączeniu ≠ odbudowa po FRT
To dwa różne procesy:
- **Po FRT** moduł mógł pozostać przyłączony i odbudowuje P po ustąpieniu zwarcia (zob. [[06 - Typ B - FRT i prąd zwarciowy]]).
- **Po odłączeniu od sieci** ponowne przyłączenie wymaga spełnienia warunków resynchronizacji.

## Resynchronizacja typu B — źródło s. 18 i 37 PDF
1. Częstotliwość sieci w przedziale **49–50,05 Hz**.
2. Napięcie w punkcie przyłączenia w zakresie dopuszczonym właściwymi regulacjami.
3. Zwłoka **co najmniej 60 s** po powrocie parametrów do dozwolonych zakresów.

W odróżnieniu od opisu typu A, w tych tabelach B nie ma analogicznie wypisanej reguły narastania **10% Pmax/min**. Nie przenoś jej automatycznie bez osobnej podstawy.

## Regulacja mocy biernej typu B
PTPiREE przedstawia graficznie wymagania **U–Q/Pmax** (możliwość regulacji Q przy określonym U) i **P–Q/Pmax** (zdolność do Q przy zmianie P).
- Punkt przyłączenia, nie dowolny punkt pomiarowy w szafie, jest istotny.
- Przy **P < 0,1 Pmax** moduł powinien udostępniać dostępną moc bierną zgodnie z możliwościami technicznymi.
- PPM powinien mieć techniczną zdolność kompensowania Q w punkcie przyłączenia **także bez generacji P**.
- Jeśli wymaga się pracy przy **U < 0,9 p.u.**, udostępnia się dostępną Q na miarę możliwości technicznych.

> [!warning]
> Odpowiednie rysunki w oryginale są niezbędne do odczytu **granic obszaru pracy**. Te notatki wyjaśniają sens, ale nie rekonstruują wszystkich punktów wykresów z tekstowego PDF.

## Co sprawdzić w projekcie?
- Czy pomiary P/Q/U są odniesione do **PCC**, a nie wyłącznie zacisków falownika?
- Czy urządzenie może produkować Q przy P≈0?
- Czy znaki i kierunki Q producenta są zgodne z konwencją projektu?
- Czy dostępne Imax i S pozwalają na jednoczesne P i Q?
- Czy regulator Q jest skoordynowany z nadrzędnym sterowaniem farmy i operatorem?

Źródło: PDF s. 18–20 i 37–39.