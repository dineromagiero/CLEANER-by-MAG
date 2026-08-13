# CLEANER by MAG

![Wersja](https://img.shields.io/badge/wersja-1.30-blue)
![Platforma](https://img.shields.io/badge/platforma-Windows%2010%20%2F%2011-0078D6)
![Technologia](https://img.shields.io/badge/skrypt-Batch%20%2B%20PowerShell-lightgrey)

**Jednoplikowy skrypt `.bat` do głębokiego czyszczenia, naprawy i optymalizacji systemu Windows** — z automatyczną instalacją, samoaktualizacją, szczegółowymi raportami i opcjonalnymi skanerami antywirusowymi.

Skrypt działa w konsoli z własnym motywem kolorystycznym (turkus/biel/żółty) oraz pływającą nakładką pokazującą postęp poza oknem konsoli.

## Spis treści

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

- **31 operacji czyszcząco-naprawczych** w jednym przebiegu (pełna lista niżej)
- 4 tryby pracy: Pełny / Szybki / Niestandardowy / Dodatkowe kroki
- Automatyczne sprawdzanie i pobieranie aktualizacji z GitHub Releases
- Tworzenie punktu przywracania systemu przed zmianami
- Wbudowane skanowanie Windows Defender i AdwCleaner
- Opcjonalne skanery na żądanie: **Malwarebytes** i **KVRT** (Kaspersky Virus Removal Tool) — pobierane, uruchamiane i sprzątane automatycznie
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
| `P` | **Pełny** | wszystkie 31 operacji (zalecane) |
| `S` | **Szybki** | pomija antywirusy, SFC/DISM oraz czyszczenie starych sterowników |
| `N` | **Niestandardowy** | seria pytań T/N — sam wybierasz kategorie |
| `D` | **Dodatkowe kroki** | pomija czyszczenie i przechodzi od razu do Malwarebytes / KVRT / wsparcia projektu |

W trybie **Niestandardowym** decydujesz osobno o:
1. skanie Windows Defender + AdwCleaner,
2. naprawie systemu (SFC + DISM — najdłuższy etap),
3. czyszczeniu starych sterowników drukarek i PnP,
4. głębokim resecie Windows Update,
5. defragmentacji / TRIM dysku C:,
6. odczycie temperatur CPU/GPU (pobiera dodatkowe narzędzie zewnętrzne).
7. 
## Co dokładnie robi tryb Pełny

<details>
<summary>Pełna lista 31 operacji (kliknij, aby rozwinąć)</summary>
  
**Bezpieczeństwo**
 
1. Punkt przywracania systemu
2. Skan Windows Defender
3. Skan AdwCleaner
4. Reset proxy

**Czyszczenie plików i pamięci podręcznej**
 
5. Pliki tymczasowe (użytkownika i systemu)
6. Prefetch (tylko na dyskach HDD — na SSD pomijane)
7. Cache przeglądarek
8. Cache Microsoft Teams
9. Cache OneDrive
10. Cache Microsoft Store
11. Thumbnail Cache (miniatury)
12. Pliki Minidump
13. Raporty błędów Windows (WER)
14. Logi instalatorów (CBS/DISM/NetSetup itd.)
15. Kolejka drukarki
16. Cache Windows Update
17. Trwałe usunięcie folderu `Windows.old` (jeśli istnieje)

**Naprawa systemu**
 
18. SFC — skan wstępny
19. DISM RestoreHealth
20. SFC — skan końcowy
21. Logi CBS
22. Naprawa WMI
23. Dzienniki zdarzeń

**Wydajność i konfiguracja**
 
24. Optymalizacja usług systemowych
25. Optymalizacja rejestru
26. Reset DNS / sieci
27. Defragmentacja / TRIM (dopasowane do typu dysku)
28. Wyłączenie Fast Startup + dopasowanie pliku stronicowania do ilości RAM
29. Optymalizacja Timer Resolution / HPET (obniżenie latencji)
30. Wyłączenie automatycznego uruchamiania Narratora (skrót ułatwień dostępu)
31. Usunięcie starych/nieużywanych sterowników PnP i drukarek
</details>

## Dodatkowe skany na żądanie

Dostępne z menu głównego (tryb Pełny/Szybki/Niestandardowy na końcu) lub bezpośrednio przez opcję **[D] Dodatkowe kroki**:

- **Malwarebytes** — pobiera instalator, instaluje program, uruchamia go do ręcznego skanu, a po zamknięciu okna automatycznie odinstalowuje program (jeśli nie był już wcześniej zainstalowany — wtedy skrypt po prostu go otwiera).
- **KVRT (Kaspersky Virus Removal Tool)** — pobiera najnowszą wersję i uruchamia w pełni automatyczny skan w tle, a po zakończeniu sam się usuwa wraz z danymi tymczasowymi.

> ⚠️ **Uwaga:** KVRT może w trakcie skanowania wykryć poważne zagrożenie i **zrestartować komputer bez pytania o zgodę**. Zapisz pracę przed uruchomieniem tej opcji — skrypt blokuje nieoczekiwany restart tylko na czas własnego działania.

## Raporty i historia czyszczeń

- Po każdym przebiegu w trybie Pełnym lub Szybkim zapisywany jest raport tekstowy:
  `Dokumenty\CLEANER by MAG\Raport_<data>_<godzina>.txt`
- Raport zawiera m.in.: datę i tryb, czas trwania, ilość zwolnionego miejsca na dysku C:, dane sprzętowe (CPU/GPU/dysk), wersję i build Windows oraz status każdej operacji (`OK` / `SKIP` / `BŁĄD`).
- Skrót **„Raporty"** w Menu Start prowadzi bezpośrednio do tego folderu.
- Na koniec działania skrypt pokazuje w konsoli wykres słupkowy (ASCII) z historii ostatnich 20 sesji — ile miejsca zwalniano i ile trwało czyszczenie.

## Automatyczna aktualizacja

- Przy każdym starcie skrypt porównuje swoją wersję z najnowszym wydaniem w repozytorium [`dineromagiero/CLEANER-by-MAG`](https://github.com/dineromagiero/CLEANER-by-MAG/releases/latest).
- Jeśli dostępna jest nowsza wersja, proponuje automatyczne pobranie i podmianę pliku `.bat`, po czym sam uruchamia nową wersję.
- Brak połączenia z internetem nie blokuje działania — sprawdzanie aktualizacji jest po prostu pomijane.

## Odinstalowanie

Użyj skrótu **„Odinstaluj CLEANER by MAG"** w Menu Start (wymaga uprawnień administratora). Proces zapyta dodatkowo o:

- usunięcie zapisanych raportów,
- przywrócenie oryginalnych ustawień systemowych sprzed instalacji (DNS, usługi, rejestr, HPET itd.) na podstawie stanu zapisanego przy pierwszym pełnym uruchomieniu.

Po odinstalowaniu usuwane są też wpisy w Harmonogramie zadań i w rejestrze dodane przez skrypt.

## Bezpieczeństwo i uwagi

- Skrypt wymaga uprawnień administratora i modyfikuje ustawienia systemowe (rejestr, usługi, DNS, harmonogram zadań, ustawienia zegara/BCD, plik stronicowania).
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

Autor: **MAG** | Wersja: **1.30** (13/08/2026)
