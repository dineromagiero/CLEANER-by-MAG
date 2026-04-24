# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.07-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do kompleksowej optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od skanowania antywirusowego i usuwania śmieciowych plików, przez naprawę plików systemowych, aż po konfigurację sieci i rejestru. Działa w trybie w pełni interaktywnym z kolorowym interfejsem konsolowym i po zakończeniu generuje szczegółowy raport tekstowy.

---

## ✨ Co robi skrypt?

### 🔒 Bezpieczeństwo i antywirusy
- Tworzy **punkt przywracania systemu** przed wprowadzeniem jakichkolwiek zmian (z weryfikacją powodzenia)
- Aktualizuje i uruchamia **szybki skan Windows Defender**
- Pobiera, uruchamia i automatycznie usuwa **AdwCleaner** (skan + czyszczenie adware)
- Opcjonalne pobranie, skan i deinstalacja **Malwarebytes** po zakończeniu

### 🗑️ Czyszczenie plików
- Pliki tymczasowe użytkownika i systemu (`%TEMP%`, `C:\Windows\Temp`)
- Folder **Prefetch**
- **Minidumpy** i raporty błędów Windows (WER)
- Cache **ikon** i czcionek
- Cache przeglądarek: **Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox**
- Cache **Microsoft Teams**
- Cache **Microsoft Store**
- Kolejka **drukarki (Spooler)** + stare sterowniki drukarek
- Logi **CBS** i Windows Update (`SoftwareDistribution`)
- Cache miniatur z restartem Eksploratora
- Folder **Windows.old** (stara instalacja systemu)
- Narzędzie **Oczyszczanie dysku** (`cleanmgr /sagerun:99`)

### 🔧 Naprawa systemu
- **SFC** (System File Checker) — dwukrotnie: przed i po DISM, z **paskiem postępu** w konsoli
- **DISM RestoreHealth** — naprawa obrazu systemu, z **paskiem postępu** w konsoli
- **DISM StartComponentCleanup** — czyszczenie bazy składników po aktualizacjach, z **paskiem postępu**
- **CHKDSK** zaplanowany na następny restart
- Naprawa repozytorium **WMI**
- Pełny reset **Windows Update** (zatrzymanie usług, usunięcie SoftwareDistribution i catroot2, rejestracja DLL)

### ⚡ Optymalizacja wydajności
- Automatyczne wykrywanie urządzenia: **laptop vs PC stacjonarny**
- Ustawienie **planu zasilania Wysoka Wydajność** (tylko dla PC stacjonarnych)
- Wyłączenie **hibernacji** (dla SSD/NVMe)
- Wyłączenie **Fast Startup**
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
- Dezaktywacja zbędnych usług: `SysMain`, `DiagTrack`, `WSearch`, `MapsBroker`, `Fax`, `RetailDemo`
- Wyłączenie zbędnych harmonogramów systemowych (CEIP, feedback, Xbox, dysk, WER)
- Blokada procesów **przeglądarek w tle** i ich autoaktualizacji (Chrome, Edge, Brave, Opera, Firefox, Adobe)

### 🖥️ Interfejs i UX
- Automatyczne **odblokowanie skryptu** (`Unblock-File`) po pobraniu z internetu
- Kolorowy, czytelny interfejs konsolowy (ANSI: aqua / biały / żółty)
- **Nagłówek z linkiem do GitHub** widoczny od pierwszego uruchomienia
- Dynamiczne dopasowanie rozmiaru okna i **bufora konsoli (200 linii)** do rozdzielczości ekranu
- **Sprawdzanie aktualizacji** przez GitHub API — możliwość pobrania nowej wersji przed startem, jeszcze przed sprawdzaniem uprawnień
- SFC, DISM i inne długie operacje wyświetlają teraz czytelny **pasek postępu** zamiast surowego tekstu systemowego
- Dźwiękowe **powiadomienie o zakończeniu** optymalizacji
- Interaktywne pytania na koniec: Malwarebytes, wsparcie projektu, restart
- Szczegółowy **raport tekstowy** zapisany do `C:\CLEANER by MAG 1.07 - WYNIKI.txt`

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
C:\CLEANER by MAG 1.07 - WYNIKI.txt
```

Raport zawiera:
- datę i czas wykonania (start i koniec)
- ilość zwolnionego miejsca na dysku C:
- parametry systemu (RAM, pagefile, model dysku, rozmiar kolejki drukarki, rozmiar CBS.log)
- listę wszystkich wykonanych operacji ze statusem `[ OK ]` lub `[----]`
- nazwę komputera i użytkownika

---

## 🗂️ Wykonywane operacje — podsumowanie

| Operacja | Opis |
|----------|------|
| Punkt przywracania systemu | Tworzy przed jakimikolwiek zmianami (z weryfikacją) |
| Windows Defender | Aktualizacja sygnatur + szybki skan |
| AdwCleaner | Skan i czyszczenie adware, auto-deinstalacja |
| Reset proxy | Usuwa proxy systemowe i IE/WinHTTP |
| Pliki tymczasowe | `%TEMP%` i `C:\Windows\Temp` |
| Prefetch | Czyści folder prefetch |
| DNS / Sieć | Flush, reset TCP/IP, Cloudflare + Google DNS (IPv4 + IPv6) |
| Dzienniki zdarzeń | Czyści cały Event Viewer |
| Usługi systemowe | Wyłącza 6 zbędnych usług |
| Optymalizacja rejestru | Tweaki wydajności, prywatności i UI |
| Cache przeglądarek | Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox |
| Cache Microsoft Teams | Cache, blob_storage, GPUCache, tmp |
| Kolejka drukarki | Czyści spooler + usuwa stare sterowniki |
| Microsoft Store cache | Czyści cache paczek AppX |
| SFC (wstępny) | Sprawdza integralność plików — pasek postępu |
| DISM RestoreHealth | Naprawia obraz systemu — pasek postępu |
| SFC (końcowy) | Ponowna weryfikacja po DISM — pasek postępu |
| DISM StartComponentCleanup | Czyści stare składniki aktualizacji — pasek postępu |
| Logi CBS | Usuwa duże logi CBS |
| Windows Update cache | Pełny reset usług i SoftwareDistribution |
| Usunięcie Windows.old | Odzyskuje miejsce po starych wersjach Windows |
| Defragmentacja / TRIM | `defrag /O` — zależnie od typu dysku, działa w tle |
| Thumbnail Cache | Czyści cache miniatur, restartuje Eksplorator |
| Naprawa WMI | Przebudowuje repozytorium WMI |
| Pliki Minidump | Usuwa zrzuty awaryjne |
| Raporty WER | Czyści raporty błędów Windows |

---

## ⚠️ Ważne uwagi

- Skrypt **planuje CHKDSK** na następny restart — komputer musi zostać uruchomiony ponownie, aby wszystkie zmiany weszły w życie
- Wyłączona jest **hibernacja** — plik `hiberfil.sys` zostaje usunięty (dotyczy systemów z SSD)
- Usługa **Windows Search (WSearch)** zostaje wyłączona — może wpłynąć na wyszukiwanie w menu Start
- Skrypt usuwa wybrane **wbudowane aplikacje** Windows: Solitaire, Bing, People, Skype, Office Hub, 3D Builder, Get Started
- Wyłączane są wpisy autostartu popularnych aplikacji (Spotify, Discord, Steam, Epic, Teams itd.) — można je ponownie włączyć ręcznie
- Folder **Windows.old** jest usuwany trwale — po tej operacji nie ma możliwości cofnięcia do poprzedniej wersji Windows przez ustawienia systemowe

---

## 🔄 Co nowego w v1.07?

- ✅ **Paski postępu** dla SFC, DISM RestoreHealth i DISM StartComponentCleanup — koniec ze ścianą niezrozumiałego tekstu podczas naprawy systemu
- ✅ **Nagłówek z linkiem do GitHub** (`github.com/dineromagiero`) widoczny od pierwszego uruchomienia
- ✅ **Sprawdzanie wersji** przeniesione przed weryfikację uprawnień — informacja o aktualizacji pojawia się natychmiast
- ✅ **Bufor konsoli ustawiony na 200 linii** — możliwość przewijania historii operacji bez ucinania tekstu
- ✅ **Defragmentacja** działa w tle bez zaśmiecania konsoli
- ✅ Pobieranie AdwCleaner przez `Invoke-WebRequest` (PowerShell) zamiast `curl`
- ✅ Komunikat braku uprawnień administratora z **10-sekundowym odliczaniem** zamiast natychmiastowego zamknięcia
- ✅ Ogólne poprawki czytelności komunikatów i separatorów w interfejsie

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

**Autor:** Mag | **Wersja:** 1.07 (24/04/2026)
