# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.05-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od usuwania śmieciowych plików, przez naprawę systemu, aż po konfigurację sieci i rejestru. Działa w trybie w pełni interaktywnym z kolorowym interfejsem konsolowym i generuje szczegółowy raport po zakończeniu.

---

## ✨ Co robi skrypt?

### 🔒 Bezpieczeństwo i antywirusy
- Tworzy **punkt przywracania systemu** przed wprowadzeniem jakichkolwiek zmian
- Aktualizuje i uruchamia **szybki skan Windows Defender**
- Pobiera, uruchamia i automatycznie usuwa **AdwCleaner** (skan + czyszczenie)
- Opcjonalny skan i usunięcie **Malwarebytes** po zakończeniu

### 🗑️ Czyszczenie plików
- Pliki tymczasowe użytkownika i systemu (`%TEMP%`, `C:\Windows\Temp`)
- Folder **Prefetch**
- **Minidumpy** i raporty błędów Windows (WER)
- Cache **ikon** i czcionek
- Cache przeglądarek: **Chrome, Edge, Brave, Opera, Opera GX**
- Cache **Microsoft Store** (`wsreset`)
- Kolejka **drukarki (Spooler)**
- Logi **CBS** i Windows Update (`SoftwareDistribution`)
- Cache miniatur (`Thumbnail Cache`)

### 🔧 Naprawa systemu
- **SFC** (System File Checker) — dwukrotnie: przed i po DISM
- **DISM RestoreHealth** — naprawa obrazu systemu
- **CHKDSK** zaplanowany na następny restart
- Naprawa repozytorium **WMI**

### ⚡ Optymalizacja wydajności
- Ustawienie **planu zasilania** (Wysoka Wydajność dla PC, bez zmian dla laptopa)
- Wyłączenie **hibernacji** (dla SSD/NVMe)
- Wyłączenie **Fast Startup**
- Optymalizacja **pliku stronicowania** (PageFile) — automatycznie dopasowana do ilości RAM
- Priorytety **procesora** i zarządzanie **pamięcią** (tweaki rejestru)
- **Defragmentacja** (HDD) lub **TRIM** (SSD)

### 🌐 Sieć
- **Reset proxy** systemowego i IE/WinHTTP
- Flush i rejestracja **DNS**
- Reset **stosu TCP/IP** i Winsock
- Ustawienie serwerów DNS na **Cloudflare (1.1.1.1)** i **Google (8.8.8.8)**
- Optymalizacja parametrów **TCP** (autotuninglevel, RSS, chimney)

### 🛡️ Prywatność i telemetria
- Wyłączenie zbierania danych diagnostycznych (**telemetria**)
- Wyłączenie **Cortany**
- Wyłączenie spersonalizowanych doświadczeń
- Wyłączenie raportowania błędów Windows (WER)

### ⚙️ Usługi i harmonogramy
- Dezaktywacja zbędnych usług: `SysMain`, `DiagTrack`, `WSearch`, `MapsBroker`, `Fax`, `RetailDemo`
- Wyłączenie zbędnych harmonogramów systemowych (CEIP, feedback, Xbox, dysk)
- Blokada procesów **przeglądarek w tle** i ich autoaktualizacji

### 🖥️ Interfejs i UX
- Kolorowy, czytelny interfejs konsolowy (ANSI)
- Dynamiczne dopasowanie rozmiaru okna konsoli do ekranu
- **Sprawdzanie aktualizacji** skryptu przez GitHub API
- Interaktywne pytania (restart, Malwarebytes, wsparcie projektu)
- Szczegółowy **raport tekstowy** zapisany do `C:\CLEANER by MAG 1.05 - WYNIKI.txt`

---

## 🚀 Wymagania

| Wymaganie | Szczegóły |
|-----------|-----------|
| System | Windows 10 / Windows 11 |
| Uprawnienia | **Administrator** (wymagane) |
| Połączenie | Opcjonalne (dla aktualizacji, AdwCleaner, Malwarebytes) |
| .NET / PS | PowerShell 5.x (wbudowany w Windows) |

---

## 📥 Instalacja i uruchomienie

1. Pobierz najnowszą wersję ze strony [Releases](https://github.com/dineromagiero/CLEANER-by-MAG/releases/latest)
2. Kliknij prawym przyciskiem myszy na plik `.bat`
3. Wybierz **„Uruchom jako administrator"**
4. Postępuj zgodnie z instrukcjami na ekranie

> ⚠️ **Uwaga:** Skrypt **musi** być uruchomiony jako administrator. Bez uprawnień administratora zakończy pracę z komunikatem błędu.

---

## 📊 Raport wynikowy

Po zakończeniu działania skrypt automatycznie zapisuje plik tekstowy:

```
C:\CLEANER by MAG 1.05 - WYNIKI.txt
```

Raport zawiera:
- datę i czas wykonania
- ilość zwolnionego miejsca na dysku
- wykryte parametry systemu (RAM, pagefile, model dysku)
- listę wykonanych operacji ze statusem `[ OK ]` lub `[----]`

---

## 🗂️ Wykonywane operacje — podsumowanie

| Operacja | Opis |
|----------|------|
| Punkt przywracania | Tworzy przed jakimikolwiek zmianami |
| Windows Defender | Aktualizacja sygnatur + szybki skan |
| AdwCleaner | Skan i czyszczenie adware |
| Reset proxy | Usuwa proxy systemowe i IE |
| Pliki tymczasowe | Temp użytkownika i systemu |
| Prefetch | Czyści folder prefetch |
| DNS / Sieć | Flush, reset, Cloudflare + Google DNS |
| Dzienniki zdarzeń | Czyści Event Viewer |
| Usługi systemowe | Wyłącza zbędne usługi |
| Optymalizacja rejestru | Tweaki wydajności i prywatności |
| Cache przeglądarek | Chrome, Edge, Brave, Opera, Opera GX |
| Kolejka drukarki | Czyści spooler |
| Microsoft Store cache | Uruchamia wsreset |
| SFC (wstępny) | Sprawdza integralność plików |
| DISM RestoreHealth | Naprawia obraz systemu |
| SFC (końcowy) | Ponowna weryfikacja po DISM |
| Logi CBS | Usuwa duże logi CBS |
| Windows Update cache | Czyści SoftwareDistribution |
| Defragmentacja / TRIM | Zależnie od typu dysku |
| Thumbnail Cache | Czyści cache miniatur |
| Naprawa WMI | Przebudowuje repozytorium WMI |
| Pliki Minidump | Usuwa zrzuty awaryjne |
| Raporty WER | Czyści raporty błędów Windows |

---

## ⚠️ Uwagi

- **Kosz NIE jest opróżniany** — musisz zrobić to ręcznie po zakończeniu
- Skrypt **planuje CHKDSK** na następny restart — wymagane ponowne uruchomienie komputera
- Wyłączona jest **hibernacja** (plik hiberfil.sys zostaje usunięty) — dotyczy głównie dysków SSD
- Usługa **Windows Search (WSearch)** zostaje wyłączona — wpływa na wyszukiwanie w menu Start
- Skrypt usuwa wybrane **wbudowane aplikacje** (Solitaire, Bing, People, Skype, Office Hub, 3D Builder)

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

**Autor:** Mag  
**Wersja:** 1.05 (21/04/2026)  
**Status:** Stabilna
