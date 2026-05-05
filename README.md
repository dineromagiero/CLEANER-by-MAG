# 🧹 CLEANER by MAG

> Kompleksowy skrypt optymalizacji i czyszczenia systemu Windows — jednym kliknięciem.

![Version](https://img.shields.io/badge/wersja-1.16-blue)
![Platform](https://img.shields.io/badge/platforma-Windows%2010%2F11-0078d4?logo=windows)
![Language](https://img.shields.io/badge/język-Batch%20%2F%20PowerShell-4EAA25)
![License](https://img.shields.io/badge/licencja-MIT-green)

---

## 📋 Opis

**CLEANER by MAG** to zaawansowany skrypt `.bat` do kompleksowej optymalizacji systemu Windows. Automatyzuje dziesiątki żmudnych zadań konserwacyjnych — od skanowania antywirusowego i usuwania śmieciowych plików, przez naprawę plików systemowych, aż po konfigurację sieci i rejestru. Skrypt **inteligentnie dostosowuje się do sprzętu i systemu**, instaluje się w Menu Start i po każdym uruchomieniu pokazuje historię wszystkich poprzednich sesji czyszczenia.

---

## 📋 Spis treści

- [Funkcje](#-funkcje)
- [Wymagania](#-wymagania)
- [Instalacja i użycie](#-instalacja-i-użycie)
- [Co robi skrypt](#-co-robi-skrypt-krok-po-kroku)
- [Raportowanie](#-raportowanie)
- [Aktualizacje](#-aktualizacje-automatyczne)
- [Wsparcie projektu](#-wsparcie-projektu)

---

## ✨ Funkcje

### 🔒 Bezpieczeństwo i naprawa
- Tworzenie punktu przywracania systemu przed jakimikolwiek zmianami
- Aktualizacja i szybkie skanowanie Windows Defender
- Automatyczne pobranie, uruchomienie i usunięcie **AdwCleaner** (skanowanie adware)
- Naprawa plików systemowych: **SFC** (dwukrotnie) + **DISM RestoreHealth**
- Naprawa repozytorium **WMI**
- Planowanie **CHKDSK** przy kolejnym uruchomieniu
- Opcjonalne skanowanie **Malwarebytes** po zakończeniu optymalizacji

### 🧹 Czyszczenie
| Obszar | Co jest czyszczone |
|---|---|
| Pliki tymczasowe | `%TEMP%`, `C:\Windows\Temp` |
| Prefetch | Tylko na HDD (na SSD pomijane) |
| Thumbnail Cache | Pamięć podręczna miniaturek |
| Kolejka drukarki (Spooler) | Zawieszone zadania drukowania |
| Minidump | Pliki zrzutu awarii |
| Raporty WER | Windows Error Reporting |
| Logi instalatorów | CBS, DISM, NetSetup, waasmedic, INF |
| Dzienniki zdarzeń | Event Viewer (wszystkie kanały) |
| Cache przeglądarek | Chrome, Edge, Firefox, Brave, Opera, Vivaldi, Waterfox, LibreWolf, Zen, Floorp, Thunderbird |
| Cache Microsoft Teams | Klasyczny i nowy (UWP) |
| Cache OneDrive | Logi i kolejka błędów |
| Cache Microsoft Store | Resetowanie wsstore |
| Cache Java / .NET | Tymczasowe pliki ASP.NET |
| Windows.old | Poprzednia instalacja systemu |
| Cleanmgr | Uruchamianie narzędzia Oczyszczanie dysku (profil 99) |

### ⚡ Optymalizacja systemu
- Optymalizacja CPU: priorytety procesów, `Win32PrioritySeparation`
- Optymalizacja RAM: `DisablePagingExecutive` (≥8 GB RAM), konfiguracja page file
- Optymalizacja dysku: automatyczne wykrycie SSD/HDD, wyłączenie hibernacji na SSD, TRIM/defragmentacja
- Optymalizacja timera: wyłączenie `dynamictick`, `useplatformclock`, `tscsyncpolicy=enhanced`
- Konfiguracja planu zasilania: Wysoka Wydajność (smart boost na laptopach)
- Wyłączenie Fast Startup

### 🌐 Sieć
- Reset DNS, Winsock, TCP/IP
- Ustawienie DNS: Cloudflare (1.1.1.1 / 1.0.0.1) + Google (8.8.8.8) dla IPv4 i IPv6
- Reset ustawień proxy
- Optymalizacja TCP (`autotuninglevel`, `rss`, `chimney`)

### 🔧 Rejestr i usługi
- Wyłączenie telemetrii i Cortany
- Przyspieszenie animacji UI (MenuShowDelay, MinAnimate)
- Wyłączenie WER (Windows Error Reporting)
- Wyłączenie zbędnych usług: `DiagTrack`, `WSearch`, `MapsBroker`, `Fax`, `RetailDemo`, `SysMain` (SSD)
- Wyłączenie zbędnych zadań harmonogramu: CEIP, Xbox, Feedback, WER
- Wyłączenie aktualizacji w tle przeglądarek (Chrome, Edge, Firefox, Brave, Opera)
- Wyłączenie Narratora i skrótów ułatwień dostępu
- Usunięcie zbędnych aplikacji Windowsa (Solitaire, Bing, People, Skype, 3D Builder, itp.)
- Ustawienie aktywnych godzin Windows Update (8:00–23:00)
- Usunięcie nieużywanych sterowników PnP

---

## 📋 Wymagania

- **System operacyjny:** Windows 10 / Windows 11
- **Uprawnienia:** Administrator (wymagane)
- **Połączenie z internetem:** opcjonalne (niektóre funkcje jak AdwCleaner, aktualizacje, Malwarebytes wymagają internetu)

---

## 🚀 Instalacja i użycie

1. Pobierz plik `CLEANER_by_MAG_vX_XX.bat` z [zakładki Releases](../../releases/latest)
2. Kliknij **prawym przyciskiem myszy** na pobrany plik
3. Wybierz **„Uruchom jako administrator"**
4. Postępuj zgodnie z instrukcjami wyświetlanymi w konsoli

> ⚠️ Skrypt **nie zadziała** bez uprawnień administratora — wyświetli stosowny komunikat i zakończy działanie.

Przy pierwszym uruchomieniu skrypt automatycznie:
- Kopiuje się do `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\CLEANER by MAG\`
- Tworzy skrót w Menu Start
- Tworzy skrót do folderu z raportami w Dokumentach

---

## 🔍 Co robi skrypt krok po kroku

```
[1]  Sprawdzenie dostępności nowej wersji (GitHub API)
[2]  Weryfikacja uprawnień administratora
[3]  Instalacja skrótu w Menu Start
[4]  Punkt przywracania systemu
[5]  Windows Defender – aktualizacja sygnatur + szybki skan
[6]  AdwCleaner – skan + czyszczenie + deinstalacja
[7]  Reset proxy
[8]  Optymalizacja CPU, RAM, pliku stronicowania
[9]  Wykrycie SSD/HDD + optymalizacje pod typ dysku
[10] Konfiguracja planu zasilania
[11] Wyłączenie Fast Startup
[12] Czyszczenie plików tymczasowych
[13] Czyszczenie Prefetch (tylko HDD)
[14] Reset DNS + Winsock + konfiguracja DNS Cloudflare/Google
[15] Optymalizacja TCP
[16] Czyszczenie dzienników zdarzeń
[17] Uruchomienie Cleanmgr (Oczyszczanie dysku)
[18] Planowanie CHKDSK
[19] Optymalizacja usług systemowych
[20] Optymalizacja Timer Resolution / HPET
[21] Optymalizacja rejestru (telemetria, UI, WER)
[22] Wyłączenie Narratora i ułatwień dostępu
[23] Usunięcie zbędnych aplikacji Windows
[24] Czyszczenie cache przeglądarek (10 przeglądarek)
[25] Czyszczenie cache Microsoft Teams
[26] Czyszczenie cache OneDrive
[27] Czyszczenie kolejki drukarki
[28] Resetowanie Microsoft Store
[29] SFC (wstępny)
[30] DISM RestoreHealth
[31] SFC (końcowy)
[32] Czyszczenie logów CBS
[33] Usunięcie cache Windows Update (SoftwareDistribution)
[34] Usunięcie Windows.old
[35] Defragmentacja (HDD) / TRIM (SSD)
[36] Czyszczenie Thumbnail Cache
[37] Naprawa WMI
[38] Czyszczenie Minidump
[39] Czyszczenie WER
[40] Czyszczenie logów instalatorów
[41] Usunięcie starych sterowników PnP
[42] Generowanie raportu + historia sesji
[43] Opcjonalne skanowanie Malwarebytes
[44] Propozycja restartu systemu
```

---

## 📊 Raportowanie

Po każdym uruchomieniu skrypt generuje szczegółowy raport tekstowy zapisywany w:

```
%USERPROFILE%\Documents\CLEANER by MAG\Raport_YYYY-MM-DD_HH-mm-ss.txt
```

Raport zawiera:
- Datę i godzinę uruchomienia
- Informacje o sprzęcie (CPU, GPU, system Windows, dysk)
- Czas trwania optymalizacji
- Ilość zwolnionego miejsca na dysku (MB)
- Status każdego wykonanego zadania (OK / SKIP / BŁĄD)

Po zakończeniu konsola wyświetla **historię ostatnich 20 sesji** z wykresem słupkowym zwolnionego miejsca.

---

## 🔄 Aktualizacje automatyczne

Przy każdym uruchomieniu skrypt sprawdza dostępność nowej wersji poprzez GitHub API. Jeśli dostępna jest nowsza wersja, użytkownik może:
- Pobrać i uruchomić nową wersję automatycznie (jeden klik)
- Kontynuować z aktualną wersją

---

## 💛 Wsparcie projektu

Jeśli skrypt Ci pomógł, możesz postawić twórcy kawę:

👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

---

## 📄 Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

---

**Autor:** Mag | **Wersja:** 1.16 (05/05/2026)
