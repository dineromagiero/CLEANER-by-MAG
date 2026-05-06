# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.17-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do kompleksowej optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od skanowania antywirusowego i usuwania śmieciowych plików, przez naprawę plików systemowych, aż po konfigurację sieci i rejestru. Skrypt wykonuje **31 ponumerowanych kroków**, inteligentnie dostosowuje się do sprzętu i systemu, instaluje się w Menu Start i po każdym uruchomieniu pokazuje historię wszystkich poprzednich sesji czyszczenia.

---

## ✨ Co robi skrypt?

### 🔄 Automatyczna aktualizacja i instalacja
- Przy starcie sprawdza dostępność nowej wersji przez **GitHub API**
- Jeśli dostępna aktualizacja — pobiera plik `.bat` przez dedykowany skrypt pomocniczy uruchamiany w tle (`updater_helper.bat`), co eliminuje konflikt blokady pliku podczas podmiany
- Przy pierwszym uruchomieniu **instaluje się w Menu Start** ze skrótem uruchamiającym jako administrator
- Tworzy **skrót do folderu raportów** w Menu Start

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
- **DISM RestoreHealth** — naprawa obrazu systemu, z paskiem postępu i kodem błędu
- **DISM StartComponentCleanup** — czyszczenie bazy składników po aktualizacjach
- **CHKDSK** — planowany na następny restart; `[ SKIP ]` jeśli już zaplanowany
- **Inteligentna naprawa WMI** — weryfikacja → Salvage → Reset z rejestracją MOF/MFL; `[ SKIP ]` jeśli baza jest zdrowa
- Pełny reset **Windows Update** (zatrzymanie usług, usunięcie SoftwareDistribution i catroot2 z fallbackiem, rejestracja DLL)

### 🗂️ Czyszczenie przestarzałych sterowników PnP
- Automatyczne wykrycie wszystkich zainstalowanych sterowników `oem*.inf` przez `Win32_PnPSignedDriver`
- Grupowanie według urządzenia i producenta, sortowanie według numeru wersji
- Usunięcie przez `pnputil /delete-driver` wszystkich starszych wersji — **najnowsza zawsze zachowana**
- Windows automatycznie blokuje usunięcie sterowników aktywnie używanych
- Raport w konsoli i pliku: liczba zachowanych / usuniętych / błędów; `[ SKIP ]` jeśli brak przestarzałych

### ⚡ Optymalizacja wydajności
- Automatyczne wykrywanie urządzenia: **laptop vs PC stacjonarny**
- Inteligentny dobór **planu zasilania**:
  - **PC stacjonarny** → Wysoka Wydajność
  - **Laptop na Windows 11** (build ≥ 22000) → globalna nakładka **Tryb Zasilania**: podłączony = Najlepsza Wydajność, na baterii = Najlepsza efektywność energetyczna
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

### 🔕 Narrator i ułatwienia dostępu
- Wyłączenie **Narratora** przez IFEO i klucze polityk
- Wyłączenie **StickyKeys, ToggleKeys, FilterKeys**
- Wyciszenie **dźwięków dostępności** i wyłączenie autostartu Narratora przy logowaniu

### 🌐 Sieć i Delivery Optimization
- **Reset proxy** systemowego (WinHTTP i rejestr IE)
- Flush i rejestracja **DNS**
- Reset **stosu TCP/IP** i Winsock
- Ustawienie serwerów DNS: **Cloudflare (1.1.1.1 / 1.0.0.1)** i **Google (8.8.8.8)** dla IPv4 i IPv6 — dla każdego aktywnego interfejsu sieciowego
- Optymalizacja parametrów **TCP** (autotuninglevel, RSS, chimney)
- **Wyłączenie trybu P2P Delivery Optimization** (`DODownloadMode=0`) — aktualizacje Windows nie są udostępniane innym komputerom w sieci

### 🛡️ Prywatność i telemetria
- Wyłączenie zbierania danych diagnostycznych (**telemetria**)
- Wyłączenie **Cortany**
- Wyłączenie **Bing Search** w menu Start
- Wyłączenie śledzenia wpisów i kolekcji danych wejściowych (InputPersonalization, TIPC)
- Wyłączenie sugestii i powiadomień systemowych (ContentDeliveryManager)
- Wyłączenie raportowania błędów Windows (WER)

### ⚙️ Usługi, harmonogramy i autoaktualizacje
- Dezaktywacja zbędnych usług: `DiagTrack`, `WSearch`, `MapsBroker`, `Fax`, `RetailDemo`, `dmwappushservice`; `SysMain` — tylko na SSD
- Wyłączenie zbędnych harmonogramów systemowych (CEIP, feedback, Xbox, dysk, WER)
- Blokada procesów **przeglądarek w tle** (Chrome, Edge, Brave, Opera, Firefox, Adobe)
- **Blokowanie automatycznych aktualizacji Microsoft Office** — wyłączenie `enableautomaticupdates`, zatrzymanie i wyłączenie `ClickToRunSvc`
- **Blokowanie automatycznych aktualizacji Microsoft Store** — `AutoDownload=2`, `AutoUpdateFrequencyEnabled=0`, wyłączenie harmonogramów `WindowsUpdate\Automatic App Update`

### 🌡️ Pomiar temperatury sprzętu
- Automatyczne pobranie, uruchomienie i usunięcie **OpenHardwareMonitor** (z retry przy usuwaniu)
- Odczyt temperatury **CPU** i **GPU** przez WMI
- Wyniki w raporcie; jeśli brak internetu lub OHM nie zadziała — wartości oznaczane jako `niedostępne`

### 📊 Historia optymalizacji
- Po każdym uruchomieniu wyświetlana **tabela ostatnich 20 sesji** czyszczenia
- Dla każdej sesji: data, czas trwania, zwolnione MB i proporcjonalny pasek wizualny `#`
- Podsumowanie: łączna liczba sesji, łączne zwolnione MB, średnia MB na sesję

### 🖥️ Interfejs i UX
- Automatyczne **odblokowanie skryptu** (`Unblock-File`) po pobraniu z internetu
- Kolorowy, czytelny interfejs konsolowy (ANSI: aqua / biały / żółty)
- **Ponumerowane kroki** w separatorach: `[01/31]` do `[31/31]` — wiadomo dokładnie ile zostało
- Nagłówek z linkiem do GitHub widoczny od pierwszego uruchomienia
- **Pozycja okna konsoli skalowana do DPI monitora** przez `System.Drawing`
- Dynamiczne dopasowanie rozmiaru okna i **bufor konsoli (280 linii)**
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

> Po pierwszym uruchomieniu skrypt instaluje się w Menu Start — kolejne uruchomienia możliwe są bezpośrednio stamtąd.

> ⚠️ **Uwaga:** Skrypt **musi** być uruchomiony jako administrator. Bez uprawnień administratora wyświetli czytelny komunikat i zamknie się po 10 sekundach.

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
- listę wszystkich 31 operacji ze statusem: `[ OK ]`, `[ SKIP ]`, `[ BŁĄD ]`
- nazwę komputera i użytkownika

---

## 🗂️ Wykonywane operacje — 31 kroków

| Krok | Operacja | Opis |
|:----:|----------|------|
| 01 | Punkt przywracania systemu | Tworzy przed jakimikolwiek zmianami (z weryfikacją) |
| 02 | Windows Defender | Aktualizacja sygnatur + szybki skan (z kodem błędu) |
| 03 | AdwCleaner | Skan i czyszczenie adware; `SKIP` jeśli brak internetu |
| 04 | Zarządzanie pamięcią | DisablePagingExecutive, cache L2/L3, PageFile |
| 05 | Analiza dysku | Wykrywanie SSD/HDD, hibernacja (tylko SSD) |
| 06 | Plan zasilania | PC/laptop, Win10/Win11, podłączony/bateria |
| 07 | PageFile + Fast Startup | Rozmiar pagefile wg RAM, wyłączenie fast boot |
| 08 | Pliki tymczasowe + Prefetch | `%TEMP%`, `C:\Windows\Temp`; Prefetch tylko HDD |
| 09 | DNS / Sieć | Flush, reset TCP/IP, Cloudflare + Google DNS |
| 10 | Oczyszczanie dysku | cleanmgr z 34 kategoriami; `SKIP` jeśli niedostępny |
| 11 | CHKDSK | Planowanie na restart; `SKIP` jeśli już zaplanowany |
| 12 | Usługi systemowe | SysMain (SSD), DiagTrack, WSearch, MapsBroker, Fax, RetailDemo |
| 13 | HPET / Timer Resolution | useplatformclock=false, tscsync=enhanced, dynamictick=off |
| 14 | Narrator / Ease of Access | Wyłączenie Narratora, StickyKeys, ToggleKeys, FilterKeys |
| 15 | Blokada autoaktualizacji | Przeglądarki, Office, Microsoft Store — procesy tła |
| 16 | Cache przeglądarek | Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox, LibreWolf, Zen Browser, Floorp, Thunderbird — dla każdego konta |
| 17 | Cache Microsoft Teams | Klasyczny + nowy UWP (MSTeams_*) |
| 18 | Cache OneDrive | Logi, setup/logs, .deadLetterQueue |
| 19 | Kolejka drukarki | Czyści spooler; `SKIP` jeśli pusta |
| 20 | Stare sterowniki drukarek | Usuwa stare foldery W32X86/x64/ia64 (zachowuje folder `3`) |
| 21 | Stare sterowniki PnP | `pnputil /delete-driver` dla starszych wersji; `SKIP` jeśli brak |
| 22 | Microsoft Store cache | Czyści cache AppX + klucze rejestru Store |
| 23 | Delivery Optimization | DODownloadMode=0, restart DoSvc |
| 24 | SFC (wstępny) | Sprawdza integralność plików — pasek postępu + kod błędu |
| 25 | DISM RestoreHealth | Naprawia obraz systemu — pasek postępu + kod błędu |
| 26 | SFC (końcowy) | Ponowna weryfikacja po DISM — pasek postępu + kod błędu |
| 27 | DISM StartComponentCleanup | Czyści stare składniki aktualizacji |
| 28 | Logi CBS + Windows Update | Usuwa CBS.log + pełny reset SoftwareDistribution/catroot2 |
| 29 | Windows.old | `takeown` + usunięcie; `SKIP` jeśli nie istnieje |
| 30 | Defragmentacja / TRIM | `defrag /O` — zależnie od wykrytego typu dysku |
| 31 | Thumbnail Cache + WMI | Cache miniatur, Eksplorator, naprawa WMI |

---

## ⚠️ Ważne uwagi

- Skrypt **planuje CHKDSK** na następny restart (tylko jeśli nie był już zaplanowany) — wymagane ponowne uruchomienie
- Wyłączona jest **hibernacja** — ale tylko na SSD/NVMe; na HDD pozostaje aktywna
- Usługa **Windows Search (WSearch)** zostaje wyłączona — może wpłynąć na wyszukiwanie w menu Start
- Usługa **SysMain (Superfetch)** wyłączana jest tylko na SSD; na HDD pozostaje aktywna
- Folder **Prefetch** jest czyszczony tylko na HDD — na SSD Windows zarządza nim automatycznie
- Tweaki **HPET/Timer Resolution** modyfikują ustawienia bootloadera (`bcdedit`) — zmiany wymagają restartu
- Wyłączenie **Narratora** realizowane jest przez IFEO (przekierowanie procesu) — można cofnąć ręcznie w rejestrze
- Czyszczenie **sterowników PnP** usuwa wyłącznie starsze wersje; najnowszy sterownik każdego urządzenia zawsze zostaje zachowany
- **Delivery Optimization** zostaje wyłączone w trybie P2P — Windows Update działa normalnie, ale nie udostępnia danych innym komputerom
- Blokowanie **aktualizacji Office** (`ClickToRunSvc`) — aktualizacje Office można uruchomić ręcznie z poziomu aplikacji
- Skrypt usuwa wybrane **wbudowane aplikacje** Windows: Solitaire, Bing News/Finance/Sports/Weather, People, Skype, Office Hub, 3D Builder, Get Started, Get Help, Feedback Hub, Mapy, Mixed Reality Portal, Power Automate, Quick Assist, Clipchamp, Family Features, Microsoft To Do
- Wyłączane są wpisy autostartu popularnych aplikacji (Spotify, Discord, Steam, Epic, Teams itd.) — można je ponownie włączyć ręcznie
- Folder **Windows.old** jest usuwany trwale — nie ma możliwości cofnięcia do poprzedniej wersji Windows przez ustawienia systemowe

---

## 🔄 Co nowego w v1.17?

- ✅ **Ponumerowane kroki** — każdy separator pokazuje numer bieżącego kroku `[01/31]` do `[31/31]`; 31 kroków łącznie; wiadomo dokładnie ile zostało do końca
- ✅ **Rozszerzona lista usuniętych aplikacji** — dodane: Get Help, Feedback Hub, Mapy, Bing News/Weather/Finance/Sports, Mixed Reality Portal, Power Automate Desktop, Quick Assist, Clipchamp, Family Features, Microsoft To Do, Microsoft Solitaire Collection; usunięty Zune (niedostępny w nowszych wersjach Windows)
- ✅ **Blokowanie automatycznych aktualizacji Microsoft Office** — wyłączenie `enableautomaticupdates` i `hideenabledisableupdates` w rejestrze + zatrzymanie i wyłączenie usługi `ClickToRunSvc`
- ✅ **Blokowanie automatycznych aktualizacji Microsoft Store** — `AutoDownload=2`, `AutoUpdateFrequencyEnabled=0`, wyłączenie harmonogramów `Automatic App Update` i `Scheduled Start`
- ✅ **Naprawa Delivery Optimization** — nowa sekcja: `DODownloadMode=0` wyłącza tryb P2P (aktualizacje nie są rozsyłane do innych komputerów w sieci ani internecie), restart `DoSvc`
- ✅ **Niezawodny mechanizm aktualizacji** — podmiana pliku przez zewnętrzny `updater_helper.bat` uruchamiany jako zminimalizowane okno (`start /min`), co eliminuje błąd blokady pliku gdy stary proces skryptu jeszcze działa
- ✅ **Bufor konsoli zwiększony do 280 linii**
- ✅ **Historia optymalizacji — inline PS** — zamiast zakodowanego Base64, historia to czytelna jednolinijkowa komenda PowerShell; wyświetla ostatnie 20 sesji z tabeli plików raportów

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

**Autor:** Mag | **Wersja:** 1.17 (6/05/2026)
