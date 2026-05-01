# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.14-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do kompleksowej optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od skanowania antywirusowego i usuwania śmieciowych plików, przez naprawę plików systemowych, aż po konfigurację sieci i rejestru. Skrypt **inteligentnie dostosowuje się do sprzętu i systemu**, instaluje się w Menu Start i po każdym uruchomieniu pokazuje historię wszystkich poprzednich sesji czyszczenia.

---

## ✨ Co robi skrypt?

### 🔄 Automatyczna aktualizacja i instalacja
- Przy starcie sprawdza dostępność nowej wersji przez **GitHub API**
- Jeśli dostępna aktualizacja — pobiera plik `.bat` bezpośrednio z GitHub Releases i **automatycznie uruchamia nową wersję** bez konieczności otwierania przeglądarki
- Przy pierwszym uruchomieniu **instaluje się w Menu Start** ze skrótem uruchamiającym jako administrator
- Tworzy **skrót do folderu raportów** w Menu Start prowadzący do `Dokumenty\CLEANER by MAG`

### 🔒 Bezpieczeństwo i antywirusy
- Tworzy **punkt przywracania systemu** przed wprowadzeniem jakichkolwiek zmian (z weryfikacją powodzenia)
- Aktualizuje i uruchamia **szybki skan Windows Defender** z raportowaniem kodu błędu
- Pobiera, uruchamia i automatycznie usuwa **AdwCleaner** (skan + czyszczenie adware); brak internetu oznacza `[ SKIP ]`
- Opcjonalne pobranie, skan i deinstalacja **Malwarebytes** po zakończeniu

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
- Cache **Microsoft Store** (LocalCache, INetCache, Temp dla wszystkich pakietów AppX; z czyszczeniem kluczy rejestru Store)
- Kolejka **drukarki (Spooler)** + stare sterowniki drukarek (W32X86, x64, ia64; aktywne zachowane); `[ SKIP ]` jeśli kolejka pusta
- Logi **CBS** i Windows Update (`SoftwareDistribution`, `catroot2` — z fallbackiem na zmianę nazwy)
- Cache miniatur z restartem Eksploratora
- Folder **Windows.old** — `takeown` + `icacls` przed usunięciem; `[ SKIP ]` jeśli nie istnieje
- **Oczyszczanie dysku** (`cleanmgr`) z konfiguracją 34 kategorii przez rejestr, okno wysuwane na pierwszy plan; `[ SKIP ]` jeśli `cleanmgr` niedostępny

### 🔧 Naprawa systemu
- **SFC** (System File Checker) — dwukrotnie: przed i po DISM, z paskiem postępu i kodem błędu w raporcie
- **DISM RestoreHealth** — naprawa obrazu systemu, z paskiem postępu i kodem błędu; ostrzeżenie o nieprzerywaniu
- **DISM StartComponentCleanup** — czyszczenie bazy składników po aktualizacjach
- **CHKDSK** — planowany na następny restart; `[ SKIP ]` jeśli już zaplanowany
- **Inteligentna naprawa WMI** — weryfikacja → Salvage → Reset z rejestracją MOF/MFL; `[ SKIP ]` jeśli baza jest zdrowa
- Pełny reset **Windows Update** (zatrzymanie usług, usunięcie SoftwareDistribution i catroot2 z fallbackiem na zmianę nazwy, rejestracja DLL)

### 🗂️ Czyszczenie przestarzałych sterowników PnP
- Automatyczne wykrycie wszystkich zainstalowanych sterowników `oem*.inf` przez `Win32_PnPSignedDriver`
- Grupowanie według urządzenia i producenta, sortowanie według numeru wersji
- Usunięcie przez `pnputil /delete-driver` wszystkich starszych wersji — **najnowsza zawsze zachowana**
- Windows automatycznie blokuje usunięcie sterowników aktywnie używanych przez podłączone urządzenia
- Raport w konsoli i pliku: liczba zachowanych / usuniętych / błędów; `[ SKIP ]` jeśli brak przestarzałych

### ⚡ Optymalizacja wydajności
- Automatyczne wykrywanie urządzenia: **laptop vs PC stacjonarny**
- Inteligentny dobór **planu zasilania**:
  - **PC stacjonarny** → Wysoka Wydajność
  - **Laptop na Windows 11** (build ≥ 22000) → globalna nakładka **Tryb Zasilania** (`ActiveOverlayAcPowerScheme` / `ActiveOverlayDcPowerScheme`): podłączony = Najlepsza Wydajność, na baterii = Najlepsza efektywność energetyczna
  - **Laptop na Windows 10** → Wysoka Wydajność z boost CPU podłączony / bez boost na baterii
- Wykrywanie dysku przez `Get-PhysicalDisk` (MediaType: SSD / NVMe / HDD) z fallbackiem na nazwę modelu
- Wyłączenie **hibernacji** — tylko na SSD/NVMe; na HDD pozostaje bez zmian
- Wyłączenie **Fast Startup**
- **SysMain (Superfetch)** — wyłączany tylko na SSD; na HDD pozostaje aktywny
- **DisablePagingExecutive** — włączany przy ≥ 8000 MB RAM
- Optymalizacja **pliku stronicowania** (PageFile): 8192/8192 MB dla systemów ≤ 8 GB RAM; 4096/4096 MB dla systemów z > 8 GB RAM
- Tweaki rejestru: priorytety **procesora**, zarządzanie **pamięcią**, animacje UI, aktywne godziny Windows Update (8:00–23:00)
- **Defragmentacja** (HDD) lub **TRIM** (SSD) przez `defrag /O`
- Wyłączenie wybranych programów **startowych**: Skype, OneDrive, Spotify, Discord, Steam, Epic Games, Teams, GOG Galaxy, EA App, Ubisoft Connect, Overwolf, Zoom, Viber

### ⏱️ Optymalizacja latencji (HPET / Timer Resolution)
- Wyłączenie **platform clock** (`bcdedit /set useplatformclock false`)
- Ustawienie **TSC sync policy** na `enhanced`
- Wyłączenie **Dynamic Tick** (`disabledynamictick yes`)
- Aktywacja **GlobalTimerResolutionRequests** w rejestrze
- Efekt: niższa latencja timera systemowego — szczególnie odczuwalna w grach i aplikacjach audio

### 🔕 Narrator i ułatwienia dostępu
- Wyłączenie **Narratora** przez IFEO (przekierowanie na `systray.exe`) i klucze polityk
- Wyłączenie **StickyKeys, ToggleKeys, FilterKeys**
- Wyciszenie **dźwięków dostępności**
- Wyłączenie automatycznego uruchamiania Narratora przy logowaniu

### 🌐 Sieć
- **Reset proxy** systemowego (WinHTTP i rejestr IE)
- Flush i rejestracja **DNS**
- Reset **stosu TCP/IP** i Winsock
- Ustawienie serwerów DNS: **Cloudflare (1.1.1.1 / 1.0.0.1)** i **Google (8.8.8.8)** dla IPv4 i IPv6 — dla każdego aktywnego interfejsu sieciowego
- Optymalizacja parametrów **TCP** (autotuninglevel, RSS, chimney)

### 🛡️ Prywatność i telemetria
- Wyłączenie zbierania danych diagnostycznych (**telemetria**)
- Wyłączenie **Cortany**
- Wyłączenie **Bing Search** w menu Start
- Wyłączenie śledzenia wpisów i kolekcji danych wejściowych (InputPersonalization, TIPC)
- Wyłączenie sugestii i powiadomień systemowych (ContentDeliveryManager)
- Wyłączenie raportowania błędów Windows (WER)

### ⚙️ Usługi i harmonogramy
- Dezaktywacja zbędnych usług: `DiagTrack`, `WSearch`, `MapsBroker`, `Fax`, `RetailDemo`, `dmwappushservice`; `SysMain` — tylko na SSD
- Wyłączenie zbędnych harmonogramów systemowych (CEIP, feedback, Xbox, dysk, WER)
- Blokada procesów **przeglądarek w tle** i ich autoaktualizacji (Chrome, Edge, Brave, Opera — dynamiczne wyszukiwanie usługi, Firefox, Adobe)

### 🌡️ Pomiar temperatury sprzętu
- Automatyczne pobranie, uruchomienie i usunięcie **OpenHardwareMonitor** (z retry przy usuwaniu)
- Odczyt temperatury **CPU** i **GPU** przez WMI
- Wyniki zapisywane w raporcie; jeśli brak internetu lub OHM nie zadziała — wartości oznaczane jako `niedostępne`

### 📊 Historia optymalizacji
- Po każdym uruchomieniu wyświetlana jest **tabela wszystkich poprzednich sesji** czyszczenia
- Dla każdej sesji pokazywane są: data, czas trwania, zwolnione MB i proporcjonalny pasek wizualny `#`
- Podsumowanie: łączna liczba sesji, łączne zwolnione MB, średnia MB na sesję

### 🖥️ Interfejs i UX
- Automatyczne **odblokowanie skryptu** (`Unblock-File`) po pobraniu z internetu
- Kolorowy, czytelny interfejs konsolowy (ANSI: aqua / biały / żółty)
- Nagłówek z linkiem do GitHub widoczny od pierwszego uruchomienia
- **Pozycja okna konsoli skalowana do DPI monitora** przez `System.Drawing`
- Dynamiczne dopasowanie rozmiaru okna i **bufor konsoli (240 linii)**
- **Automatyczna aktualizacja jednym kliknięciem** — pobieranie i podmiana pliku bez otwierania przeglądarki
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
| Połączenie | Opcjonalne — potrzebne dla aktualizacji, AdwCleaner, Malwarebytes, OHM |
| PowerShell | 5.x (wbudowany w Windows) |

---

## 📥 Instalacja i uruchomienie

1. Pobierz najnowszą wersję ze strony [Releases](https://github.com/dineromagiero/CLEANER-by-MAG/releases/latest)
2. Kliknij **prawym przyciskiem myszy** na plik `.bat`
3. Wybierz **„Uruchom jako administrator"**
4. Postępuj zgodnie z instrukcjami na ekranie

> Po pierwszym uruchomieniu skrypt instaluje się w Menu Start — kolejne uruchomienia możliwe są bezpośrednio stamtąd bez potrzeby szukania pliku `.bat`.

> ⚠️ **Uwaga:** Skrypt **musi** być uruchomiony jako administrator. Bez uprawnień administratora wyświetli czytelny komunikat z instrukcją i zamknie się po 10 sekundach.

---

## 📊 Raport wynikowy

Po zakończeniu działania skrypt automatycznie zapisuje plik tekstowy w **Dokumentach**:

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
| DNS / Sieć | Flush, reset TCP/IP, Cloudflare + Google DNS dla wszystkich interfejsów |
| Dzienniki zdarzeń | Czyści cały Event Viewer |
| Usługi systemowe | SysMain (tylko SSD), DiagTrack, WSearch, MapsBroker, Fax, RetailDemo, dmwappushservice |
| Optymalizacja rejestru | Tweaki wydajności, prywatności, UI i Windows Update |
| Cache przeglądarek | Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox, LibreWolf, Zen Browser, Floorp, Thunderbird — dla każdego konta |
| Cache Microsoft Teams | Klasyczny + nowy UWP (MSTeams_*) |
| Cache OneDrive | Logi, setup/logs, .deadLetterQueue |
| Cache Java i .NET | Java Deployment Cache + Temporary ASP.NET Files |
| Kolejka drukarki | Czyści spooler + usuwa stare sterowniki drukarek; `SKIP` jeśli pusta |
| Stare sterowniki PnP | `pnputil /delete-driver` dla starszych wersji każdego urządzenia; `SKIP` jeśli brak |
| Microsoft Store cache | Czyści cache AppX + klucze rejestru Store |
| Oczyszczanie dysku | cleanmgr z 34 kategoriami, okno na pierwszym planie; `SKIP` jeśli niedostępny |
| Logi instalatorów | CBS, DISM, MeasuredBoot, NetSetup, waasmedic, INF (*.log, *.pnf) |
| SFC (wstępny) | Sprawdza integralność plików — pasek postępu + kod błędu |
| DISM RestoreHealth | Naprawia obraz systemu — pasek postępu + kod błędu |
| SFC (końcowy) | Ponowna weryfikacja po DISM — pasek postępu + kod błędu |
| DISM StartComponentCleanup | Czyści stare składniki aktualizacji |
| Logi CBS | Usuwa CBS.log, .cab i .persist |
| Windows Update cache | Pełny reset usług + SoftwareDistribution + catroot2 |
| Usunięcie Windows.old | `takeown` + usunięcie; `SKIP` jeśli nie istnieje |
| Defragmentacja / TRIM | `defrag /O` — zależnie od wykrytego typu dysku |
| Thumbnail Cache | Czyści cache miniatur i ikon, restartuje Eksplorator |
| Naprawa WMI | Weryfikacja → Salvage → Reset z MOF/MFL; `SKIP` jeśli baza zdrowa |
| Pliki Minidump | Usuwa zrzuty awaryjne |
| Raporty WER | Czyści raporty błędów Windows |
| HPET / Timer Resolution | useplatformclock=false, tscsync=enhanced, dynamictick=off |
| Narrator / Ease of Access | Wyłączenie Narratora, StickyKeys, ToggleKeys, FilterKeys |
| Temperatury CPU / GPU | Pobiera OHM, odczytuje temp., usuwa OHM (retry przy usuwaniu) |

---

## ⚠️ Ważne uwagi

- Skrypt **planuje CHKDSK** na następny restart (tylko jeśli nie był już zaplanowany) — wymagane ponowne uruchomienie
- Wyłączona jest **hibernacja** — ale tylko na SSD/NVMe; na HDD pozostaje aktywna
- Usługa **Windows Search (WSearch)** zostaje wyłączona — może wpłynąć na wyszukiwanie w menu Start
- Usługa **SysMain (Superfetch)** wyłączana jest tylko na SSD; na HDD pozostaje aktywna
- Folder **Prefetch** jest czyszczony tylko na HDD — na SSD Windows zarządza nim automatycznie
- Tweaki **HPET/Timer Resolution** modyfikują ustawienia bootloadera (`bcdedit`) — zmiany wymagają restartu
- Wyłączenie **Narratora** realizowane jest przez IFEO (przekierowanie procesu) — można cofnąć ręcznie w rejestrze
- Czyszczenie **sterowników PnP** usuwa wyłącznie starsze wersje; najnowszy sterownik każdego urządzenia zawsze zostaje zachowany; Windows automatycznie blokuje usunięcie aktywnych sterowników
- Skrypt usuwa wybrane **wbudowane aplikacje** Windows: Solitaire, Bing, Zune, People, Skype, Office Hub, 3D Builder, Get Started
- Wyłączane są wpisy autostartu popularnych aplikacji (Spotify, Discord, Steam, Epic, Teams itd.) — można je ponownie włączyć ręcznie
- Folder **Windows.old** jest usuwany trwale — nie ma możliwości cofnięcia do poprzedniej wersji Windows przez ustawienia systemowe

---

## 🔄 Co nowego w v1.14?

- ✅ **Czyszczenie przestarzałych sterowników PnP** — nowa operacja: skrypt grupuje wszystkie `oem*.inf` według urządzenia i producenta, sortuje po numerze wersji i usuwa przez `pnputil /delete-driver` wszystko poza najnowszą; Windows sam blokuje usunięcie aktywnych sterowników; w raporcie widać liczniki zachowanych, usuniętych i błędów
- ✅ **Historia optymalizacji** — po zakończeniu wyświetlana jest tabela wszystkich poprzednich sesji z folderu `Dokumenty\CLEANER by MAG\`: data, czas trwania, zwolnione MB i wizualny pasek proporcjonalny do najlepszego wyniku; na dole podsumowanie łączne
- ✅ **Poprawka logiki PageFile** — odwrócona: systemy z ≤ 8 GB RAM dostają 8192/8192 MB pagefile (potrzebują większego buforu), systemy z > 8 GB RAM dostają 4096/4096 MB (mniej potrzebne przy dużej ilości RAM)
- ✅ **Poprawka nakładki zasilania Win11** — zmiana z `OverlaySchemeKey` (per-schemat, nieskuteczna) na `ActiveOverlayAcPowerScheme`/`ActiveOverlayDcPowerScheme` w kluczu globalnym `PowerSchemes` — nakładka Trybu Zasilania działa teraz prawidłowo
- ✅ **Skrót w Menu Start** — zmiana `/c` na `/k` w argumencie `cmd.exe` — okno konsoli pozostaje widoczne po zakończeniu skryptu zamiast natychmiast się zamykać
- ✅ **OHM — retry przy usuwaniu** — pętla do 5 prób z przerwą 1 sekundy przy usuwaniu folderu OpenHardwareMonitor, żeby nie zostawał po sobie gdy proces OHM nie zdążył się zamknąć

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

**Autor:** Mag | **Wersja:** 1.14 (1/05/2026)
