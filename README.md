# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.08-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do kompleksowej optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od skanowania antywirusowego i usuwania śmieciowych plików, przez naprawę plików systemowych, aż po konfigurację sieci i rejestru. Od wersji 1.08 skrypt jest **inteligentniejszy** — dostosowuje swoje decyzje do realnego sprzętu, zamiast stosować te same ustawienia na każdym komputerze.

---

## ✨ Co robi skrypt?

### 🔒 Bezpieczeństwo i antywirusy
- Tworzy **punkt przywracania systemu** przed wprowadzeniem jakichkolwiek zmian (z weryfikacją powodzenia)
- Aktualizuje i uruchamia **szybki skan Windows Defender** z obsługą kodu błędu
- Pobiera, uruchamia i automatycznie usuwa **AdwCleaner** (skan + czyszczenie adware); jeśli brak internetu — operacja jest oznaczana jako `[ SKIP ]`
- Opcjonalne pobranie, skan i deinstalacja **Malwarebytes** po zakończeniu

### 🗑️ Czyszczenie plików
- Pliki tymczasowe użytkownika i systemu (`%TEMP%`, `C:\Windows\Temp`)
- Folder **Prefetch**
- **Minidumpy** i raporty błędów Windows (WER)
- Cache **ikon** i czcionek
- Cache przeglądarek: **Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox** (domyślny profil + wszystkie Profile *)
- Cache **Microsoft Teams**
- Cache **Microsoft Store** (LocalCache, INetCache, Temp dla wszystkich pakietów AppX)
- Kolejka **drukarki (Spooler)** + stare sterowniki drukarek (W32X86, x64, ia64)
- Logi **CBS** i Windows Update (`SoftwareDistribution`, `catroot2`)
- Cache miniatur z restartem Eksploratora
- Folder **Windows.old** z weryfikacją powodzenia usunięcia
- **Oczyszczanie dysku** (`cleanmgr`) z konfiguracją **34 kategorii** przed uruchomieniem

### 🔧 Naprawa systemu
- **SFC** (System File Checker) — dwukrotnie: przed i po DISM, z paskiem postępu i **kodem błędu** w raporcie
- **DISM RestoreHealth** — naprawa obrazu systemu, z paskiem postępu i **kodem błędu** w raporcie
- **DISM StartComponentCleanup** — czyszczenie bazy składników po aktualizacjach
- **CHKDSK** zaplanowany na następny restart
- Naprawa repozytorium **WMI** z bezpieczną obsługą katalogu (`pushd/popd`)
- Pełny reset **Windows Update** (zatrzymanie usług, usunięcie SoftwareDistribution i catroot2, rejestracja DLL)

### ⚡ Optymalizacja wydajności
- Automatyczne wykrywanie urządzenia: **laptop vs PC stacjonarny**
- Ustawienie **planu zasilania Wysoka Wydajność** (tylko dla PC stacjonarnych)
- **Inteligentne wykrywanie dysku** przez `Get-PhysicalDisk` (MediaType: SSD / NVMe / HDD) z fallbackiem na nazwę modelu
- Wyłączenie **hibernacji** — **tylko na SSD/NVMe**; na HDD hibernacja pozostaje bez zmian
- Wyłączenie **Fast Startup**
- **SysMain (Superfetch)** — wyłączany **tylko na SSD** (na HDD poprawia wydajność, pozostawiany)
- **DisablePagingExecutive** — włączany **tylko przy ≥ 8 GB RAM**; przy mniej RAM ustawienie jest pomijane
- Optymalizacja **pliku stronicowania** (PageFile) — automatycznie dopasowana do ilości RAM
- Tweaki rejestru: priorytety **procesora**, zarządzanie **pamięcią**, animacje UI
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
- Blokada procesów **przeglądarek w tle** i ich autoaktualizacji (Chrome, Edge, Brave, Opera — dynamiczne wyszukiwanie nazwy usługi, Firefox, Adobe)

### 🖥️ Interfejs i UX
- Automatyczne **odblokowanie skryptu** (`Unblock-File`) po pobraniu z internetu
- Kolorowy, czytelny interfejs konsolowy (ANSI: aqua / biały / żółty)
- Nagłówek z linkiem do GitHub widoczny od pierwszego uruchomienia
- Dynamiczne dopasowanie rozmiaru okna i **buforu konsoli (200 linii)**
- **Sprawdzanie aktualizacji** przez GitHub API — przed sprawdzaniem uprawnień, z możliwością pobrania nowej wersji
- Trójstanowy system statusów w raporcie: `[ OK ]` / `[ SKIP ]` / `[----]`
- SFC, DISM i inne długie operacje wyświetlają czytelny **pasek postępu**
- Dźwiękowe **powiadomienie o zakończeniu** optymalizacji
- Interaktywne pytania na koniec: Malwarebytes, wsparcie projektu, restart
- Szczegółowy **raport tekstowy** zapisany do `C:\CLEANER by MAG 1.08 - WYNIKI.txt`

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
C:\CLEANER by MAG 1.08 - WYNIKI.txt
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
| AdwCleaner | Skan i czyszczenie adware; `SKIP` jeśli brak internetu |
| Reset proxy | Usuwa proxy systemowe i IE/WinHTTP |
| Pliki tymczasowe | `%TEMP%` i `C:\Windows\Temp` |
| Prefetch | Czyści folder prefetch |
| DNS / Sieć | Flush, reset TCP/IP, Cloudflare + Google DNS (IPv4 + IPv6) |
| Dzienniki zdarzeń | Czyści cały Event Viewer |
| Usługi systemowe | SysMain (tylko SSD), DiagTrack, WSearch, MapsBroker, Fax, RetailDemo |
| Optymalizacja rejestru | Tweaki wydajności, prywatności i UI |
| Cache przeglądarek | Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox |
| Cache Microsoft Teams | Cache, blob_storage, GPUCache, tmp |
| Kolejka drukarki | Czyści spooler + usuwa stare sterowniki (W32X86 / x64 / ia64) |
| Microsoft Store cache | Czyści cache paczek AppX |
| Oczyszczanie dysku | cleanmgr z 34 konfigurowalnymi kategoriami |
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
- Wyłączona jest **hibernacja** — ale **tylko na SSD/NVMe**; na HDD hibernacja pozostaje aktywna
- Usługa **Windows Search (WSearch)** zostaje wyłączona — może wpłynąć na wyszukiwanie w menu Start
- Usługa **SysMain (Superfetch)** wyłączana jest **tylko na dyskach SSD**; na HDD pozostaje aktywna
- Skrypt usuwa wybrane **wbudowane aplikacje** Windows: Solitaire, Bing, Zune, People, Skype, Office Hub, 3D Builder, Get Started
- Wyłączane są wpisy autostartu popularnych aplikacji (Spotify, Discord, Steam, Epic, Teams itd.) — można je ponownie włączyć ręcznie
- Folder **Windows.old** jest usuwany trwale — po tej operacji nie ma możliwości cofnięcia do poprzedniej wersji Windows przez ustawienia systemowe

---

## 🔄 Co nowego w v1.08?

- ✅ **Trójstanowy system statusów** — obok `[ OK ]` i `[----]` pojawia się nowy `[ SKIP ]` gdy operacja jest świadomie pominięta (np. brak internetu, folder nie istnieje)
- ✅ **SysMain (Superfetch) inteligentnie sterowany** — wyłączany tylko na SSD; na HDD zostaje aktywny, bo faktycznie poprawia wydajność
- ✅ **DisablePagingExecutive uzależniony od RAM** — tweak włączany tylko przy ≥ 8 GB; przy mniejszej ilości RAM jest pomijany z komunikatem
- ✅ **Hibernacja uzależniona od typu dysku** — wyłączana tylko na SSD/NVMe; na HDD pozostawiana bez zmian
- ✅ **Ulepszone wykrywanie typu dysku** — użycie `Get-PhysicalDisk` (MediaType) z fallbackiem na nazwę modelu
- ✅ **Kody błędów w raporcie** dla SFC (wstępny), SFC (końcowy) i DISM — widać dokładnie, która operacja naprawcza się nie powiodła i z jakim kodem
- ✅ **Windows.old** — weryfikacja po usunięciu; jeśli nie istnieje, wpis w raporcie ma status `[ SKIP ]`
- ✅ **cleanmgr skonfigurowany z 34 kategoriami** przez rejestr przed uruchomieniem — oczyszczanie jest teraz kompletne i przewidywalne
- ✅ **Opera autoupdate** — usługa szukana dynamicznie przez `sc query` zamiast hardcoded nazwy z numerem wersji
- ✅ **Naprawa WMI** z bezpiecznym `pushd/popd` — katalog roboczy jest przywracany poprawnie po zakończeniu

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

**Autor:** Mag | **Wersja:** 1.08 (25/04/2026)
