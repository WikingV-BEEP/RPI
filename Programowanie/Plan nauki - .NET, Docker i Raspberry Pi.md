# Plan nauki — .NET + ASP.NET Core + Raspberry Pi + Docker

## Cel

Zbudować aplikację działającą 24/7 na Raspberry Pi, która:

- działa bez otwartego klienta,
- wykonuje logikę w tle,
- komunikuje się z urządzeniami / PLC / czujnikami,
- udostępnia REST API,
- wysyła dane live do frontendu,
- zapisuje dane do bazy,
- działa w kontenerach Docker.

Docelowy stack:

```text
Raspberry Pi
│
├── Docker Compose
│
├── ASP.NET Core
│   ├── REST API
│   ├── BackgroundService
│   ├── SignalR
│   └── logika aplikacyjna
│
├── PostgreSQL / SQLite
│
├── Frontend
│   └── React + Vite
│
└── Nginx
```

## 1. Linux / Raspberry Pi

### Nauczyć się

- SSH
- struktura katalogów Linux
- procesy
- porty
- uprawnienia
- usługi systemowe
- logi

### Komendy

```bash
ssh
cd
ls
cp
mv
rm
chmod
ps
top
systemctl
journalctl
curl
ss
```

### Cel praktyczny

Umieć wejść przez SSH na RPi i sprawdzić:

- działające procesy,
- zajęte porty,
- logi,
- stan aplikacji.

## 2. Docker

### Nauczyć się

- image
- container
- Dockerfile
- port mapping
- volume
- environment variables
- Docker Compose

### Cel praktyczny

Uruchomić pierwszy kontener:

```bash
docker run hello-world
```

Następnie własną aplikację .NET w Dockerze.

## 3. Podstawy .NET / C#

### Nauczyć się

- projekt .NET
- `dotnet new`
- `dotnet build`
- `dotnet run`
- klasy
- interfejsy
- async / await
- dependency injection

### Cel praktyczny

Napisać aplikację konsolową odczytującą np.:

- temperaturę CPU,
- uptime,
- wykorzystanie RAM.

## 4. ASP.NET Core

### Nauczyć się

- WebApplication
- endpointy
- Minimal API
- HTTP
- JSON
- Dependency Injection
- konfiguracja aplikacji

### Pierwsze endpointy

```text
GET /api/status
GET /api/cpu
GET /api/memory
```

Przykład:

```csharp
app.MapGet("/api/status", () =>
{
    return new
    {
        Status = "OK",
        Time = DateTime.Now
    };
});
```

## 5. REST API

### Nauczyć się

Metody:

```text
GET
POST
PUT
DELETE
```

Kody HTTP:

```text
200 OK
201 Created
400 Bad Request
404 Not Found
500 Internal Server Error
```

### Cel praktyczny

API urządzenia:

```text
GET  /api/device
GET  /api/device/status
POST /api/device/start
POST /api/device/stop
```

## 6. BackgroundService

Kluczowy element aplikacji działającej bez otwartego klienta.

### Nauczyć się

- `BackgroundService`
- `IHostedService`
- CancellationToken
- pętle asynchroniczne
- obsługa wyjątków

Przykład:

```csharp
public class DeviceWorker : BackgroundService
{
    protected override async Task ExecuteAsync(
        CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            // odczyt danych
            // logika
            // zapis do bazy

            await Task.Delay(100, stoppingToken);
        }
    }
}
```

### Cel praktyczny

Proces działający 24/7:

```text
odczyt
↓
logika
↓
sterowanie
↓
zapis danych
```

bez otwartego browsera.

## 7. Soft / semi real-time

ASP.NET Core nie jest hard real-time.

Może jednak realizować logikę typu:

```text
1000 ms
500 ms
100 ms
50 ms
```

w zależności od wymagań.

Nie stosować do funkcji wymagających gwarantowanego deadline'u.

Hard RT pozostawić:

```text
PLC
MCU
RT Linux
TwinCAT
```

Architektura:

```text
PLC / MCU
     ↓
MQTT / TCP / ADS / OPC UA
     ↓
ASP.NET Core
     ↓
REST / SignalR
     ↓
Frontend
```

## 8. SignalR

SignalR odpowiada za dane live w przeglądarce.

### Nauczyć się

- WebSocket
- Hub
- połączenie klient-serwer
- push danych

Schemat:

```text
BackgroundService
      ↓
   SignalR
      ↓
Frontend
```

REST służy do pobrania aktualnego stanu.

SignalR służy do kolejnych zmian live.

## 9. Baza danych

Na początku:

```text
SQLite
```

Później:

```text
PostgreSQL
```

### Nauczyć się

- Entity Framework Core
- DbContext
- modele
- migracje
- zapytania LINQ

Przykładowa tabela:

```text
Measurement

Id
Timestamp
Temperature
Value
DeviceId
```

## 10. Frontend

Dopiero po działającym backendzie.

Stack:

```text
React
Vite
JavaScript / TypeScript
```

### Nauczyć się

- komponenty
- state
- fetch
- REST
- SignalR
- podstawowy routing

Pierwszy ekran:

```text
RPi Status

CPU: 52°C
RAM: 43%
Uptime: 4d 12h
Device: RUNNING
```

## 11. Docker Compose

Połączyć:

```text
backend
frontend
database
nginx
```

Przykładowo:

```text
docker-compose.yml
│
├── backend
├── frontend
├── postgres
└── nginx
```

Całość uruchamiana:

```bash
docker compose up -d
```

## 12. Nginx

Na końcu.

Routing:

```text
/
↓
Frontend

/api
↓
ASP.NET Core

/hub
↓
SignalR
```

# Projekt końcowy

## Raspberry Pi Control Panel

Funkcje:

- temperatura CPU,
- wykorzystanie RAM,
- wykorzystanie dysku,
- uptime,
- stan usług,
- historia pomiarów,
- dane live,
- sterowanie urządzeniami,
- komunikacja z PLC,
- logowanie zdarzeń.

Architektura:

```text
             Raspberry Pi

┌────────────────────────────────┐
│                                │
│ BackgroundService              │
│       ↓                        │
│ Device Services                │
│       ↓                        │
│ ASP.NET Core                   │
│   ├── REST API                 │
│   └── SignalR                  │
│       ↓                        │
│ PostgreSQL                     │
│                                │
└───────────────┬────────────────┘
                │
              HTTP
                │
                ▼
          React Frontend
```

# Kolejność nauki

```text
Linux
↓
Docker
↓
C# / .NET
↓
ASP.NET Core
↓
REST
↓
BackgroundService
↓
SignalR
↓
SQLite
↓
React
↓
Docker Compose
↓
PostgreSQL
↓
Nginx
```

## Zasada

Nie uczyć się całego stacku teoretycznie.

Każdy etap kończyć działającym fragmentem projektu.
