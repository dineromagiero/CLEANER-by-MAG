# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.12-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do kompleksowej optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od skanowania antywirusowego i usuwania śmieciowych plików, przez naprawę plików systemowych, aż po konfigurację sieci i rejestru. Skrypt **inteligentnie dostosowuje się do sprzętu i systemu** — inne decyzje podejmuje na SSD vs HDD, na laptopie vs PC, na Windows 10 vs Windows 11.

---

## ✨ Co robi skrypt?

### 🔒 Bezpieczeństwo i antywirusy
- Tworzy **punkt przywracania systemu** przed wprowadzeniem jakichkolwiek zmian (z weryfikacją powodzenia)
- Aktualizuje i uruchamia **szybki skan Windows Defender** z raportowaniem kodu błędu
- Pobiera, uruchamia i automatycznie usuwa **AdwCleaner** (skan + czyszczenie adware); pobieranie z limitem 30 sekund — brak internetu oznacza `[ SKIP ]`
- Opcjonalne pobranie, skan i deinstalacja **Malwarebytes** po zakończeniu; `mbam.exe` wyszukiwany dynamicznie przez PowerShell

### 🗑️ Czyszczenie plików
- Pliki tymczasowe użytkownika i systemu (`%TEMP%`, `C:\Windows\Temp`)
- Folder **Downloaded Program Files**
- **Folder Prefetch** — czyszczony tylko na HDD; na SSD zarządzany automatycznie przez system (`[ SKIP ]`)
- **Minidumpy** i raporty błędów Windows (WER)
- Cache **ikon** i czcionek
- **Logi instalatorów systemowych**: CBS, DISM, MeasuredBoot, NetSetup, waasmedic, INF (`*.log`, `*.pnf`)
- Cache przeglądarek — **dla każdego konta użytkownika na komputerze** (pętla po `C:\Users\*`): **Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox, LibreWolf, Zen Browser, Floorp, Thunderbird** (domyślny profil + wszystkie `Profile *`)
- Cache **Microsoft Teams** — obsługa obu wersji: klasycznej i nowej UWP (`MSTeams_*\LocalCache`)
- Cache **Microsoft OneDrive** — logi, setup/logs, `.deadLetterQueue`
- Cache **Java** (`AppData\LocalLow\Sun\Java\Deployment\cache`)
- Pliki tymczasowe **ASP.NET** (`Temporary ASP.NET Files`)
- Cache **Microsoft Store** (LocalCache, INetCache, Temp dla wszystkich pakietów AppX)
- Kolejka **drukarki (Spooler)** + stare sterowniki drukarek (W32X86, x64, ia64; aktywne zachowane)
- Logi **CBS** i Windows Update (`SoftwareDistribution`, `catroot2` — z fallbackiem na zmianę nazwy)
- Cache miniatur z restartem Eksploratora
- Folder **Windows.old** — `takeown` + `icacls` przed usunięciem; `[ SKIP ]` jeśli nie istnieje
- **Oczyszczanie dysku** (`cleanmgr`) z konfiguracją 34 kategorii przez rejestr, okno wysuwane na pierwszy plan podczas działania; `[ SKIP ]` jeśli `cleanmgr` niedostępny

### 🔧 Naprawa systemu
- **SFC** (System File Checker) — dwukrotnie: przed i po DISM, z paskiem postępu i kodem błędu w raporcie
- **DISM RestoreHealth** — naprawa obrazu systemu, z paskiem postępu i kodem błędu; ostrzeżenie o nieprzerywaniu
- **DISM StartComponentCleanup** — czyszczenie bazy składników po aktualizacjach
- **CHKDSK** — planowany na następny restart; `[ SKIP ]` jeśli już zaplanowany (brak duplikatów)
- **Inteligentna naprawa WMI** — weryfikacja → Salvage → Reset; `[ SKIP ]` jeśli baza jest zdrowa
- Pełny reset **Windows Update** (zatrzymanie usług, usunięcie SoftwareDistribution i catroot2 z fallbackiem na zmianę nazwy, rejestracja DLL)

### ⚡ Optymalizacja wydajności
- Automatyczne wykrywanie urządzenia: **laptop vs PC stacjonarny**
- Inteligentny dobór **planu zasilania**:
  - **PC stacjonarny** → Wysoka Wydajność
  - **Laptop na Windows 11** (build ≥ 22000) → nakładka **Tryb Zasilania**: podłączony = Najlepsza Wydajność, na baterii = Najlepsza efektywność energetyczna
  - **Laptop na Windows 10** → Wysoka Wydajność z boost CPU podłączony / bez boost na baterii
- Wykrywanie dysku przez `Get-PhysicalDisk` (MediaType: SSD / NVMe / HDD) z fallbackiem na nazwę modelu
- Wyłączenie **hibernacji** — tylko na SSD/NVMe; na HDD pozostaje bez zmian
- Wyłączenie **Fast Startup**
- **SysMain (Superfetch)** — wyłączany tylko na SSD; na HDD pozostaje aktywny
- **DisablePagingExecutive** — włączany tylko przy ≥ 8 GB RAM
- Optymalizacja **pliku stronicowania** (PageFile) — automatycznie dopasowana do ilości RAM przez `CimInstance`
- Tweaki rejestru: priorytety **procesora**, zarządzanie **pamięcią**, animacje UI, aktywne godziny Windows Update (8:00–23:00)
- **Defragmentacja** (HDD) lub **TRIM** (SSD) przez `defrag /O`
- Wyłączenie wybranych programów **startowych**: Skype, OneDrive, Spotify, Discord, Steam, Epic Games, Teams, GOG Galaxy, EA App, Ubisoft Connect, Overwolf, Zoom, Viber

### ⏱️ Optymalizacja latencji (HPET / Timer Resolution)
- Wyłączenie **platform clock** (`bcdedit /set useplatformclock false`)
- Ustawienie **TSC sync policy** na `enhanced`
- Wyłączenie **Dynamic Tick** (`disabledynamictick yes`)
- Aktywacja **GlobalTimerResolutionRequests** w rejestrze
- Efekt: niższa latencja timera systemowego, mniej mikroprzerwań — szczególnie odczuwalne w grach i aplikacjach audio

### 🔕 Narrator i ułatwienia dostępu
- Wyłączenie **Narratora** przez IFEO (Image File Execution Options — przekierowanie na `systray.exe`)
- Wyłączenie **StickyKeys, ToggleKeys, FilterKeys** — koniec z przypadkowym uruchamianiem podczas gry
- Wyciszenie **dźwięków dostępności**
- Wyłączenie automatycznego uruchamiania Narratora przy logowaniu

### 🌐 Sieć
- **Reset proxy** systemowego (WinHTTP i rejestr IE)
- Flush i rejestracja **DNS**
- Reset **stosu TCP/IP** i Winsock
- Ustawienie serwerów DNS: **Cloudflare (1.1.1.1 / 1.0.0.1)** i **Google (8.8.8.8)** dla IPv4 i IPv6
- Optymalizacja parametrów **TCP** (autotuninglevel, RSS, chimney)

### 🛡️ Prywatność i telemetria
- Wyłączenie zbierania danych diagnostycznych (**telemetria**)
- Wyłączenie **Cortany**
- Wyłączenie **Bing Search** w menu Start
- Wyłączenie śledzenia wpisów i kolekcji danych wejściowych (InputPersonalization)
- Wyłączenie sugestii i powiadomień systemowych (ContentDeliveryManager)
- Wyłączenie raportowania błędów Windows (WER)

### ⚙️ Usługi i harmonogramy
- Dezaktywacja zbędnych usług: `DiagTrack`, `WSearch`, `MapsBroker`, `Fax`, `RetailDemo`, `dmwappushservice`; `SysMain` — tylko na SSD
- Wyłączenie zbędnych harmonogramów systemowych (CEIP, feedback, Xbox, dysk, WER)
- Blokada procesów **przeglądarek w tle** i ich autoaktualizacji (Chrome, Edge, Brave, Opera — dynamiczne wyszukiwanie usługi, Firefox, Adobe)

### 🌡️ Pomiar temperatury sprzętu
- Automatyczne pobranie, uruchomienie i usunięcie **OpenHardwareMonitor**
- Odczyt temperatury **CPU** (pakiet / rdzeń #1) i **GPU** przez WMI
- Wyniki zapisywane w raporcie jako `CPU_TEMP °C` i `GPU_TEMP °C`
- Jeśli brak internetu lub OHM nie zadziała — wartości oznaczane jako `niedostępne`

### 🖥️ Interfejs i UX
- Automatyczne **odblokowanie skryptu** (`Unblock-File`) po pobraniu z internetu
- Kolorowy, czytelny interfejs konsolowy (ANSI: aqua / biały / żółty)
- Nagłówek z linkiem do GitHub widoczny od pierwszego uruchomienia
- **Pozycja okna konsoli dostosowywana do DPI monitora** przez `System.Drawing`
- Dynamiczne dopasowanie rozmiaru okna i **bufor konsoli (240 linii)**
- **Sprawdzanie aktualizacji** przez GitHub API przed sprawdzaniem uprawnień
- Trójstanowy system statusów: `[ OK ]` / `[SKIP]` / `[BŁĄD]`
- SFC, DISM i inne długie operacje wyświetlają czytelny **pasek postępu**
- **Czas trwania** mierzony i wyświetlany w formacie `HH:MM:SS`
- Dźwiękowe **powiadomienie o zakończeniu** optymalizacji
- Interaktywne pytania na koniec: Malwarebytes, wsparcie projektu, restart
- Raport zapisywany w **Dokumentach** z unikalną nazwą zawierającą datę i czas

---

## 🚀 Wymagania

| Wymaganie | Szczegóły |
|-----------|-----------|
| System | Windows 10 / Windows 11 |
| Uprawnienia | **Administrator** (wymagane) |
| Połączenie | Opcjonalne — potrzebne dla sprawdzania wersji, AdwCleaner, Malwarebytes, OHM |
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

Po zakończeniu działania skrypt automatycznie zapisuje plik tekstowy w **Dokumentach**, z unikalną nazwą zawierającą datę i czas:

```
%USERPROFILE%\Documents\CLEANER by MAG\Raport_YYYY-MM-DD_HH-mm-ss.txt
```

Każde uruchomienie tworzy **nowy plik** — poprzednie raporty nie są nadpisywane. Raport zawiera:
- datę i czas wykonania (start, koniec, łączny czas trwania `HH:MM:SS`)
- ilość zwolnionego miejsca na dysku C:
- parametry systemu: RAM, pagefile, model dysku, CPU, GPU, wersja Windows + build number
- temperatury CPU i GPU zmierzone przez OpenHardwareMonitor
- rozmiar kolejki drukarki i CBS.log przed czyszczeniem
- listę wszystkich operacji ze statusem: `[ OK ]`, `[ SKIP ]`, `[ BŁĄD ]`
- nazwę komputera i użytkownika

---

## 🗂️ Wykonywane operacje — podsumowanie

| Operacja | Opis |
|----------|------|
| Punkt przywracania systemu | Tworzy przed jakimikolwiek zmianami (z weryfikacją) |
| Windows Defender | Aktualizacja sygnatur + szybki skan (z kodem błędu) |
| AdwCleaner | Skan i czyszczenie adware; `SKIP` jeśli brak internetu |
| Reset proxy | Usuwa proxy systemowe i IE/WinHTTP |
| Pliki tymczasowe | `%TEMP%` i `C:\Windows\Temp` |
| Prefetch | Czyszczony na HDD; `SKIP` na SSD |
| DNS / Sieć | Flush, reset TCP/IP, Cloudflare + Google DNS (IPv4 + IPv6) |
| Dzienniki zdarzeń | Czyści cały Event Viewer |
| Usługi systemowe | SysMain (tylko SSD), DiagTrack, WSearch, MapsBroker, Fax, RetailDemo, dmwappushservice |
| Optymalizacja rejestru | Tweaki wydajności, prywatności, UI i Windows Update |
| Cache przeglądarek | Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox, LibreWolf, Zen Browser, Floorp, Thunderbird — dla każdego konta |
| Cache Microsoft Teams | Klasyczny + nowy UWP (MSTeams_*) |
| Cache OneDrive | Logi, setup/logs, .deadLetterQueue |
| Cache Java i .NET | Java Deployment Cache + Temporary ASP.NET Files |
| Kolejka drukarki | Czyści spooler + usuwa stare sterowniki; `SKIP` jeśli pusta |
| Microsoft Store cache | Czyści cache paczek AppX |
| Oczyszczanie dysku | cleanmgr z 34 kategoriami, okno na pierwszym planie; `SKIP` jeśli niedostępny |
| Logi instalatorów | CBS, DISM, MeasuredBoot, NetSetup, waasmedic, INF (*.log, *.pnf) |
| SFC (wstępny) | Sprawdza integralność plików — pasek postępu + kod błędu |
| DISM RestoreHealth | Naprawia obraz systemu — pasek postępu + kod błędu |
| SFC (końcowy) | Ponowna weryfikacja po DISM — pasek postępu + kod błędu |
| DISM StartComponentCleanup | Czyści stare składniki aktualizacji |
| Logi CBS | Usuwa logi CBS przed i po SFC/DISM |
| Windows Update cache | Pełny reset usług + SoftwareDistribution + catroot2 |
| Usunięcie Windows.old | `takeown` + usunięcie; `SKIP` jeśli nie istnieje |
| Defragmentacja / TRIM | `defrag /O` — zależnie od wykrytego typu dysku |
| Thumbnail Cache | Czyści cache miniatur, restartuje Eksplorator |
| Naprawa WMI | Weryfikacja → Salvage → Reset; `SKIP` jeśli baza zdrowa |
| Pliki Minidump | Usuwa zrzuty awaryjne |
| Raporty WER | Czyści raporty błędów Windows |
| HPET / Timer Resolution | useplatformclock=false, tscsync=enhanced, dynamictick=off |
| Narrator / Ease of Access | Wyłączenie Narratora, StickyKeys, ToggleKeys, FilterKeys |
| Temperatury CPU / GPU | Pobiera OHM, odczytuje temp. przez WMI, usuwa OHM |

---

## ⚠️ Ważne uwagi

- Skrypt **planuje CHKDSK** na następny restart (tylko jeśli nie był już zaplanowany) — wymagane ponowne uruchomienie
- Wyłączona jest **hibernacja** — ale tylko na SSD/NVMe; na HDD pozostaje aktywna
- Usługa **Windows Search (WSearch)** zostaje wyłączona — może wpłynąć na wyszukiwanie w menu Start
- Usługa **SysMain (Superfetch)** wyłączana jest tylko na SSD; na HDD pozostaje aktywna
- Folder **Prefetch** jest czyszczony tylko na HDD — na SSD Windows zarządza nim automatycznie
- Tweaki **HPET/Timer Resolution** modyfikują ustawienia bootloadera (`bcdedit`) — zmiany wymagają restartu
- Wyłączenie **Narratora** realizowane jest przez IFEO (przekierowanie procesu) — można cofnąć ręcznie w rejestrze
- Skrypt usuwa wybrane **wbudowane aplikacje** Windows: Solitaire, Bing, Zune, People, Skype, Office Hub, 3D Builder, Get Started
- Wyłączane są wpisy autostartu popularnych aplikacji (Spotify, Discord, Steam, Epic, Teams itd.) — można je ponownie włączyć ręcznie
- Folder **Windows.old** jest usuwany trwale przy użyciu `takeown` i `icacls` — nie ma możliwości cofnięcia do poprzedniej wersji Windows przez ustawienia systemowe
- Pobieranie i uruchamianie **OpenHardwareMonitor** wymaga połączenia z internetem; bez połączenia temperatury są oznaczane jako `niedostępne`

---

## 🔄 Co nowego w v1.12?

- ✅ **HPET / Timer Resolution** — nowa sekcja optymalizacji latencji: `useplatformclock=false`, `tscsyncpolicy=enhanced`, `disabledynamictick=yes`, `GlobalTimerResolutionRequests`; szczególnie przydatne dla graczy i producentów audio
- ✅ **Narrator i ułatwienia dostępu** — wyłączenie Narratora przez IFEO, dezaktywacja StickyKeys / ToggleKeys / FilterKeys i dźwięków dostępności
- ✅ **Logi instalatorów** — nowa sekcja czyszcząca CBS, DISM, MeasuredBoot, NetSetup, waasmedic i INF (`*.log`, `*.pnf`)
- ✅ **Inteligentny plan zasilania dla laptopa** — na Windows 11 używa nakładki **Tryb Zasilania** zamiast klasycznego planu; automatyczne rozróżnienie podłączony/na baterii z odpowiednim profilem
- ✅ **Raport w Dokumentach z unikatową nazwą** — `Raport_YYYY-MM-DD_HH-mm-ss.txt` w folderze `Dokumenty\CLEANER by MAG\`; każde uruchomienie tworzy nowy plik
- ✅ **Pozycja okna konsoli skalowana do DPI** — `System.Drawing.Graphics.DpiX` zapewnia prawidłowe pozycjonowanie na ekranach HiDPI/4K
- ✅ **Czyszczenie klucza konsoli** przed startem (`reg delete`) — brak konfliktów ze starymi ustawieniami z poprzednich wersji
- ✅ **CHKDSK** — sprawdza czy już zaplanowany przed dodaniem nowego wpisu; `[ SKIP ]` jeśli tak
- ✅ **cleanmgr** — podczas oczyszczania Win32 API centruje kursor myszy i wysuwa okno na pierwszy plan, żeby nie zginęło za innymi oknami
- ✅ **catroot2** — jeśli nie można usunąć (plik zajęty), automatycznie zmienia nazwę z losowym sufiksem zamiast cicho pomijać operację

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

**Autor:** Mag | **Wersja:** 1.12 (29/04/2026)
