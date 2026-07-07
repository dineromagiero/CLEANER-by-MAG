# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.25-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do kompleksowej optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od skanowania antywirusowego i usuwania śmieciowych plików, przez naprawę plików systemowych, aż po konfigurację sieci i rejestru. Skrypt wykonuje **31 ponumerowanych kroków**, inteligentnie dostosowuje się do sprzętu i systemu, wyświetla graficzny pasek postępu i **zapobiega usypianiu komputera** podczas całego procesu.

---

## ✨ Co robi skrypt?
https://github.com/user-attachments/assets/022caddb-17e5-4845-84eb-8595fd1cbd78

### 🔄 Automatyczna aktualizacja i instalacja
- Przy starcie sprawdza dostępność nowej wersji przez **GitHub API**
- Jeśli dostępna aktualizacja — pobiera plik `.bat` przez dedykowany skrypt pomocniczy uruchamiany w tle (`updater_helper.bat`), eliminując konflikt blokady pliku podczas podmiany
- Przy pierwszym uruchomieniu **instaluje się w Menu Start** ze skrótem opatrzonym własną ikoną (`icon.ico` wypakowywaną z pliku `.bat`)
- Tworzy **skrót do folderu raportów** i **skrót deinstalatora** w Menu Start (oba ze własną ikoną)
- Tworzy dedykowany **folder w Harmonogramie zadań** (`\CLEANER by MAG\`)

### 🗑️ Deinstalacja
- Skrót **„Odinstaluj CLEANER by MAG"** tworzony automatycznie w Menu Start przy każdym uruchomieniu
- Deinstalator otwiera własne okno z nagłówkiem, pyta o potwierdzenie oraz opcjonalne usunięcie raportów
- Usuwa folder programu z Menu Start, klucz konsoli z rejestru, folder w Harmonogramie zadań oraz harmonogramy AdwCleaner i Malwarebytes

### 🔒 Bezpieczeństwo i antywirusy
- Tworzy **punkt przywracania systemu** przed wprowadzeniem jakichkolwiek zmian (z weryfikacją powodzenia)
- Aktualizuje i uruchamia **szybki skan Windows Defender** z raportowaniem kodu błędu
- Pobiera, uruchamia i automatycznie usuwa **AdwCleaner** (skan + czyszczenie adware + usunięcie jego harmonogramów); brak internetu oznacza `[ SKIP ]`
- Opcjonalne pobranie, skan i deinstalacja **Malwarebytes** po zakończeniu (ciche + usunięcie harmonogramów)

### 🗑️ Czyszczenie plików
- Pliki tymczasowe użytkownika i systemu (`%TEMP%`, `C:\Windows\Temp`)
- Folder **Downloaded Program Files**
- **Folder Prefetch** — czyszczony tylko na HDD; na SSD zarządzany automatycznie przez system (`[ SKIP ]`)
- **Minidumpy** i raporty błędów Windows (WER)
- Cache **ikon** i czcionek
- **Logi instalatorów systemowych**: CBS, DISM, MeasuredBoot, NetSetup, waasmedic, INF (`*.log`, `*.pnf`)
- Cache przeglądarek — **dla każdego konta użytkownika na komputerze** (pętla po `C:\Users\*`, z pominięciem kont systemowych): **Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox, LibreWolf, Zen Browser, Floorp, Thunderbird**
- Cache **Microsoft Teams** — obsługa obu wersji: klasycznej i nowej UWP (`MSTeams_*\LocalCache`)
- Cache **Microsoft OneDrive** — logi, setup/logs, `.deadLetterQueue`
- Cache **Java** (`AppData\LocalLow\Sun\Java\Deployment\cache`)
- Pliki tymczasowe **ASP.NET** (`Temporary ASP.NET Files`)
- Cache **Microsoft Store** (LocalCache, INetCache, Temp dla wszystkich pakietów AppX; z czyszczeniem kluczy rejestru Store)
- Kolejka **drukarki (Spooler)** + stare sterowniki drukarek (W32X86, x64, ia64; folder `3` zachowany); `[ SKIP ]` jeśli kolejka pusta
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
- Raport: liczba zachowanych / usuniętych / błędów; `[ SKIP ]` jeśli brak przestarzałych

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
- **PageFile** — ustawiany na 8192/8192 MB (≤ 8 GB RAM) lub 4096/4096 MB (> 8 GB RAM) **wyłącznie jeśli dotychczas był zarządzany automatycznie**; jeśli użytkownik ręcznie ustawił własne wartości — skrypt je zachowuje
- Tweaki rejestru: priorytety **procesora**, zarządzanie **pamięcią**, animacje UI, aktywne godziny Windows Update (8:00–23:00)
- **Defragmentacja** (HDD) lub **TRIM** (SSD) przez `defrag /O`
- **Wyłączenie harmonogramu automatycznej defragmentacji** (`ScheduledDefrag`) — usługa `defragsvc` pozostaje dostępna dla DISM

### 🧩 Wyłączanie zbędnych funkcji Windows
- Automatyczne wyłączenie przez DISM z paskiem postępu, bez restartu:
  - **Usługi XPS** (`Printing-XPSServices-Features`)
  - **Klasyczny Windows Media Player** (`WindowsMediaPlayer`)
  - **Klient Folderów Roboczych** (`WorkFolders-Client`)
  - **Drukuj do PDF** (`Printing-PrintToPDFServices-Features`)
  - **Protokół SMB 1.0** (`SMB1Protocol`) — przestarzały protokół stanowiący zagrożenie bezpieczeństwa

### 🌐 Blokowanie procesów i autoaktualizacji przeglądarek
- Zamknięcie procesów Chrome, Edge, Brave, Opera, Firefox
- Wyłączenie **trybu działania w tle** przeglądarek przez polityki rejestru (Chrome, Edge, Brave, Opera)
- Wyłączenie **usług autoaktualizacji**: `gupdate`, `gupdatem`, `MicrosoftEdgeElevationService`, `edgeupdate`, `MozillaMaintenance`
- Wyłączenie **harmonogramów aktualizacji**: Edge, Google Chrome, Opera, Adobe

### 📴 Blokowanie dostępu w tle dla aplikacji UWP
- Wyłączenie aktywności w tle dla 10 wbudowanych aplikacji UWP przez klucz `BackgroundAccessApplications` w rejestrze:
  **Zdjęcia, Aparat, Kalkulator, Notatnik, Paint, Mapy, Pogoda, Filmy i TV, Groove Music, Microsoft Store**
- Aplikacje pozostają dostępne do ręcznego uruchomienia

### ⏱️ Optymalizacja latencji (HPET / Timer Resolution)
- Wyłączenie **platform clock** (`bcdedit /set useplatformclock false`)
- Ustawienie **TSC sync policy** na `enhanced`
- Wyłączenie **Dynamic Tick** (`disabledynamictick yes`)
- Aktywacja **GlobalTimerResolutionRequests** w rejestrze

### 🔕 Narrator, OneNote i ułatwienia dostępu
- Wyłączenie **Narratora** przez IFEO i klucze polityk
- Blokada autostartu **OneNote** (`ONENOTEM.EXE`) przez IFEO oraz usunięcie wpisów `Run` z rejestru (HKCU i HKLM, x64 i WOW6432Node)
- Wyłączenie **StickyKeys, ToggleKeys, FilterKeys**
- Wyciszenie **dźwięków dostępności** i wyłączenie autostartu Narratora przy logowaniu

### 🌐 Sieć i Delivery Optimization
- **Reset proxy** systemowego (WinHTTP i rejestr IE)
- Flush i rejestracja **DNS**
- Reset **stosu TCP/IP** i Winsock
- Ustawienie serwerów DNS: **Cloudflare (1.1.1.1 / 1.0.0.1)** i **Google (8.8.8.8)** dla IPv4 i IPv6 — dla każdego aktywnego interfejsu sieciowego
- Optymalizacja parametrów **TCP** (autotuninglevel, RSS, chimney)
- **Wyłączenie trybu P2P Delivery Optimization** (`DODownloadMode=0`)

### 🛡️ Prywatność i telemetria
- Wyłączenie zbierania danych diagnostycznych (**telemetria**)
- Wyłączenie **Cortany**
- Wyłączenie **Bing Search** w menu Start
- Wyłączenie śledzenia wpisów i kolekcji danych wejściowych (InputPersonalization)
- Wyłączenie sugestii i powiadomień systemowych (ContentDeliveryManager)
- Wyłączenie raportowania błędów Windows (WER)

### ⚙️ Usługi systemowe i harmonogramy
- Dezaktywacja zbędnych usług: `DiagTrack`, `WSearch`, `MapsBroker`, `Fax`, `RetailDemo`, `dmwappushservice`; `SysMain` — tylko na SSD
- Wyłączenie zbędnych harmonogramów systemowych (CEIP, feedback, Xbox, dysk, WER, ScheduledDefrag)
- **Blokowanie automatycznych aktualizacji Microsoft Office** — wyłączenie `enableautomaticupdates`, zatrzymanie `ClickToRunSvc`
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
- **Własna ikona** (`icon.ico`) wypakowywana z pliku `.bat` — widoczna w skrótach Menu Start
- **Graficzny pasek postępu** (overlay PS1) uruchamiany w tle i aktualizowany przy każdym z 31 kroków
- **Odliczanie 10 sekund** przed sekcją autostartu — czas na zamknięcie otwartych przeglądarek
- **Zapobieganie usypianiu** — blokuje uśpienie systemu i wygaszacz ekranu na czas działania; oryginalne ustawienia przywracane automatycznie po zakończeniu
- **Ponumerowane kroki** w separatorach: `[01/31]` do `[31/31]`
- Nagłówek z linkiem do GitHub widoczny od pierwszego uruchomienia
- **Pozycja okna konsoli skalowana do DPI monitora** przez `System.Drawing`
- Dynamiczne dopasowanie rozmiaru okna i **bufor konsoli (280 linii)**
- Trójstanowy system statusów: `[ OK ]` / `[SKIP]` / `[BŁĄD]`
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

> Po pierwszym uruchomieniu skrypt instaluje się w Menu Start — kolejne uruchomienia oraz deinstalacja możliwe są bezpośrednio stamtąd.

> ⚠️ **Uwaga:** Skrypt **musi** być uruchomiony jako administrator. Bez uprawnień administratora wyświetli czytelny komunikat i zamknie się po 10 sekundach.

## 🗑️ Deinstalacja

Otwórz Menu Start → **CLEANER by MAG** → kliknij prawym przyciskiem **„Odinstaluj CLEANER by MAG"** → „Uruchom jako administrator". Deinstalator zapyta o potwierdzenie i opcjonalne usunięcie raportów.

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
- rozmiar kolejki drukarki przed czyszczeniem
- listę wszystkich 31 operacji ze statusem: `[ OK ]`, `[ SKIP ]`, `[ BŁĄD ]`
- nazwę komputera i użytkownika

---

## 🗂️ Wykonywane operacje — 31 kroków

| Krok | Operacja | Opis |
|:----:|----------|------|
| 01 | Punkt przywracania systemu | Tworzy przed jakimikolwiek zmianami (z weryfikacją) |
| 02 | Windows Defender | Aktualizacja sygnatur + szybki skan (z kodem błędu) |
| 03 | AdwCleaner | Skan i czyszczenie adware + usunięcie harmonogramów; `SKIP` jeśli brak internetu |
| 04 | Zarządzanie pamięcią | DisablePagingExecutive (≥8 GB), cache L2/L3, proxy reset |
| 05 | Analiza dysku | Wykrywanie SSD/HDD, hibernacja (tylko SSD) |
| 06 | Plan zasilania | PC/laptop, Win10/Win11, podłączony/bateria |
| 07 | PageFile + Fast Startup | PageFile wg RAM (tylko jeśli auto), wyłączenie fast boot |
| 08 | Pliki tymczasowe + Prefetch + Minidump + WER + logi | `%TEMP%`, `C:\Windows\Temp`; Prefetch tylko HDD; minidumpy; WER; logi instalatorów |
| 09 | DNS / Sieć | Flush, reset TCP/IP + Winsock, Cloudflare + Google DNS, TCP tweaki |
| 10 | Oczyszczanie dysku | cleanmgr z 34 kategoriami; `SKIP` jeśli niedostępny |
| 11 | CHKDSK | Planowanie na restart; `SKIP` jeśli już zaplanowany |
| 12 | Usługi systemowe | SysMain (SSD), DiagTrack, WSearch, MapsBroker, Fax, RetailDemo |
| 13 | HPET / Timer Resolution | useplatformclock=false, tscsync=enhanced, dynamictick=off |
| 14 | Optymalizacja rejestru + Narrator | Animacje, telemetria, Cortana, WER; wyłączenie Narratora, StickyKeys, ToggleKeys, FilterKeys |
| 15 | Blokada autoaktualizacji i autostartu | Przeglądarki (tło + usługi + harmonogramy), Office, Store, OneNote; wyłączanie funkcji Windows (XPS, WMP, WorkFolders, PDF, SMB 1.0); blokada tła 10 aplikacji UWP; wyłączenie harmonogramu defragmentacji |
| 16 | Cache przeglądarek | Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox, LibreWolf, Zen Browser, Floorp, Thunderbird — dla każdego konta |
| 17 | Cache Microsoft Teams | Klasyczny + nowy UWP (MSTeams_*) |
| 18 | Cache OneDrive | Logi, setup/logs, .deadLetterQueue |
| 19 | Java + .NET + Spooler | Java Deployment Cache, ASP.NET Files; kolejka drukarki; `SKIP` jeśli pusta |
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
| 31 | Thumbnail Cache + WMI | Cache miniatur, restartuje Eksplorator, naprawa WMI |

---

## ⚠️ Ważne uwagi

- Skrypt **planuje CHKDSK** na następny restart (tylko jeśli nie był już zaplanowany) — wymagane ponowne uruchomienie
- Wyłączona jest **hibernacja** — ale tylko na SSD/NVMe; na HDD pozostaje aktywna
- Usługa **Windows Search (WSearch)** zostaje wyłączona — może wpłynąć na wyszukiwanie w menu Start
- Usługa **SysMain (Superfetch)** wyłączana jest tylko na SSD; na HDD pozostaje aktywna
- Folder **Prefetch** jest czyszczony tylko na HDD — na SSD Windows zarządza nim automatycznie
- **PageFile** jest zmieniany tylko wtedy, gdy był ustawiony na automatyczny; ręczne ustawienia użytkownika są zachowywane bez zmian
- Tweaki **HPET/Timer Resolution** modyfikują ustawienia bootloadera (`bcdedit`) — zmiany wymagają restartu
- Wyłączenie **Narratora** realizowane jest przez IFEO — można cofnąć ręcznie w rejestrze
- Blokada **OneNote** (`ONENOTEM.EXE`) uniemożliwia uruchomienie procesu szybkich notatek; główna aplikacja OneNote działa normalnie
- Wyłączony jest **harmonogram automatycznej defragmentacji** — usługa `defragsvc` pozostaje dostępna (wymagana przez DISM)
- **Blokada tła aplikacji UWP** nie usuwa aplikacji — można je nadal uruchamiać ręcznie
- Wyłączenie **SMB 1.0** jest zalecane ze względów bezpieczeństwa; może wpłynąć na komunikację ze starszymi urządzeniami sieciowymi
- Czyszczenie **sterowników PnP** usuwa wyłącznie starsze wersje; najnowszy sterownik każdego urządzenia zawsze zostaje zachowany
- **Delivery Optimization** zostaje wyłączone w trybie P2P — Windows Update działa normalnie
- Blokowanie **aktualizacji Office** — aktualizacje można uruchomić ręcznie z poziomu aplikacji
- Skrypt usuwa wybrane **wbudowane aplikacje** Windows: Solitaire, Bing News/Finance/Sports/Weather, People, Skype, Office Hub, 3D Builder, Get Started, Get Help, Feedback Hub, Mapy, Mixed Reality Portal, Power Automate, Quick Assist, Clipchamp, Family Features, Microsoft To Do, Sticky Notes
- Folder **Windows.old** jest usuwany trwale — nie ma możliwości cofnięcia do poprzedniej wersji Windows przez ustawienia systemowe

---

## 🔄 Co nowego w v1.25?

- ✅ **Graficzny pasek postępu (overlay)** — skrypt wypakowuje i uruchamia w tle plik `cleaner_mag_overlay.ps1`; każdy z 31 kroków aktualizuje plik `cleaner_mag_overlay_step.dat`, a overlay odczytuje go i wyświetla aktualny postęp
- ✅ **Zapobieganie usypianiu systemu** — nowy moduł PowerShell uruchamiany w tle przy starcie: blokuje uśpienie i wygaszacz ekranu przez cały czas działania; zapisuje oryginalne ustawienia i przywraca je po zakończeniu lub gdy skrypt zostanie zamknięty
- ✅ **Własna ikona** — plik `icon.ico` wypakowywany z sekcji Base64 w pliku `.bat`; skróty w Menu Start używają tej ikony
- ✅ **Odliczanie 10 sekund** przed sekcją autostartu (krok 15) — czas na ręczne zamknięcie otwartych przeglądarek przed ich wymuszoną terminacją
- ✅ **Zachowanie ręcznych ustawień PageFile** — jeśli użytkownik wcześniej ręcznie skonfigurował plik stronicowania, skrypt go nie nadpisuje i wyświetla informację `[ SKIP ]`; oryginalne wartości zapisywane są do pliku `.dat` przy pierwszym uruchomieniu
- ✅ **Deinstalator z własnym oknem** — tryb `UNINSTALL` otwiera dedykowane okno konsoli z własnym tytułem i kolorami
- ✅ **Skrót „Raporty"** — zmiana nazwy z `Raporty (Dokumenty).lnk` na `Raporty.lnk` dla zwięzłości w Menu Start

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

Autor: **MAG** | Wersja: **1.25** (08/07/2026)
