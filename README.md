# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.09-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do kompleksowej optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od skanowania antywirusowego i usuwania śmieciowych plików, przez naprawę plików systemowych, aż po konfigurację sieci i rejestru. Skrypt **inteligentnie dostosowuje się do sprzętu** — inne decyzje podejmuje na SSD, inne na HDD, inne na laptopie, inne na PC stacjonarnym.

---

## ✨ Co robi skrypt?

### 🔒 Bezpieczeństwo i antywirusy
- Tworzy **punkt przywracania systemu** przed wprowadzeniem jakichkolwiek zmian (z weryfikacją powodzenia)
- Aktualizuje i uruchamia **szybki skan Windows Defender** z raportowaniem kodu błędu
- Pobiera, uruchamia i automatycznie usuwa **AdwCleaner** (skan + czyszczenie adware); pobieranie z limitem czasowym 30 sekund — jeśli brak internetu, operacja oznaczana jako `[ SKIP ]`
- Opcjonalne pobranie, skan i deinstalacja **Malwarebytes** po zakończeniu; brak połączenia sygnalizowany komunikatem

### 🗑️ Czyszczenie plików
- Pliki tymczasowe użytkownika i systemu (`%TEMP%`, `C:\Windows\Temp`)
- Folder **Downloaded Program Files**
- **Folder Prefetch** — czyszczony **tylko na HDD**; na SSD Windows zarządza nim automatycznie i operacja jest oznaczana jako `[ SKIP ]`
- **Minidumpy** i raporty błędów Windows (WER)
- Cache **ikon** i czcionek
- Cache przeglądarek: **Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox** (domyślny profil + wszystkie `Profile *`)
- Cache **Microsoft Teams**
- Cache **Microsoft Store** (LocalCache, INetCache, Temp dla wszystkich pakietów AppX)
- Kolejka **drukarki (Spooler)** + stare sterowniki drukarek (W32X86, x64, ia64; aktywne zachowane)
- Logi **CBS** i Windows Update (`SoftwareDistribution`, `catroot2`)
- Cache miniatur z restartem Eksploratora
- Folder **Windows.old** z weryfikacją powodzenia usunięcia; `[ SKIP ]` jeśli nie istnieje
- **Oczyszczanie dysku** (`cleanmgr`) z konfiguracją 34 kategorii przez rejestr; `[ SKIP ]` jeśli `cleanmgr` jest niedostępny w danej wersji systemu

### 🔧 Naprawa systemu
- **SFC** (System File Checker) — dwukrotnie: przed i po DISM, z paskiem postępu i kodem błędu w raporcie
- **DISM RestoreHealth** — naprawa obrazu systemu, z paskiem postępu i kodem błędu w raporcie
- **DISM StartComponentCleanup** — czyszczenie bazy składników po aktualizacjach
- **CHKDSK** zaplanowany na następny restart
- Naprawa repozytorium **WMI** z bezpieczną obsługą katalogu (`pushd/popd`)
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

### 🖥️ Interfejs i UX
- Automatyczne **odblokowanie skryptu** (`Unblock-File`) po pobraniu z internetu
- Kolorowy, czytelny interfejs konsolowy (ANSI: aqua / biały / żółty)
- Nagłówek z linkiem do GitHub widoczny od pierwszego uruchomienia
- Dynamiczne dopasowanie rozmiaru okna i **buforu konsoli (200 linii)**
- **Sprawdzanie aktualizacji** przez GitHub API przed sprawdzaniem uprawnień
- Trójstanowy system statusów: `[ OK ]` / `[SKIP]` / `[----]`
- SFC, DISM i inne długie operacje wyświetlają czytelny **pasek postępu**
- Dźwiękowe **powiadomienie o zakończeniu** optymalizacji
- Interaktywne pytania na koniec: Malwarebytes, wsparcie projektu, restart
- Szczegółowy **raport tekstowy** zapisany do `C:\CLEANER by MAG 1.09 - WYNIKI.txt`

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

Po zakończeniu działania skrypt automatycznie zapisuje plik tekstowy:

```
C:\CLEANER by MAG 1.09 - WYNIKI.txt
```

Raport zawiera:
- datę i czas wykonania (start i koniec)
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
| Cache przeglądarek | Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox |
| Cache Microsoft Teams | Cache, blob_storage, GPUCache, tmp |
| Kolejka drukarki | Czyści spooler + usuwa stare sterowniki (aktywne zachowane) |
| Microsoft Store cache | Czyści cache paczek AppX |
| Oczyszczanie dysku | cleanmgr z 34 kategoriami; `SKIP` jeśli niedostępny w systemie |
| SFC (wstępny) | Sprawdza integralność plików — pasek postępu + kod błędu |
| DISM RestoreHealth | Naprawia obraz systemu — pasek postępu + kod błędu |
| SFC (końcowy) | Ponowna weryfikacja po DISM — pasek postępu + kod błędu |
| DISM StartComponentCleanup | Czyści stare składniki aktualizacji |
| Logi CBS | Usuwa duże logi CBS |
| Windows Update cache | Pełny reset usług i SoftwareDistribution |
| Usunięcie Windows.old | Odzyskuje miejsce; `SKIP` jeśli folder nie istnieje |
| Defragmentacja / TRIM | `defrag /O` — zależnie od wykrytego typu dysku |
| Thumbnail Cache | Czyści cache miniatur, restartuje Eksplorator |
| Naprawa WMI | Przebudowuje repozytorium WMI (pushd/popd) |
| Pliki Minidump | Usuwa zrzuty awaryjne |
| Raporty WER | Czyści raporty błędów Windows |

---

## ⚠️ Ważne uwagi

- Skrypt **planuje CHKDSK** na następny restart — wymagane ponowne uruchomienie, aby wszystkie zmiany weszły w życie
- Wyłączona jest **hibernacja** — ale tylko na SSD/NVMe; na HDD hibernacja pozostaje aktywna
- Usługa **Windows Search (WSearch)** zostaje wyłączona — może wpłynąć na wyszukiwanie w menu Start
- Usługa **SysMain (Superfetch)** wyłączana jest tylko na SSD; na HDD pozostaje aktywna, bo faktycznie poprawia wydajność
- Folder **Prefetch** jest czyszczony tylko na HDD — na SSD Windows zarządza nim automatycznie i ręczne czyszczenie jest zbędne
- Skrypt usuwa wybrane **wbudowane aplikacje** Windows: Solitaire, Bing, Zune, People, Skype, Office Hub, 3D Builder, Get Started
- Wyłączane są wpisy autostartu popularnych aplikacji (Spotify, Discord, Steam, Epic, Teams itd.) — można je ponownie włączyć ręcznie
- Folder **Windows.old** jest usuwany trwale — po tej operacji nie ma możliwości cofnięcia do poprzedniej wersji Windows przez ustawienia systemowe

---

## 🔄 Co nowego w v1.09?

- ✅ **Prefetch inteligentnie pomijany na SSD** — na dyskach SSD/NVMe folder Prefetch jest zarządzany automatycznie przez system; skrypt oznacza tę operację jako `[ SKIP ]` zamiast niepotrzebnie czyścić
- ✅ **Sprawdzanie dostępności `cleanmgr`** przez `where` przed uruchomieniem — jeśli narzędzie nie jest dostępne (np. okrojone wydania Windows), operacja jest oznaczana `[ SKIP ]` zamiast cicho zawiesić
- ✅ **Limit czasowy pobierania AdwCleaner** — dodano `-TimeoutSec 30` do `Invoke-WebRequest`, co zapobiega zawieszeniu skryptu na słabym lub niestabilnym łączu
- ✅ **Obsługa braku połączenia dla Malwarebytes** — wyraźny komunikat zamiast cichego pominięcia, gdy nie udało się pobrać instalatora
- ✅ **Poprawka `!errorlevel!`** — konsekwentne użycie opóźnionego rozwijania zmiennych (`delayed expansion`) zamiast `%errorlevel%` w miejscach, gdzie wartość mogłaby być odczytana nieprawidłowo wewnątrz bloków warunkowych

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

**Autor:** Mag | **Wersja:** 1.09 (26/04/2026)
