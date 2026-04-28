# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.11-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do kompleksowej optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od skanowania antywirusowego i usuwania śmieciowych plików, przez naprawę plików systemowych, aż po konfigurację sieci i rejestru. Skrypt **inteligentnie dostosowuje się do sprzętu** i generuje szczegółowy raport zawierający m.in. temperatury CPU i GPU zmierzone podczas działania.

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
- Cache przeglądarek — **dla każdego konta użytkownika na komputerze** (pętla po `C:\Users\*`): **Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox, LibreWolf, Zen Browser, Floorp, Thunderbird** (domyślny profil + wszystkie `Profile *`)
- Cache **Microsoft Teams** — obsługa obu wersji: klasycznej i nowej UWP (`MSTeams_*\LocalCache`)
- Cache **Microsoft OneDrive** — logi, setup/logs, `.deadLetterQueue`
- Cache **Java** (`AppData\LocalLow\Sun\Java\Deployment\cache`)
- Pliki tymczasowe **ASP.NET** (`Temporary ASP.NET Files`)
- Cache **Microsoft Store** (LocalCache, INetCache, Temp dla wszystkich pakietów AppX)
- Kolejka **drukarki (Spooler)** + stare sterowniki drukarek (W32X86, x64, ia64; aktywne zachowane)
- Logi **CBS** i Windows Update (`SoftwareDistribution`, `catroot2`)
- Cache miniatur z restartem Eksploratora
- Folder **Windows.old** — podawany jest rozmiar przed usunięciem; `[ SKIP ]` jeśli nie istnieje
- **Oczyszczanie dysku** (`cleanmgr`) z konfiguracją 34 kategorii przez rejestr; `[ SKIP ]` jeśli `cleanmgr` niedostępny

### 🔧 Naprawa systemu
- **SFC** (System File Checker) — dwukrotnie: przed i po DISM, z paskiem postępu i kodem błędu w raporcie
- **DISM RestoreHealth** — naprawa obrazu systemu, z paskiem postępu i kodem błędu; ostrzeżenie w konsoli, aby nie przerywać procesu
- **DISM StartComponentCleanup** — czyszczenie bazy składników po aktualizacjach
- **CHKDSK** zaplanowany na następny restart
- **Inteligentna naprawa WMI** — weryfikacja → Salvage → Reset; `[ SKIP ]` jeśli baza jest zdrowa
- Pełny reset **Windows Update** (zatrzymanie usług, usunięcie SoftwareDistribution i catroot2, rejestracja DLL)

### ⚡ Optymalizacja wydajności
- Automatyczne wykrywanie urządzenia: **laptop vs PC stacjonarny**
- Ustawienie **planu zasilania Wysoka Wydajność** (tylko dla PC stacjonarnych)
- Wykrywanie dysku przez `Get-PhysicalDisk` (MediaType: SSD / NVMe / HDD) z fallbackiem na nazwę modelu
- Wyłączenie **hibernacji** — tylko na SSD/NVMe; na HDD pozostaje bez zmian
- Wyłączenie **Fast Startup**
- **SysMain (Superfetch)** — wyłączany tylko na SSD; na HDD pozostaje aktywny
- **DisablePagingExecutive** — włączany tylko przy ≥ 8 GB RAM
- Optymalizacja **pliku stronicowania** (PageFile) — automatycznie dopasowana do ilości RAM
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

### 🌡️ Pomiar temperatury sprzętu
- Automatyczne pobranie, uruchomienie i usunięcie **OpenHardwareMonitor**
- Odczyt temperatury **CPU** (pakiet / rdzeń #1) i **GPU** przez WMI
- Wyniki zapisywane w raporcie jako `CPU_TEMP °C` i `GPU_TEMP °C`
- Jeśli brak internetu lub OHM nie zadziała — wartości oznaczane jako `niedostępne`

### 🖥️ Interfejs i UX
- Automatyczne **odblokowanie skryptu** (`Unblock-File`) po pobraniu z internetu
- Kolorowy, czytelny interfejs konsolowy (ANSI: aqua / biały / żółty)
- Nagłówek z linkiem do GitHub widoczny od pierwszego uruchomienia
- Dynamiczne dopasowanie rozmiaru okna i **bufor konsoli (240 linii)**
- **Sprawdzanie aktualizacji** przez GitHub API przed sprawdzaniem uprawnień
- Trójstanowy system statusów w konsoli i raporcie: `[ OK ]` / `[SKIP]` / `[BŁĄD]`
- SFC, DISM i inne długie operacje wyświetlają czytelny **pasek postępu**
- **Czas trwania** mierzony i wyświetlany w formacie `HH:MM:SS`
- Data i czas przez `Get-Date` — format niezależny od regionalnych ustawień systemu
- Dźwiękowe **powiadomienie o zakończeniu** optymalizacji
- Interaktywne pytania na koniec: Malwarebytes, wsparcie projektu, restart
- Raport zapisywany na **Pulpicie** użytkownika: `CLEANER by MAG - WYNIKI.txt`

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

Po zakończeniu działania skrypt automatycznie zapisuje plik tekstowy na **Pulpicie**:

```
%USERPROFILE%\Desktop\CLEANER by MAG - WYNIKI.txt
```

Raport zawiera:
- datę i czas wykonania (start, koniec, łączny czas trwania w formacie `HH:MM:SS`)
- ilość zwolnionego miejsca na dysku C:
- parametry systemu: RAM, pagefile, model dysku, **CPU, GPU, wersja Windows + build number**
- **temperatury CPU i GPU** zmierzone przez OpenHardwareMonitor
- rozmiar kolejki drukarki i CBS.log przed czyszczeniem
- listę wszystkich wykonanych operacji ze statusem: `[ OK ]`, `[ SKIP ]`, `[ BŁĄD ]`
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
| Usługi systemowe | SysMain (tylko SSD), DiagTrack, WSearch, MapsBroker, Fax, RetailDemo |
| Optymalizacja rejestru | Tweaki wydajności, prywatności, UI i Windows Update |
| Cache przeglądarek | Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox, LibreWolf, Zen Browser, **Floorp, Thunderbird** — dla każdego konta |
| Cache Microsoft Teams | Klasyczny + nowy UWP (MSTeams_*) |
| Cache OneDrive | Logi, setup/logs, .deadLetterQueue |
| Cache Java i .NET | Java Deployment Cache + Temporary ASP.NET Files |
| Kolejka drukarki | Czyści spooler + usuwa stare sterowniki; `SKIP` jeśli pusta |
| Microsoft Store cache | Czyści cache paczek AppX |
| Oczyszczanie dysku | cleanmgr z 34 kategoriami; `SKIP` jeśli niedostępny |
| SFC (wstępny) | Sprawdza integralność plików — pasek postępu + kod błędu |
| DISM RestoreHealth | Naprawia obraz systemu — pasek postępu + kod błędu |
| SFC (końcowy) | Ponowna weryfikacja po DISM — pasek postępu + kod błędu |
| DISM StartComponentCleanup | Czyści stare składniki aktualizacji |
| Logi CBS | Usuwa duże logi CBS |
| Windows Update cache | Pełny reset usług i SoftwareDistribution |
| Usunięcie Windows.old | Odzyskuje miejsce z podaniem rozmiaru; `SKIP` jeśli nie istnieje |
| Defragmentacja / TRIM | `defrag /O` — zależnie od wykrytego typu dysku |
| Thumbnail Cache | Czyści cache miniatur, restartuje Eksplorator |
| Naprawa WMI | Weryfikacja → Salvage → Reset; `SKIP` jeśli baza zdrowa |
| Pliki Minidump | Usuwa zrzuty awaryjne |
| Raporty WER | Czyści raporty błędów Windows |
| Temperatury CPU / GPU | Pobiera i uruchamia OHM, odczytuje temp., usuwa OHM |

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
- Pobieranie i uruchamianie **OpenHardwareMonitor** wymaga połączenia z internetem; bez połączenia temperatury są oznaczane jako `niedostępne`

---

## 🔄 Co nowego w v1.11?

- ✅ **Pomiar temperatury CPU i GPU** — automatyczne pobranie OpenHardwareMonitor, odczyt przez WMI, wyniki w raporcie; OHM usuwany natychmiast po pomiarze
- ✅ **Cache OneDrive** — nowa sekcja: logi, setup/logs i `.deadLetterQueue`; własna pozycja w raporcie
- ✅ **Cache Java i ASP.NET** — czyszczenie Java Deployment Cache i tymczasowych plików .NET
- ✅ **Nowe przeglądarki: Floorp i Thunderbird** — łącznie 12 obsługiwanych aplikacji w sekcji cache przeglądarek
- ✅ **Pełna informacja o sprzęcie w raporcie** — nazwa CPU, GPU, wersja Windows z numerem build
- ✅ **Status `[BŁĄD]`** — nieudane operacje oznaczane są teraz czytelnym `[BŁĄD]` w kolorze żółtym zamiast neutralnego `[----]`
- ✅ **Kolejka drukarki** — jeśli była pusta przed czyszczeniem, status w raporcie to `[ SKIP ]`
- ✅ **Data i czas przez `Get-Date`** — format `yyyy-MM-dd` i `HH:mm:ss` niezależny od regionalnych ustawień systemu
- ✅ **Malwarebytes** — dynamiczne wyszukiwanie `mbam.exe` przez PowerShell w obu lokalizacjach Program Files i ścieżce systemowej
- ✅ **Zabezpieczenie przed ujemnym SPACE_FREED** — jeśli obliczone zwolnione miejsce byłoby ujemne (np. przez pliki OHM), wartość jest zerowana

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

**Autor:** Mag | **Wersja:** 1.11 (28/04/2026)
