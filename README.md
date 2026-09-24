# CLEANER by MAG

![Wersja](https://img.shields.io/badge/wersja-1.36-blue)
![Platforma](https://img.shields.io/badge/platforma-Windows%2010%20%2F%2011-0078D6)
![Technologia](https://img.shields.io/badge/skrypt-Batch%20%2B%20PowerShell-lightgrey)

**Jednoplikowy skrypt `.bat` do głębokiego czyszczenia, naprawy, optymalizacji i zabezpieczania systemu Windows** — z automatyczną instalacją, samoaktualizacją, szczegółowymi raportami i opcjonalnymi skanerami antywirusowymi.

Skrypt działa w konsoli z własnym motywem kolorystycznym (turkus/biel/żółty) oraz pływającą nakładką pokazującą postęp poza oknem konsoli.

## Co nowego w wersji 1.36

Wydanie skupione na niezawodności — wiele kroków dokładniej sprawdza, czy operacja faktycznie się powiodła, zamiast zakładać sukces z góry.

- Nowy status **REST** (wymaga restartu): SFC i oczyszczanie bazy składników (DISM) wykrywają teraz oczekujący restart systemu i jasno o nim informują zamiast zgłaszać niejasny błąd. Dokładniejsze raportowanie objęło też Timer Resolution/HPET, CHKDSK, defragmentację, reset Windows Update, DNS i stare sterowniki drukarek (pokazuje liczbę usuniętych folderów).
- Plan zasilania „Wysoka wydajność” jest teraz sam odnajdywany/odblokowywany, jeśli jest ukryty (częste na laptopach/OEM) — wcześniej mogło to kończyć się cichym niepowodzeniem.
- Reset stosu IP jest pomijany, gdy wykryto statyczny adres IP — zapobiega utracie połączenia sieciowego. Konfiguracja DNS została przepisana na bardziej niezawodny mechanizm z osobną obsługą błędów dla każdego interfejsu.
- Wykrywanie uprawnień administratora poprawione (`fltmc` zamiast `net session`) — działa teraz poprawnie nawet gdy usługa Server jest wyłączona.
- Ponowne uruchamianie Eksploratora Windows wraca z normalnymi uprawnieniami zamiast dziedziczyć uprawnienia administratora ze skryptu; restart OneDrive po czyszczeniu cache ma teraz zapasową metodę przez Harmonogram zadań, jeśli standardowe uruchomienie się nie powiedzie.
- Przejmowanie własności folderu `Windows.old` naprawione na niektórych językowych wersjach Windows.
- Odczyt temperatur CPU/GPU: sterownik OpenHardwareMonitor (WinRing0) zastąpiony przez PawnIO, ponieważ WinRing0 bywał usuwany przez Windows Defender jako podatny sterownik.
- Instalacja winget (gdy go brakuje) jest bardziej niezawodna — obsługuje zależności dla właściwej architektury (x64/x86/ARM64) i ma zapasową metodę przez oficjalny moduł Microsoft.WinGet.Client.
- Mechanizm przywracania ustawień przy odinstalowaniu został gruntownie przebudowany — dokładnie liczy udane/nieudane/pominięte operacje i obejmuje więcej ustawień (BCD, TCP, DoH, plany zasilania, VSS, AppX), z zachowaniem kompatybilności ze stanem zapisanym w starszych wersjach.

## Spis treści

- [Co nowego w wersji 1.36](#co-nowego-w-wersji-136)
- [O programie](#o-programie)
- [Funkcje](#funkcje)
- [Wymagania](#wymagania)
- [Instalacja i pierwsze uruchomienie](#instalacja-i-pierwsze-uruchomienie)
- [Tryby pracy](#tryby-pracy)
- [Co dokładnie robi tryb Pełny](#co-dokładnie-robi-tryb-pełny)
- [Dodatkowe skany na żądanie](#dodatkowe-skany-na-żądanie)
- [Raporty i historia czyszczeń](#raporty-i-historia-czyszczeń)
- [Automatyczna aktualizacja](#automatyczna-aktualizacja)
- [Odinstalowanie](#odinstalowanie)
- [Bezpieczeństwo i uwagi](#bezpieczeństwo-i-uwagi)
- [Wsparcie projektu](#wsparcie-projektu)
- [Licencja](#licencja)
- [Autor](#autor)

## O programie

CLEANER by MAG to darmowe narzędzie do porządkowania i optymalizacji Windowsa, dystrybuowane jako pojedynczy plik `.bat` (bez instalatora `.exe`, bez zależności do ręcznego pobierania). Przy pierwszym uruchomieniu skrypt sam instaluje się w Menu Start, tworzy skróty i jest gotowy do ponownego użycia jednym kliknięciem.

## Funkcje

- **37 kroków** czyszcząco-naprawczo-optymalizacyjnych i zabezpieczających w jednym przebiegu (pełna lista niżej)
- 4 tryby pracy: Pełny / Szybki / Niestandardowy / Dodatkowe kroki
- Automatyczne sprawdzanie i pobieranie aktualizacji z GitHub Releases
- Tworzenie punktu przywracania systemu przed zmianami
- Wbudowane skanowanie Windows Defender i AdwCleaner
- Opcjonalne skanery na żądanie: **Malwarebytes** i **KVRT** (Kaspersky Virus Removal Tool) — pobierane, uruchamiane i sprzątane automatycznie
- Aktualizacja zainstalowanych programów przez **winget** (Menedżer pakietów Windows), z automatyczną instalacją winget, jeśli go brakuje
- Utwardzanie bezpieczeństwa: reguły ASR, ochrona LSA, zabezpieczenie SMB, blokada autoodtwarzania i makr Office, przywrócenie UAC, audyt persystencji i diagnostyka zdrowia systemu
- Debloat: usuwanie zbędnych aplikacji Windows, wyłączanie zbędnych funkcji, procesów w tle i programów startowych
- Strojenie sieci: DNS, DNS over HTTPS, parametry TCP oraz reset proxy
- Szczegółowy raport `.txt` po każdym przebiegu + wykres historii ostatnich 20 sesji w konsoli
- Pływająca nakładka z paskiem postępu i licznikiem kroków
- Blokada uśpienia komputera („keep-awake") na czas długich operacji
- Samoinstalacja w Menu Start (aplikacja, skrót do raportów, skrót do odinstalowania)
- Pełny deinstalator z opcją przywrócenia ustawień sprzed instalacji
- Dopasowanie okna konsoli (rozmiar, czcionka, kolory) do rozdzielczości ekranu

## Wymagania

- Windows 10 lub Windows 11
- Uprawnienia administratora (skrypt sam o nie poprosi, jeśli ich zabraknie)
- PowerShell (wbudowany w system — skrypt intensywnie z niego korzysta)
- Połączenie z internetem — potrzebne do sprawdzania aktualizacji, skanów AdwCleaner/Defender oraz pobierania Malwarebytes/KVRT

## Instalacja i pierwsze uruchomienie

1. Pobierz najnowszy plik `.bat` z zakładki [Releases](https://github.com/dineromagiero/CLEANER-by-MAG/releases/latest).
2. Kliknij plik prawym przyciskiem myszy → **Uruchom jako administrator**.
3. Przy pierwszym uruchomieniu skrypt automatycznie skopiuje się do Menu Start (`CLEANER by MAG`) i utworzy skróty: do aplikacji, do folderu raportów oraz do odinstalowania.
4. Kolejne uruchomienia wykonuj ze skrótu w Menu Start — skrypt sam sprawdzi, czy jest aktualny.

## Tryby pracy

| Skrót | Tryb | Opis |
|---|---|---|
| `P` | **Pełny** | wszystkie 37 kroków (zalecane) |
| `S` | **Szybki** | pomija antywirusy, SFC/DISM oraz czyszczenie starych sterowników |
| `N` | **Niestandardowy** | seria pytań T/N — sam wybierasz kategorie |
| `D` | **Dodatkowe kroki** | pomija czyszczenie i przechodzi od razu do Malwarebytes / KVRT / wsparcia projektu |

W trybie **Niestandardowym** decydujesz osobno o:
1. skanie Windows Defender + AdwCleaner,
2. naprawie systemu (SFC + DISM — najdłuższy etap),
3. czyszczeniu starych sterowników drukarek i PnP,
4. głębokim resecie Windows Update,
5. defragmentacji / TRIM dysku C:,
6. sprawdzaniu dysku C: (CHKDSK) — z pominięciem limitu „raz na 30 dni”,
7. odczycie temperatur CPU/GPU (pobiera dodatkowe narzędzie zewnętrzne).

Punkt przywracania systemu (krok 1) jest tworzony zawsze, niezależnie od tego, które kategorie zaznaczysz. Nowy blok utwardzania bezpieczeństwa (kroki 31–36) nie ma jeszcze osobnej kategorii — w trybie Niestandardowym jest zawsze pomijany.

## Co dokładnie robi tryb Pełny

<details>
<summary>Pełna lista 37 kroków (kliknij, aby rozwinąć)</summary>

Poniższe pozycje odpowiadają krokom pokazywanym w konsoli — część z nich wykonuje po kilka powiązanych czynności naraz.

1. Punkt przywracania systemu i ograniczenie miejsca na kopie w tle (VSS)
2. Skan Windows Defender
3. Skan AdwCleaner i reset ustawień proxy
4. Harmonogramy zadań, priorytety procesora i zarządzanie pamięcią
5. Analiza typu dysku (HDD/SSD) i mobilności urządzenia
6. Plan zasilania
7. Wyłączenie Fast Startup i dopasowanie pliku stronicowania do ilości RAM
8. Pliki tymczasowe, Downloaded Program Files, Minidump, raporty WER, logi instalatorów, folder Prefetch (tylko HDD) oraz cache GPU we wszystkich profilach użytkowników
9. Sieć, DNS (Cloudflare/Google), DNS over HTTPS i parametry TCP
10. Dzienniki zdarzeń i Oczyszczanie dysku
11. Sprawdzanie dysku przy następnym uruchomieniu (CHKDSK)
12. Optymalizacja usług systemowych
13. Timer Resolution / HPET (obniżenie latencji)
14. Optymalizacja rejestru i wyłączenie Narratora
15. Zbędne aplikacje Windows, aktywne godziny (8–23) i procesy w tle
16. Cache przeglądarek
17. Cache Microsoft Teams
18. Cache OneDrive
19. Cache Java / .NET oraz kolejka drukarki (Spooler)
20. Czyszczenie starych sterowników drukarek
21. Usuwanie przestarzałych sterowników PnP
22. Cache Microsoft Store
23. Optymalizacja dostarczania aktualizacji, wyłączenie śledzenia/sugestii, blokowanie reklam i cichych instalacji oraz wyłączenie historii aktywności, Recall i Copilot
24. SFC — skan wstępny
25. DISM RestoreHealth (naprawa obrazu systemu)
26. SFC — skan końcowy i oczyszczenie bazy składników
27. Głęboka naprawa i reset Windows Update
28. Optymalizacja Funkcji Windows i logi CBS
29. Trwałe usunięcie folderu `Windows.old` (jeśli istnieje)
30. Defragmentacja / TRIM dysku C: (dopasowane do typu dysku)
31. Utwardzanie Windows Defender i reguły ASR (Attack Surface Reduction)
32. Utwardzanie systemu, kont i protokołów — ochrona LSA/poświadczeń, wyłączenie LLMNR/NetBIOS/WPAD, zabezpieczenie SMB, blokada autoodtwarzania, przywrócenie UAC do najwyższego poziomu, blokada makr Office pobranych z internetu, widoczność rozszerzeń plików
33. Optymalizacja GPU (planowanie sprzętowe, Game DVR), sieci i systemu plików NTFS
34. Konfiguracja Czujnika pamięci (Storage Sense) i cyklicznego zadania konserwacji
35. Audyt persystencji i bezpieczeństwa (tylko raportuje, niczego nie usuwa)
36. Diagnostyka zdrowia systemu
37. Zbędne programy startowe, cache ikon/miniatur/czcionek, naprawa Eksploratora Windows i baza WMI

</details>

## Dodatkowe skany na żądanie

Dostępne z menu głównego (na końcu trybu Pełnego/Szybkiego/Niestandardowego) lub bezpośrednio przez opcję **[D] Dodatkowe kroki**:

- **Malwarebytes** — pobiera instalator, instaluje program, uruchamia go do ręcznego skanu, a po zamknięciu okna automatycznie odinstalowuje program (jeśli nie był już wcześniej zainstalowany — wtedy skrypt po prostu go otwiera).
- **Aktualizacja programów przez winget** — sprawdza, czy Menedżer pakietów Windows (winget) jest dostępny; jeśli go brakuje (np. na edycjach LTSC), oferuje instalację nieoficjalnym sposobem — pobranie najnowszego wydania bezpośrednio z repozytorium `microsoft/winget-cli` na GitHubie. Następnie aktualizuje po kolei wszystkie zainstalowane programy, z podglądem postępu pobierania na żywo.
- **KVRT (Kaspersky Virus Removal Tool)** — pobiera najnowszą wersję i uruchamia w pełni automatyczny skan w tle, a po zakończeniu sam się usuwa wraz z danymi tymczasowymi (folder `KVRT_Data` jest sprzątany, chyba że na komputerze wykryto zainstalowany pełny produkt Kaspersky — wtedy zostaje nietknięty).

> ⚠️ **Uwaga:** KVRT może w trakcie skanowania wykryć poważne zagrożenie i **zrestartować komputer bez pytania o zgodę**. Zapisz pracę przed uruchomieniem tej opcji — skrypt blokuje nieoczekiwany restart tylko na czas własnego działania.

## Raporty i historia czyszczeń

- Po każdym przebiegu w trybie Pełnym lub Szybkim zapisywany jest raport tekstowy:
  `Dokumenty\CLEANER by MAG\Raport_<data>_<godzina>.txt`
- Raport zawiera m.in.: datę i tryb, czas trwania, ilość zwolnionego miejsca na dysku C:, dane sprzętowe (CPU/GPU/dysk), wersję i build Windows oraz status każdej operacji (`OK` / `SKIP` / `BŁĄD` / `REST` — wymaga restartu). Jeśli krok 35 (audyt) znajdzie pozycje do przejrzenia, ich lista trafia do tego samego raportu.
- Skrót **„Raporty"** w Menu Start prowadzi bezpośrednio do tego folderu.
- Na koniec działania skrypt pokazuje w konsoli wykres słupkowy (ASCII) z historii ostatnich 20 sesji — ile miejsca zwalniano i ile trwało czyszczenie.

## Automatyczna aktualizacja

- Przy każdym starcie skrypt porównuje swoją wersję z najnowszym wydaniem w repozytorium [`dineromagiero/CLEANER-by-MAG`](https://github.com/dineromagiero/CLEANER-by-MAG/releases/latest).
- Jeśli dostępna jest nowsza wersja, proponuje automatyczne pobranie i podmianę pliku `.bat`, po czym sam uruchamia nową wersję.
- Integralność pobranego pliku jest sprawdzana sumą kontrolną SHA-256 (gdy wydanie ją udostępnia) — przy niezgodności aktualizacja zostaje przerwana, a skrypt otwiera stronę Releases zamiast uruchomić niezweryfikowany plik.
- Brak połączenia z internetem nie blokuje działania — sprawdzanie aktualizacji jest po prostu pomijane.

## Odinstalowanie

Użyj skrótu **„Odinstaluj CLEANER by MAG"** w Menu Start (wymaga uprawnień administratora). Proces zapyta dodatkowo o:

- usunięcie zapisanych raportów,
- przywrócenie oryginalnych ustawień systemowych sprzed instalacji (DNS, usługi, rejestr, HPET itd.) na podstawie stanu zapisanego przy pierwszym pełnym uruchomieniu — zaplanowane zadania wracają do swojego dokładnego stanu sprzed zmian (Włączone/Wyłączone), a nie są po prostu masowo włączane.

Po odinstalowaniu usuwane są też pozostałe wpisy w Harmonogramie zadań i w rejestrze dodane przez skrypt.

## Bezpieczeństwo i uwagi

- Skrypt wymaga uprawnień administratora i modyfikuje ustawienia systemowe (rejestr, usługi, DNS, harmonogram zadań, ustawienia zegara/BCD, plik stronicowania).
- Blok utwardzania bezpieczeństwa (kroki 31–32) zmienia też ustawienia sieciowe (SMB, LLMNR/NetBIOS/WPAD) i UAC — w rzadkich przypadkach (stare drukarki lub udziały sieciowe w lokalnej sieci) może to wymagać ręcznej korekty.
- Przed pierwszą pełną optymalizacją tworzony jest **punkt przywracania systemu**, a stan kluczowych ustawień jest zapisywany do ewentualnego przywrócenia przy odinstalowaniu.
- Tryb Pełny i Szybki **zamykają otwarte okna przeglądarki** oraz **trwale usuwają folder `Windows.old`**, jeśli istnieje — skrypt prosi o potwierdzenie przed rozpoczęciem.
- Skrypt nie jest podpisany cyfrowo — Windows SmartScreen/Defender może wyświetlić ostrzeżenie przy pierwszym uruchomieniu. Pobieraj go wyłącznie z oficjalnego repozytorium.
- Zamknij ważne, niezapisane prace przed uruchomieniem — zwłaszcza przed skanem KVRT (patrz wyżej) i przed ewentualnym restartem na końcu przebiegu.

## Wsparcie projektu

Na końcu działania skrypt pyta, czy chcesz postawić autorowi symboliczną kawę:
👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

## Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

## Autor

Autor: **MAG** | Wersja: **1.36** (23/09/2026)
