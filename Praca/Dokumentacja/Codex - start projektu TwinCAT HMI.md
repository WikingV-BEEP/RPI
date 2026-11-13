# Codex — start projektu TwinCAT HMI

## 1. Wybierz foldery w Codex

Utwórz projekt lokalny. Jako **Make primary** ustaw katalog nowego projektu, czyli miejsce na kod, rozwiązanie i wyniki walidacji. Jeśli masz już rozwiązanie, wybierz katalog obejmujący `.sln` i wszystkie projekty, nad którymi będziesz pracować.

| Folder                                                                                        | Kiedy podpiąć                                                                                                                                 |
| --------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Katalog nowego projektu, np. `C:/Users/WiktorD/Desktop/Beckhoff/Codex Projects/MojProjektHMI` | Główny folder. Tutaj Codex zapisuje kod i wyniki.                                                                                             |
| `C:/Users/WiktorD/Desktop/Beckhoff/Codex Agents/HMI Framework Developer`                      | Jako dodatkowy folder, jeśli sesja nie widzi współdzielonych plików pod tą ścieżką. Traktuj go jako źródło instrukcji i narzędzi.             |
| `C:/Users/WiktorD/Desktop/Codex/Empty template project`                                       | Referencja przy tworzeniu projektu od zera lub sprawdzaniu lokalnych deklaracji i schematów. Dokładne ścieżki są w `environment.json` agenta. |
| `C:/ProgramData/Beckhoff/NuGetPackages`                                                       | Lokalny feed, gdy trzeba odczytać paczki lub użyć go do restore.                                                                              |
| Repozytorium biblioteki, np. `openbridge-webcomponents`                                       | Tylko gdy jest źródłem kodu lub zasobów dla danego zadania.                                                                                   |

Dodatkowe foldery są potrzebne tylko wtedy, gdy zadanie z nich korzysta i sesja nie ma już do nich dostępu. Ustawienie folderu jako dodatkowego samo nie nadaje mu trybu tylko do odczytu. W instrukcjach projektu określ, gdzie wolno zapisywać zmiany.

## 2. Podłącz agenta na stałe

W głównym folderze projektu utwórz `AGENTS.md` lub dopisz poniższy blok do istniejącego pliku.

```markdown
## TwinCAT HMI Framework Developer

Przed pracą z TwinCAT HMI przeczytaj i stosuj instrukcje:
[HMI Framework Developer AGENTS.md](<C:/Users/WiktorD/Desktop/Beckhoff/Codex Agents/HMI Framework Developer/AGENTS.md>).

Zasoby i skrypty agenta rozpoznawaj względem jego współdzielonego katalogu.
Kod, artefakty i raporty zapisuj w docelowym projekcie.
Współdzielony katalog agenta, pusty szablon, feed NuGet i dodatkowe repozytoria referencyjne traktuj jako źródła do odczytu, chyba że wyraźnie wskażę inaczej.
```

Aby Codex automatycznie wykrywał skille, udostępnij je pod `.agents/skills` projektu. Poniższy PowerShell uruchom **w głównym folderze projektu**. Polecenie zachowuje istniejącą zawartość `.agents/skills` i pomija nazwy, które już istnieją.

```powershell
$hmiSkillSource = 'C:/Users/WiktorD/Desktop/Beckhoff/Codex Agents/HMI Framework Developer/.agents/skills'
$hmiSkillTarget = Join-Path (Get-Location).Path '.agents/skills'
$hmiSkillRoot = Get-Item -LiteralPath $hmiSkillTarget -ErrorAction SilentlyContinue
if (-not $hmiSkillRoot) {
    New-Item -ItemType Directory -Path $hmiSkillTarget -Force | Out-Null
}
if ($hmiSkillRoot.LinkType) {
    throw 'Folder .agents/skills jest już odsyłaczem. Sprawdź jego cel.'
}
Get-ChildItem -LiteralPath $hmiSkillSource -Directory -Filter 'tchmi-*' | ForEach-Object {
    $hmiLink = Join-Path $hmiSkillTarget $_.Name
    if (-not (Get-Item -LiteralPath $hmiLink -ErrorAction SilentlyContinue)) {
        New-Item -ItemType Junction -Path $hmiLink -Target $_.FullName | Out-Null
    }
}
```

Jeśli projekt już używa układu takiego jak `HMI Dev` i cała jego `.agents` wskazuje na współdzielony katalog agenta, pomiń polecenie. W projekcie przenoszonym na inny komputer odtwórz odsyłacze lokalnie albo skopiuj foldery skilli. Zasoby agenta można czytać bezpośrednio z jego katalogu dzięki odsyłaczowi w `AGENTS.md`.

## 3. Rozpocznij pierwszy czat

Otwórz nowy czat **Codex → Local** w projekcie i skopiuj [[_Templates/Codex - prompt startowy TwinCAT HMI|szablon promptu startowego]]. Wypełnij cel, ścieżkę i format dostawy. W kolejnych czatach w tym projekcie zwykle wystarczy podać nowe zadanie i oczekiwany wynik.

Codex automatycznie szuka instrukcji i skilli w głównym folderze projektu. Gdy skill się nie pojawi, sprawdź ustawienie **Make primary**, ścieżkę `.agents/skills` i otwórz nową sesję. [Projekty w Codex](https://learn.chatgpt.com/docs/projects) · [Instrukcje AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md) · [Wykrywanie skilli](https://learn.chatgpt.com/docs/build-skills)
