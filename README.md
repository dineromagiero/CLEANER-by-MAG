# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.06-blue)
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
- Cache przeglądarek: **Chrome, Edge, Brave, Opera, Opera GX, Firefox, Vivaldi, Waterfox** *(nowe w v1.06)*
- Cache **Microsoft Teams** *(nowe w v1.06)*
- Cache **Microsoft Store**
- Kolejka **drukarki (Spooler)** + stare sterowniki drukarek *(nowe w v1.06)*
- Logi **CBS** i Windows Update (`SoftwareDistribution`)
- Cache miniatur z restartem Eksploratora
- Folder **Windows.old** (stara instalacja systemu) *(nowe w v1.06)*
- Narzędzie **Oczyszczanie dysku** (`cleanmgr /sagerun:99`) *(nowe w v1.06)*

### 🔧 Naprawa systemu
- **SFC** (System File Checker) — dwukrotnie: przed i po DISM
- **DISM RestoreHealth** — naprawa obrazu systemu
- **DISM StartComponentCleanup** — czyszczenie bazy składników po aktualizacjach *(nowe w v1.06)*
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
- Wyłączenie wybranych programów **startowych**: Skype, OneDrive, Spotify, Discord, Steam, Epic Games, Teams, GOG Galaxy, EA App, Ubisoft Connect, Overwolf, Zoom, Viber *(nowe w v1.06)*

### 🌐 Sieć
- **Reset proxy** systemowego (WinHTTP i rejestr IE)
- Flush i rejestracja **DNS**
- Reset **stosu TCP/IP** i Winsock
- Ustawienie serwerów DNS: **Cloudflare (1.1.1.1 / 1.0.0.1)** i **Google (8.8.8.8 / 8.8.4.4)** dla IPv4 i IPv6
- Optymalizacja parametrów **TCP** (autotuninglevel, RSS, chimney)

### 🛡️ Prywatność i telemetria
- Wyłączenie zbierania danych diagnostycznych (**telemetria**)
- Wyłączenie **Cortany**
- Wyłączenie **Bing Search** w menu Start *(nowe w v1.06)*
- Wyłączenie śledzenia wpisów i kolekcji danych wejściowych *(nowe w v1.06)*
- Wyłączenie sugestii i powiadomień systemowych (ContentDeliveryManager) *(nowe w v1.06)*
- Wyłączenie raportowania błędów Windows (WER)

### ⚙️ Usługi i harmonogramy
- Dezaktywacja zbędnych usług: `SysMain`, `DiagTrack`, `WSearch`, `MapsBroker`, `Fax`, `RetailDemo`
- Wyłączenie zbędnych harmonogramów systemowych (CEIP, feedback, Xbox, dysk, WER)
- Blokada procesów **przeglądarek w tle** i ich autoaktualizacji (Chrome, Edge, Brave, Opera, Firefox, Adobe)

### 🖥️ Interfejs i UX
- Automatyczne **odblokowanie skryptu** (`Unblock-File`) po pobraniu z internetu *(nowe w v1.06)*
- Kolorowy, czytelny interfejs konsolowy (ANSI: aqua / biały / żółty)
- Dynamiczne dopasowanie rozmiaru okna konsoli do rozdzielczości ekranu
- **Sprawdzanie aktualizacji** przez GitHub API — możliwość pobrania nowej wersji przed startem
- Dźwiękowe **powiadomienie o zakończeniu** optymalizacji *(nowe w v1.06)*
- Interaktywne pytania na koniec: Malwarebytes, wsparcie projektu, restart
- Szczegółowy **raport tekstowy** zapisany do `C:\CLEANER by MAG 1.06 - WYNIKI.txt`

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

> ⚠️ **Uwaga:** Skrypt **musi** być uruchomiony jako administrator. Bez uprawnień administratora zakończy pracę z komunikatem błędu. Skrypt automatycznie odblokuje się, jeśli Windows zablokował go po pobraniu z internetu.

---

## 📊 Raport wynikowy

Po zakończeniu działania skrypt automatycznie zapisuje plik tekstowy:

```
C:\CLEANER by MAG 1.06 - WYNIKI.txt
```

Raport zawiera:
- datę i czas wykonania (start i koniec)
- ilość zwolnionego miejsca na dysku C:
- parametry systemu (RAM, pagefile, model dysku, rozmiar kolejki drukarki, rozmiar CBS.log)
- listę wszystkich wykonanych operacji ze statusem `[ OK ]` lub `[----]`
- nazwę komputera i użytkownika

---

## 🗂️ Wykonywane operacje — podsumowanie

| Operacja | Opis | Nowość v1.06 |
|----------|------|:------------:|
| Punkt przywracania systemu | Tworzy przed jakimikolwiek zmianami (z weryfikacją) | |
| Windows Defender | Aktualizacja sygnatur + szybki skan | |
| AdwCleaner | Skan i czyszczenie adware, auto-deinstalacja | |
| Reset proxy | Usuwa proxy systemowe i IE/WinHTTP | |
| Pliki tymczasowe | `%TEMP%` i `C:\Windows\Temp` | |
| Prefetch | Czyści folder prefetch | |
| DNS / Sieć | Flush, reset TCP/IP, Cloudflare + Google DNS | |
| Dzienniki zdarzeń | Czyści cały Event Viewer | |
| Usługi systemowe | Wyłącza 6 zbędnych usług | |
| Optymalizacja rejestru | Tweaki wydajności, prywatności i UI | |
| Cache przeglądarek | Chrome, Edge, Brave, Opera, Opera GX, **Firefox, Vivaldi, Waterfox** | ✅ |
| Cache Microsoft Teams | Cache, blob_storage, GPUCache, tmp | ✅ |
| Kolejka drukarki | Czyści spooler + usuwa stare sterowniki | ✅ |
| Microsoft Store cache | Czyści cache paczek AppX | |
| SFC (wstępny) | Sprawdza integralność plików systemowych | |
| DISM RestoreHealth | Naprawia obraz systemu | |
| SFC (końcowy) | Ponowna weryfikacja po DISM | |
| DISM StartComponentCleanup | Czyści stare składniki aktualizacji | ✅ |
| Logi CBS | Usuwa duże logi CBS | |
| Windows Update cache | Pełny reset usług i SoftwareDistribution | |
| Usunięcie Windows.old | Odzyskuje miejsce po starych wersjach Windows | ✅ |
| Defragmentacja / TRIM | `defrag /O` — zależnie od typu dysku | |
| Thumbnail Cache | Czyści cache miniatur, restartuje Eksplorator | |
| Naprawa WMI | Przebudowuje repozytorium WMI | |
| Pliki Minidump | Usuwa zrzuty awaryjne | |
| Raporty WER | Czyści raporty błędów Windows | |

---

## ⚠️ Ważne uwagi

- Skrypt **planuje CHKDSK** na następny restart — wymagane ponowne uruchomienie, aby wszystkie zmiany weszły w życie
- Wyłączona jest **hibernacja** — plik `hiberfil.sys` zostaje usunięty (dotyczy systemów z SSD)
- Usługa **Windows Search (WSearch)** zostaje wyłączona — może wpłynąć na wyszukiwanie w menu Start
- Skrypt usuwa wybrane **wbudowane aplikacje** Windows: Solitaire, Bing, People, Skype, Office Hub, 3D Builder, Get Started
- Wyłączane są wpisy autostartu popularnych aplikacji (Spotify, Discord, Steam, Epic, Teams itd.) — można je ponownie włączyć ręcznie
- Folder **Windows.old** jest usuwany trwale — po tej operacji nie ma możliwości cofnięcia do poprzedniej wersji Windows przez ustawienia systemowe

---

## 🔄 Co nowego w v1.06?

- ✅ Automatyczne odblokowanie skryptu po pobraniu z internetu (`Unblock-File`)
- ✅ Rozszerzony cache przeglądarek: **Firefox, Vivaldi, Waterfox**
- ✅ Czyszczenie cache **Microsoft Teams**
- ✅ Usuwanie **starych sterowników drukarek** (zachowuje aktywne)
- ✅ Usuwanie folderu **Windows.old** z odzyskiem miejsca
- ✅ Uruchamianie graficznego **Oczyszczania dysku** (`cleanmgr`)
- ✅ **DISM StartComponentCleanup** — czyszczenie bazy starych składników aktualizacji
- ✅ Wyłączanie **programów startowych** (Discord, Steam, Epic, Teams, Zoom i inne)
- ✅ Rozszerzone wyłączanie **śledzenia i sugestii** systemowych (Bing, InputPersonalization, ContentDelivery)
- ✅ Dźwiękowe **powiadomienie o zakończeniu** optymalizacji
- ✅ Ulepszona weryfikacja punktu przywracania z komunikatem błędu gdy ochrona systemu jest wyłączona
- ✅ Czystszy interfejs konsolowy

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

**Autor:** Mag
**Wersja:** 1.06 (23/04/2026)
**Status:** Stabilna
