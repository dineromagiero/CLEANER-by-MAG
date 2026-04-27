# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.10-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do kompleksowej optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od skanowania antywirusowego i usuwania śmieciowych plików, przez naprawę plików systemowych, aż po konfigurację sieci i rejestru. Skrypt **inteligentnie dostosowuje się do sprzętu** — inne decyzje podejmuje na SSD, inne na HDD, inne na laptopie, inne na PC stacjonarnym. Od wersji 1.10 czyści cache przeglądarek dla **wszystkich kont użytkowników** na komputerze.

---

## ✨ Co robi skrypt?

### 🔒 Bezpieczeństwo i antywirusy
- Tworzy **punkt przywracania systemu** przed wprowadzeniem jakichkolwiek zmian (z weryfikacją powodzenia)
- Aktualizuje i uruchamia **szybki skan Windows Defender** z raportowaniem kodu błędu
- Pobiera, uruchamia i automatycznie usuwa **AdwCleaner** (skan + czyszczenie adware); pobieranie z limitem 30 sekund — brak internetu oznacza `[ SKIP ]`
- Opcjonalne pobranie, skan i deinstalacja **Malwarebytes** po zakończeniu; brak połączenia sygnalizowany komunikatem

### 🗑️ Czyszczenie plików
- Pliki tymczasowe użytkownika i systemu (`%TEMP%`, `C:\Windows\Temp`)
- Folder **Downloaded Program Files**
- **Folder Prefetch** — czyszczony tylko na HDD; na SSD zarządzany automatycznie przez system (`[ SKIP ]`)
- **Minidumpy** i raporty błędów Windows (WER)
- Cache **ikon** i czcionek
- Cache przeglądarek — **dla każdego konta użytkownika na komputerze** (pętla po `C:\Users\*`, z pominięciem kont systemowych): **Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox, LibreWolf, Zen Browser** (domyślny profil + wszystkie `Profile *`)
- Cache **Microsoft Teams** — obsługa obu wersji: klasycznej (`%APPDATA%\Microsoft\Teams`) i nowej UWP (`MSTeams_*\LocalCache`)
- Cache **Microsoft Store** (LocalCache, INetCache, Temp dla wszystkich pakietów AppX)
- Kolejka **drukarki (Spooler)** + stare sterowniki drukarek (W32X86, x64, ia64; aktywne zachowane)
- Logi **CBS** i Windows Update (`SoftwareDistribution`, `catroot2`)
- Cache miniatur z restartem Eksploratora
- Folder **Windows.old** — podawany jest rozmiar przed usunięciem; `[ SKIP ]` jeśli nie istnieje
- **Oczyszczanie dysku** (`cleanmgr`) z konfiguracją 34 kategorii przez rejestr; `[ SKIP ]` jeśli `cleanmgr` niedostępny

### 🔧 Naprawa systemu
- **SFC** (System File Checker) — dwukrotnie: przed i po DISM, z paskiem postępu i kodem błędu w raporcie
- **DISM RestoreHealth** — naprawa obrazu systemu, z paskiem postępu i kodem błędu w raporcie
- **DISM StartComponentCleanup** — czyszczenie bazy składników po aktualizacjach
- **CHKDSK** zaplanowany na następny restart
- **Inteligentna naprawa WMI** — najpierw weryfikacja (`verifyrepository`); jeśli baza jest zdrowa → `[ SKIP ]`; jeśli nie → próba naprawy Salvage; jeśli dalej błąd → pełny Reset z rejestracją DLL i plikami MOF
- Pełny reset **Windows Update** (zatrzymanie usług, usunięcie SoftwareDistribution i catroot2, rejestracja DLL)

### ⚡ Optymalizacja wydajności
- Automatyczne wykrywanie urządzenia: **laptop vs PC stacjonarny**
- Ustawienie **planu zasilania Wysoka Wydajność** (tylko dla PC stacjonarnych)
- Wykrywanie dysku przez `Get-PhysicalDisk` (MediaType: SSD / NVMe / HDD) z fallbackiem na nazwę modelu
- Wyłączenie **hibernacji** — tylko na SSD/NVMe; na HDD pozostaje bez zmian
- Wyłączenie **Fast Startup**
- **SysMain (Superfetch)** — wyłączany tylko na SSD; na HDD pozostaje aktywny
- **DisablePagingExecutive** — włączany tylko przy ≥ 8 GB RAM
- Optymalizacja **pliku stronicowania** (PageFile) przez `CimInstance` — automatycznie dopasowana do ilości RAM
- Tweaki rejestru: priorytety **procesora**, zarządzanie **pamięcią**, animacje UI, aktywne godziny Windows Update (8:00–23:00)
- **Defragmentacja** (HDD) lub **TRIM** (SSD) przez `defrag /O`
- Wyłączenie wybranych programów **startowych**: Skype, OneDrive, Spotify, Discord, Steam, Epic Games, Teams, GOG Galaxy, EA App, Ubisoft Connect, Overwolf, Zoom, Viber

### 🌐 Sieć
- **Reset proxy** systemowego (WinHTTP i rejestr IE)
- Flush i rejestracja **DNS**
- Reset **stosu TCP/IP** i Winsock
- Ustawienie serwerów DNS: **Cloudflare (1.1.1.1 / 1.0.0.1)** i **Google (8.8.8.8 / 8.8.4.4)** dla IPv4 i IPv6
- Optymalizacja parametrów **TCP** (autotuninglevel, RSS, chimney)

### 🛡️ Prywatność i telemetria
- Wyłączenie zbierania danych diagnostycznych (**telemetria**)
- Wyłączenie **Cortany**
- Wyłączenie **Bing Search** w menu Start
- Wyłączenie śledzenia wpisów i kolekcji danych wejściowych (InputPersonalization)
- Wyłączenie sugestii i powiadomień systemowych (ContentDeliveryManager)
- Wyłączenie raportowania błędów Windows (WER)

### ⚙️ Usługi i harmonogramy
- Dezaktywacja zbędnych usług: `DiagTrack`, `WSearch`, `MapsBroker`, `Fax`, `RetailDemo`; `SysMain` — tylko na SSD
- Wyłączenie zbędnych harmonogramów systemowych (CEIP, feedback, Xbox, dysk, WER)
- Blokada procesów **przeglądarek w tle** i ich autoaktualizacji (Chrome, Edge, Brave, Opera — dynamiczne wyszukiwanie usługi, Firefox, Adobe)

### 🖥️ Interfejs i UX
- Automatyczne **odblokowanie skryptu** (`Unblock-File`) po pobraniu z internetu
- Kolorowy, czytelny interfejs konsolowy (ANSI: aqua / biały / żółty)
- Nagłówek z linkiem do GitHub widoczny od pierwszego uruchomienia
- Dynamiczne dopasowanie rozmiaru okna i **bufor konsoli (240 linii)**
- **Sprawdzanie aktualizacji** przez GitHub API — inteligentne porównanie wersji (obsługa prefiksu `v` i suffiksu `.0` w tagach)
- Trójstanowy system statusów: `[ OK ]` / `[SKIP]` / `[----]`
- SFC, DISM i inne długie operacje wyświetlają czytelny **pasek postępu**
- **Czas trwania** optymalizacji mierzony i wyświetlany w formacie `HH:MM:SS`
- Dźwiękowe **powiadomienie o zakończeniu** optymalizacji
- Interaktywne pytania na koniec: Malwarebytes, wsparcie projektu, restart
- Raport zapisywany na **Pulpicie** użytkownika: `CLEANER by MAG - WYNIKI.txt`

---

## 🚀 Wymagania

| Wymaganie | Szczegóły |
|-----------|-----------|
| System | Windows 10 / Windows 11 |
| Uprawnienia | **Administrator** (wymagane) |
| Połączenie | Opcjonalne — potrzebne dla sprawdzania wersji, AdwCleaner, Malwarebytes |
| PowerShell | 5.x (wbudowany w Windows) |

---

## 📥 Instalacja i uruchomienie

1. Pobierz najnowszą wersję ze strony [Releases](https://github.com/dineromagiero/CLEANER-by-MAG/releases/latest)
2. Kliknij **prawym przyciskiem myszy** na plik `.bat`
3. Wybierz **„Uruchom jako administrator"**
4. Postępuj zgodnie z instrukcjami na ekranie

> ⚠️ **Uwaga:** Skrypt **musi** być uruchomiony jako administrator. Bez uprawnień administratora wyświetli czytelny komunikat z instrukcją i zamknie się po 10 sekundach. Skrypt automatycznie odblokuje się, jeśli Windows zablokował go po pobraniu z internetu.

---

## 📊 Raport wynikowy

Po zakończeniu działania skrypt automatycznie zapisuje plik tekstowy na **Pulpicie**:

```
%USERPROFILE%\Desktop\CLEANER by MAG - WYNIKI.txt
```

Raport zawiera:
- datę i czas wykonania (start, koniec, **łączny czas trwania w formacie HH:MM:SS**)
- ilość zwolnionego miejsca na dysku C:
- parametry systemu (RAM, pagefile, model dysku, rozmiar kolejki drukarki, rozmiar CBS.log)
- listę wszystkich wykonanych operacji z trójstanowym statusem: `[ OK ]`, `[ SKIP ]`, `[----]`
- nazwę komputera i użytkownika

---

## 🗂️ Wykonywane operacje — podsumowanie

| Operacja | Opis |
|----------|------|
| Punkt przywracania systemu | Tworzy przed jakimikolwiek zmianami (z weryfikacją) |
| Windows Defender | Aktualizacja sygnatur + szybki skan (z kodem błędu) |
| AdwCleaner | Skan i czyszczenie adware; `SKIP` jeśli brak internetu (timeout 30s) |
| Reset proxy | Usuwa proxy systemowe i IE/WinHTTP |
| Pliki tymczasowe | `%TEMP%` i `C:\Windows\Temp` |
| Prefetch | Czyszczony na HDD; `SKIP` na SSD (zarządzany automatycznie) |
| DNS / Sieć | Flush, reset TCP/IP, Cloudflare + Google DNS (IPv4 + IPv6) |
| Dzienniki zdarzeń | Czyści cały Event Viewer |
| Usługi systemowe | SysMain (tylko SSD), DiagTrack, WSearch, MapsBroker, Fax, RetailDemo |
| Optymalizacja rejestru | Tweaki wydajności, prywatności, UI i Windows Update |
| Cache przeglądarek | Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox, LibreWolf, Zen Browser — **dla każdego konta użytkownika** |
| Cache Microsoft Teams | Klasyczny + nowy UWP (MSTeams_*) |
| Kolejka drukarki | Czyści spooler + usuwa stare sterowniki (aktywne zachowane) |
| Microsoft Store cache | Czyści cache paczek AppX |
| Oczyszczanie dysku | cleanmgr z 34 kategoriami; `SKIP` jeśli niedostępny w systemie |
| SFC (wstępny) | Sprawdza integralność plików — pasek postępu + kod błędu |
| DISM RestoreHealth | Naprawia obraz systemu — pasek postępu + kod błędu |
| SFC (końcowy) | Ponowna weryfikacja po DISM — pasek postępu + kod błędu |
| DISM StartComponentCleanup | Czyści stare składniki aktualizacji |
| Logi CBS | Usuwa duże logi CBS |
| Windows Update cache | Pełny reset usług i SoftwareDistribution |
| Usunięcie Windows.old | Odzyskuje miejsce z podaniem rozmiaru; `SKIP` jeśli nie istnieje |
| Defragmentacja / TRIM | `defrag /O` — zależnie od wykrytego typu dysku |
| Thumbnail Cache | Czyści cache miniatur, restartuje Eksplorator |
| Naprawa WMI | Weryfikacja → Salvage → Reset; `SKIP` jeśli baza jest zdrowa |
| Pliki Minidump | Usuwa zrzuty awaryjne |
| Raporty WER | Czyści raporty błędów Windows |

---

## ⚠️ Ważne uwagi

- Skrypt **planuje CHKDSK** na następny restart — wymagane ponowne uruchomienie, aby wszystkie zmiany weszły w życie
- Wyłączona jest **hibernacja** — ale tylko na SSD/NVMe; na HDD hibernacja pozostaje aktywna
- Usługa **Windows Search (WSearch)** zostaje wyłączona — może wpłynąć na wyszukiwanie w menu Start
- Usługa **SysMain (Superfetch)** wyłączana jest tylko na SSD; na HDD pozostaje aktywna
- Folder **Prefetch** jest czyszczony tylko na HDD — na SSD Windows zarządza nim automatycznie
- Skrypt usuwa wybrane **wbudowane aplikacje** Windows: Solitaire, Bing, Zune, People, Skype, Office Hub, 3D Builder, Get Started
- Wyłączane są wpisy autostartu popularnych aplikacji (Spotify, Discord, Steam, Epic, Teams itd.) — można je ponownie włączyć ręcznie
- Folder **Windows.old** jest usuwany trwale — po tej operacji nie ma możliwości cofnięcia do poprzedniej wersji Windows przez ustawienia systemowe

---

## 🔄 Co nowego w v1.10?

- ✅ **Cache przeglądarek dla wszystkich kont użytkowników** — pętla po `C:\Users\*` czyści cache każdego profilu na komputerze; każda operacja w konsoli pokazuje nazwę konta `[NazwaUżytkownika]`
- ✅ **Nowe przeglądarki: LibreWolf i Zen Browser** — dołączyły do listy obsługiwanych przeglądarek
- ✅ **Teams nowy UWP** — osobna obsługa nowego Microsoft Teams z pakietu `MSTeams_*` (EBWebView) obok wersji klasycznej
- ✅ **Inteligentna naprawa WMI** — dwuetapowa: najpierw Salvage, dopiero jeśli to nie wystarczy — pełny Reset; jeśli baza jest zdrowa od razu, operacja oznaczana jako `[ SKIP ]`
- ✅ **Raport na Pulpicie** — plik wynikowy trafia do `%USERPROFILE%\Desktop\` zamiast `C:\`
- ✅ **Czas trwania optymalizacji** — mierzony od startu do końca za pomocą `(Get-Date).Ticks`, wyświetlany w formacie `HH:MM:SS` w konsoli i raporcie
- ✅ **Bufor konsoli zwiększony do 240 linii** — więcej historii operacji bez przycinania
- ✅ **PageFile przez `CimInstance`** zamiast przestarzałego `wmic` — nowocześniejsze i pewniejsze API
- ✅ **Inteligentne porównanie wersji z GitHub** — obsługa tagów z prefiksem `v` (np. `v1.10`) i sufiksem `.0` (np. `1.10.0`) bez fałszywych alertów o aktualizacji
- ✅ **`endlocal` przed wyjściem** — poprawka wycieku zmiennych środowiskowych przy zamknięciu skryptu

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

**Autor:** Mag | **Wersja:** 1.10 (27/04/2026)
