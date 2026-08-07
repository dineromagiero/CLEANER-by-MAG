# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.29-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do kompleksowej optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od skanowania antywirusowego i usuwania śmieciowych plików, przez naprawę plików systemowych, aż po konfigurację sieci i rejestru. Skrypt wykonuje **31 ponumerowanych kroków**, inteligentnie dostosowuje się do sprzętu i systemu, a przy deinstalacji może **w pełni przywrócić stan systemu sprzed pierwszego uruchomienia**.

---

## ✨ Co robi skrypt?
https://github.com/user-attachments/assets/548bb1f3-e38e-426b-b3fc-3f6916860fcd

### 🔄 Automatyczna aktualizacja i instalacja
- Przy starcie sprawdza dostępność nowej wersji przez **GitHub API** — porównanie przez `[version]::Parse()`, obsługuje wszystkie przypadki: nowsza, taka sama, starsza
- Pytanie o aktualizację ma **20-sekundowy timeout** z domyślnym `N`; cały blok pytania usuwany z konsoli po odpowiedzi
- Jeśli dostępna aktualizacja — pobiera plik `.bat` przez dedykowany skrypt pomocniczy w tle (`updater_helper.bat`)
- Przy pierwszym uruchomieniu **instaluje się w Menu Start** ze skrótem opatrzonym własną ikoną (`icon.ico` wypakowywaną z pliku `.bat`)
- Tworzy **skrót do folderu raportów** i **skrót deinstalatora** w Menu Start
- Tworzy dedykowany **folder w Harmonogramie zadań** (`\CLEANER by MAG\`)
- Devlog odświeżany przy każdym uruchomieniu

### 🗑️ Deinstalacja z przywracaniem stanu systemu
- Skrót **„Odinstaluj CLEANER by MAG"** tworzony automatycznie w Menu Start
- Deinstalator zadaje trzy pytania: potwierdzenie odinstalowania, usunięcie raportów, **przywrócenie oryginalnych ustawień systemowych**
- Przy wyborze przywracania: skrypt uruchamia `cleaner_mag_state.ps1 -Mode Revert`, który odczytuje zapisany JSON i cofa zmiany w DNS, usługach, rejestrze, HPET i planie zasilania do stanu sprzed pierwszego uruchomienia
- Jeśli plik stanu nie istnieje (skrypt nigdy nie wykonał pełnego przebiegu) — informacja z pominięciem

### 🔒 Bezpieczeństwo i antywirusy
- Tworzy **punkt przywracania systemu** przed wprowadzeniem jakichkolwiek zmian (z weryfikacją)
- Aktualizuje i uruchamia **szybki skan Windows Defender** z wykrywaniem dostępności przez pomiar czasu trwania (skan < 4 sekundy = Defender niedostępny)
- **AdwCleaner z 30-dniowym cooldown** — plik zapisywany w `Tools\AdwCleaner.exe`; przy kolejnym uruchomieniu w ciągu 30 dni używana jest lokalna kopia bez pobierania; skan + czyszczenie + usunięcie harmonogramów
- Opcjonalne pobranie, skan i deinstalacja **Malwarebytes** po zakończeniu (ciche + usunięcie harmonogramów + weryfikacja poprawności deinstalacji)
- Opcjonalne pobranie i uruchomienie **KVRT (Kaspersky Virus Removal Tool)** — pełne, automatyczne skanowanie w trybie cichym; skrypt blokuje niechciany restart systemu przez KVRT; data ostatniego skanu wyświetlana przy pytaniu; po zakończeniu KVRT i jego dane usuwane automatycznie

### 🗑️ Czyszczenie plików
- Pliki tymczasowe użytkownika i systemu (`%TEMP%`, `C:\Windows\Temp`) — czyszczone dwukrotnie (na starcie i na końcu)
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
- **CHKDSK** — planowany na następny restart z **30-dniowym cooldown**; `[ SKIP ]` jeśli zaplanowany w ciągu ostatnich 30 dni
- **Inteligentna naprawa WMI** — weryfikacja → Salvage → Reset z rejestracją MOF/MFL; `[ SKIP ]` jeśli baza jest zdrowa
- Pełny reset **Windows Update** (zatrzymanie usług, usunięcie SoftwareDistribution i catroot2 z fallbackiem, rejestracja DLL)

### 🗂️ Czyszczenie przestarzałych sterowników PnP
- Automatyczne wykrycie wszystkich zainstalowanych sterowników `oem*.inf` przez `Win32_PnPSignedDriver`
- Grupowanie według urządzenia i producenta, sortowanie według numeru wersji
- Usunięcie przez `pnputil /delete-driver` wszystkich starszych wersji — **najnowsza zawsze zachowana**
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
- **DisablePagingExecutive** — włączany przy ≥ 8000 MB RAM; poniżej progu jawnie wyłączany
- Rozszerzone tweaki zarządzania pamięcią: PAE, SystemPages, NtfsMemoryUsage, LargeSystemCache, ClearPageFileAtShutdown, cache L2/L3, priorytety procesora
- **PageFile** — ustawiany wg RAM wyłącznie jeśli był zarządzany automatycznie; ręczne ustawienia użytkownika zachowywane; oryginalne wartości zapisywane do pliku
- Tweaki rejestru: animacje UI, aktywne godziny Windows Update (8:00–23:00)
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
- Wyłączenie **trybu działania w tle** przez polityki rejestru (Chrome, Edge, Brave, Opera)
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
- Blokada autostartu **OneNote** (`ONENOTEM.EXE`) przez IFEO oraz usunięcie wpisów `Run` z rejestru
- Wyłączenie **StickyKeys, ToggleKeys, FilterKeys**
- Wyciszenie **dźwięków dostępności** i wyłączenie autostartu Narratora przy logowaniu

### 🌐 Sieć i Delivery Optimization
- **Reset proxy** systemowego (WinHTTP i rejestr IE)
- Flush i rejestracja **DNS**
- Reset **stosu TCP/IP** i Winsock
- Ustawienie serwerów DNS dla każdego aktywnego interfejsu: **Cloudflare 1.1.1.1** (primary), **Cloudflare 1.0.0.1** (secondary), **Google 8.8.8.8** (fallback) dla IPv4 i IPv6
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
- **Blokowanie automatycznych aktualizacji Microsoft Store** — `AutoDownload=2`, `AutoUpdateFrequencyEnabled=0`, wyłączenie harmonogramów

### 🌡️ Pomiar temperatury sprzętu
- Automatyczne pobranie, uruchomienie i usunięcie **OpenHardwareMonitor** (z retry przy usuwaniu)
- Odczyt temperatury **CPU** i **GPU** przez WMI; adaptery Microsoft pomijane przy wykrywaniu GPU
- Wyniki w raporcie; jeśli brak internetu lub OHM nie zadziała — wartości oznaczane jako `niedostępne`

### 📊 Historia optymalizacji
- Po każdym uruchomieniu wyświetlana **tabela ostatnich 20 sesji** czyszczenia
- Dla każdej sesji: data, czas trwania, zwolnione MB i proporcjonalny pasek wizualny `#`
- Podsumowanie: łączna liczba sesji, łączne zwolnione MB, średnia MB na sesję

### 🖥️ Interfejs i UX
- Automatyczne **odblokowanie skryptu** (`Unblock-File`) po pobraniu z internetu
- **Ekran potwierdzenia przed startem** — 60-sekundowy timeout z domyślnym `T`
- Kolorowy, czytelny interfejs konsolowy (ANSI: aqua / biały / żółty)
- **Czyszczenie linii ANSI** — komunikaty nadpisywane w miejscu; konsola pozostaje czysta
- **Własna ikona** (`icon.ico`) wypakowywana z pliku `.bat` — widoczna w skrótach Menu Start
- **Graficzny pasek postępu** (overlay PS1) uruchamiany w tle i aktualizowany przy każdym z 31 kroków; drugi overlay do postępu pobierania z paskiem `[●●●○○] 60% - Pozostało: 12 s`
- **`FOCUS_CONSOLE`** — przed każdym pytaniem `[T/N]` okno konsoli jest aktywowane przez Win32 API (`SetForegroundWindow`, `keybd_event`); skompilowany DLL cachowany do ponownego użycia
- **`CHOICE_TN`** — własna implementacja wyboru klawiszem przez PowerShell `ReadKey` z opcjonalnym timeoutem i domyślnym wyborem; działa nawet gdy okno nie jest aktywne
- **Odliczanie 10 sekund** przed sekcją autostartu przeglądarek
- **Zapobieganie usypianiu** — `START_KEEPAWAKE` / `STOP_KEEPAWAKE` jako wywoływalne podprogramy; moduł czeka na potwierdzenie zamknięcia przed kontynuacją
- **Stan systemu zapisywany na starcie** — `cleaner_mag_state.ps1 -Mode Capture` zapisuje do JSON oryginalne wartości DNS, usług, rejestru, HPET i pliku stronicowania
- **Ponumerowane kroki** w separatorach: `[01/31]` do `[31/31]`
- Nagłówek z linkiem do GitHub widoczny od pierwszego uruchomienia
- **Pozycja okna konsoli skalowana do DPI monitora** przez `System.Drawing`
- Dynamiczne dopasowanie rozmiaru okna i **bufor konsoli (280 linii)**
- Trójstanowy system statusów: `[ OK ]` / `[SKIP]` / `[BŁĄD]`
- **Czas trwania** mierzony i wyświetlany w formacie `HH:MM:SS`
- Dźwiękowe **powiadomienie o zakończeniu** optymalizacji
- Interaktywne pytania na koniec: KVRT, Malwarebytes, wsparcie projektu, restart
- Raport zapisywany w **Dokumentach** z unikalną nazwą zawierającą datę i czas

---

## 🚀 Wymagania

| Wymaganie | Szczegóły |
|-----------|-----------|
| System | Windows 10 / Windows 11 |
| Uprawnienia | **Administrator** (wymagane) |
| Połączenie | Opcjonalne — potrzebne dla aktualizacji, AdwCleaner, Malwarebytes, KVRT, OHM |
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

Otwórz Menu Start → **CLEANER by MAG** → kliknij prawym przyciskiem **„Odinstaluj CLEANER by MAG"** → „Uruchom jako administrator". Deinstalator zapyta kolejno: o potwierdzenie, o usunięcie raportów i o **przywrócenie oryginalnych ustawień systemowych sprzed instalacji**.

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
| 02 | Windows Defender | Aktualizacja sygnatur + szybki skan (z wykrywaniem dostępności przez czas) |
| 03 | AdwCleaner | Skan i czyszczenie adware; kopia lokalna ważna 30 dni; `SKIP` jeśli brak internetu i brak kopii |
| 04 | Proxy + harmonogramy + pamięć | Reset proxy, wyłączenie harmonogramów CEIP/feedback/Xbox, DisablePagingExecutive, PAE, SystemPages, NtfsMemoryUsage, LargeSystemCache, ClearPageFileAtShutdown, cache L2/L3, priorytety CPU |
| 05 | Analiza dysku | Wykrywanie SSD/HDD, hibernacja (tylko SSD) |
| 06 | Plan zasilania | PC/laptop, Win10/Win11, podłączony/bateria |
| 07 | PageFile + Fast Startup | PageFile wg RAM (tylko jeśli auto), wyłączenie fast boot |
| 08 | Pliki tymczasowe + Prefetch + Minidump + WER + logi | `%TEMP%`, `C:\Windows\Temp`; Prefetch tylko HDD; minidumpy; WER; logi instalatorów |
| 09 | DNS | Flush, reset TCP/IP + Winsock, Cloudflare 1.1.1.1/1.0.0.1 + Google 8.8.8.8 dla IPv4 i IPv6 |
| 10 | TCP + Event Log + cleanmgr | TCP tweaki; czyszczenie Event Viewer; konfiguracja i uruchomienie cleanmgr |
| 11 | CHKDSK | Planowanie na restart z 30-dniowym cooldown; `SKIP` jeśli zaplanowany zbyt niedawno |
| 12 | Usługi systemowe | SysMain (SSD), DiagTrack, WSearch, MapsBroker, Fax, RetailDemo |
| 13 | HPET / Timer Resolution | useplatformclock=false, tscsync=enhanced, dynamictick=off |
| 14 | Optymalizacja rejestru + Narrator + dmwappushservice | Animacje, telemetria, Cortana, WER; wyłączenie Narratora, StickyKeys, ToggleKeys, FilterKeys; dmwappushservice |
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

- **Ekran potwierdzenia na starcie** — skrypt informuje o wszystkich ingerencjach wymagających świadomej zgody zanim cokolwiek wykona
- Skrypt **planuje CHKDSK** maksymalnie raz na 30 dni
- **KVRT** — skanowanie może trwać kilkanaście minut; skrypt blokuje ewentualny automatyczny restart przez KVRT (`shutdown /a`); **zapisz pracę przed rozpoczęciem**
- Wyłączona jest **hibernacja** — ale tylko na SSD/NVMe; na HDD pozostaje aktywna
- Usługa **Windows Search (WSearch)** zostaje wyłączona — może wpłynąć na wyszukiwanie w menu Start
- Usługa **SysMain (Superfetch)** wyłączana jest tylko na SSD; na HDD pozostaje aktywna
- Folder **Prefetch** jest czyszczony tylko na HDD
- **PageFile** jest zmieniany tylko gdy był ustawiony na automatyczny; oryginalne wartości zapisywane do pliku i odtwarzalne przy deinstalacji
- Tweaki **HPET/Timer Resolution** modyfikują ustawienia bootloadera (`bcdedit`) — zmiany wymagają restartu
- Wyłączenie **Narratora** realizowane jest przez IFEO — można cofnąć ręcznie lub przez deinstalator
- Blokada **OneNote** (`ONENOTEM.EXE`) uniemożliwia uruchomienie procesu szybkich notatek; główna aplikacja OneNote działa normalnie
- Wyłączony jest **harmonogram automatycznej defragmentacji** — usługa `defragsvc` pozostaje dostępna (wymagana przez DISM)
- **Blokada tła aplikacji UWP** nie usuwa aplikacji — można je nadal uruchamiać ręcznie
- Wyłączenie **SMB 1.0** jest zalecane ze względów bezpieczeństwa; może wpłynąć na komunikację ze starszymi urządzeniami sieciowymi
- Czyszczenie **sterowników PnP** usuwa wyłącznie starsze wersje; najnowszy sterownik każdego urządzenia zawsze zostaje zachowany
- **Delivery Optimization** zostaje wyłączone w trybie P2P — Windows Update działa normalnie
- Blokowanie **aktualizacji Office** — aktualizacje można uruchomić ręcznie z poziomu aplikacji
- Skrypt usuwa wybrane **wbudowane aplikacje** Windows: Solitaire, Bing News/Finance/Sports/Weather, People, Skype, Office Hub, 3D Builder, Get Started, Get Help, Feedback Hub, Mapy, Mixed Reality Portal, Power Automate, Quick Assist, Clipchamp, Family Features, Microsoft To Do, Sticky Notes
- Folder **Windows.old** jest usuwany trwale
- **Deinstalacja z przywracaniem stanu** wymaga aby skrypt był wcześniej co najmniej raz uruchomiony do końca — tylko wtedy plik `cleaner_mag_original_state.json` istnieje; część zmian (DISM, usunięte aplikacje) nie jest cofalana przez ten mechanizm

---

## 🔄 Co nowego w v1.29?

- ✅ **KVRT (Kaspersky Virus Removal Tool)** — nowe opcjonalne skanowanie antywirusowe po Malwarebytes: pobieranie ~170 MB z serwerów Kaspersky z paskiem postępu i szacowanym czasem, skanowanie w trybie cichym (`-silent -processlevel 3`), aktywne blokowanie niechcianego restartu przez KVRT (`shutdown /a`), zapis daty ostatniego skanu z wyświetlaniem przy pytaniu, automatyczne sprzątanie po zakończeniu
- ✅ **Przywracanie stanu systemu** — nowy moduł `cleaner_mag_state.ps1` (wypakowywany z pliku `.bat`) zapisuje w trybie `Capture` oryginalne wartości DNS, usług, rejestru i HPET do JSON przed pierwszym uruchomieniem; deinstalator w trybie `Revert` może przywrócić system do stanu sprzed instalacji skryptu
- ✅ **AdwCleaner z 30-dniowym cooldown** — plik `AdwCleaner.exe` zapisywany lokalnie w `Tools\` i reużywany przez 30 dni; eliminuje pobieranie przy każdym uruchomieniu
- ✅ **`CHOICE_TN` jako subrutyną PowerShell** — zastępuje `choice /c TN /n`; używa `ReadKey` co działa poprawnie gdy okno konsoli nie jest aktywne; obsługuje opcjonalny timeout i domyślny klawisz
- ✅ **`FOCUS_CONSOLE` przed każdym pytaniem** — wywołuje Win32 API (`SetForegroundWindow`, `keybd_event`) przez skompilowany DLL cachowany do `cleaner_mag_focus.dll`; okno konsoli wynoszone na wierzch przed każdym oczekiwaniem na wybór użytkownika
- ✅ **`START_KEEPAWAKE` / `STOP_KEEPAWAKE`** — moduł anti-sleep przerobiony na dwa wywoływalne podprogramy; `STOP_KEEPAWAKE` czeka na potwierdzenie zamknięcia procesu (max 6 sekund) zamiast tylko ustawiać flagę; moduł uruchamiany i zatrzymywany precyzyjnie wokół długich operacji (Malwarebytes, KVRT)
- ✅ **Wykrywanie dostępności Defendera przez czas trwania skanu** — skan kończący się w < 4000 ms traktowany jako nieudany (Defender niedostępny lub błąd); eliminuje fałszywe pozytywy przy braku Windows Defendera

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

Autor: **MAG** | Wersja: **1.29** (07/08/2026)
