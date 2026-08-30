# Docker — Images & Containers

## Images

**Image** to szablon / blueprint, na podstawie którego Docker tworzy kontenery.

Najważniejsze cechy:
- jest **read-only**,
- zawiera aplikację i jej środowisko:
  - system / bazowy filesystem,
  - runtime,
  - biblioteki,
  - narzędzia,
- sam Image się nie uruchamia,
- uruchamiana jest jego instancja, czyli **Container**.

Image może być:
- gotowy, np. pobrany z Docker Hub,
- zbudowany samodzielnie za pomocą `Dockerfile`.

## Dockerfile

`Dockerfile` opisuje sposób zbudowania Image.

Budowanie obrazu:

```bash
docker build .
```

Każda instrukcja Dockerfile tworzy osobną **warstwę (layer)** obrazu. Warstwy pozwalają Dockerowi szybciej przebudowywać i efektywniej współdzielić obrazy.

### CMD

`CMD` nie wykonuje się podczas budowania Image, ale dopiero podczas uruchamiania kontenera.

```text
Dockerfile
↓
Image
↓
Container
↓
CMD
↓
uruchomiona aplikacja
```

# Containers

**Container** to uruchomiona instancja Image.

```bash
docker run IMAGE_NAME
```

Podczas tworzenia kontenera Docker dodaje na Image cienką warstwę **read-write**.

```text
Image
(read-only)
    ↓
Container layer
(read-write)
    ↓
Running Container
```

Na podstawie jednego Image można uruchomić wiele niezależnych kontenerów.

```text
          Image
         /  |  \
        /   |   \
 Container Container Container
```

Kontenery działają w izolacji i domyślnie nie współdzielą stanu aplikacji ani zapisanych danych.

# Image vs Container

```text
Image = program / szablon
Container = uruchomiona instancja tego szablonu
```

Analogicznie:

```text
Class → Object
Image → Container
```

# Najważniejsze komendy Docker

## Budowanie Image

```bash
docker build .
```

Nadanie nazwy i taga:

```bash
docker build -t NAME:TAG .
```

Przykład:

```bash
docker build -t myapp:1.0 .
```

## Uruchamianie kontenera

```bash
docker run IMAGE_NAME
```

Jeżeli Image nie istnieje lokalnie, Docker może automatycznie pobrać go z registry.

### Nazwa kontenera

```bash
docker run --name NAME IMAGE
```

### Detached mode

```bash
docker run -d IMAGE
```

Kontener działa w tle.

### Interactive mode

```bash
docker run -it IMAGE
```

Kontener / aplikacja jest przygotowana do odbierania wejścia z terminala.

### Automatyczne usuwanie

```bash
docker run --rm IMAGE
```

Po zatrzymaniu kontenera Docker automatycznie go usuwa.

# Wyświetlanie kontenerów

Uruchomione kontenery:

```bash
docker ps
```

Wszystkie kontenery, również zatrzymane:

```bash
docker ps -a
```

# Wyświetlanie Images

```bash
docker images
```

# Usuwanie

Kontener:

```bash
docker rm CONTAINER
```

Image:

```bash
docker rmi IMAGE
```

# Czyszczenie Dockera

Usunięcie wszystkich zatrzymanych kontenerów:

```bash
docker container prune
```

Usunięcie dangling Images:

```bash
docker image prune
```

Usunięcie wszystkich nieużywanych lokalnych Images:

```bash
docker image prune -a
```

# Docker Registry

Pobranie Image:

```bash
docker pull IMAGE
```

Wysłanie Image:

```bash
docker push IMAGE
```

# Podstawowy workflow

```text
Dockerfile
    ↓
docker build
    ↓
Image
    ↓
docker run
    ↓
Container
    ↓
Running application
```

Przykład:

```bash
docker build -t myapp:1.0 .
docker run --name myapp-container -d myapp:1.0
docker ps
docker rm myapp-container
docker rmi myapp:1.0
```

# Najważniejsze do zapamiętania

**Image**
- szablon,
- read-only,
- zawiera aplikację i jej środowisko.

**Container**
- instancja Image,
- posiada dodatkową warstwę read-write,
- faktycznie uruchamia aplikację.

**Dockerfile**
- przepis na zbudowanie Image.

```text
Dockerfile → Image → Container
```
