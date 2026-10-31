# <img width="40" height="40" alt="CbM_Logo" src="https://github.com/user-attachments/assets/a047f7d7-4f88-4f7f-878b-cdf53fc4ca23" /> CLEANER by MAG

![Wersja](https://img.shields.io/badge/wersja-2.0-blue)
![Platforma](https://img.shields.io/badge/platforma-Windows%2010%20%2F%2011-0078D6)
![Technologia](https://img.shields.io/badge/aplikacja-Python%20%2B%20Qt%20%2B%20PowerShell-lightgrey)

**Aplikacja do głębokiego czyszczenia, naprawy, optymalizacji i zabezpieczania systemu Windows** — z przejrzystym oknem zamiast konsoli, samoaktualizacją, raportami otwieranymi w programie i opcjonalnymi skanerami antywirusowymi. Dostarczana jako jeden plik `.exe`.

Za całe czyszczenie odpowiada ten sam sprawdzony silnik co w wersjach 1.x, więc kroki i ich kolejność pozostają takie, jakie znasz. Zmienia się to, jak program wygląda i jak się go obsługuje.

## Co nowego w wersji 2.0

Duży update: CLEANER by MAG przestaje być skryptem `.bat` w oknie konsoli i staje się pełnoprawną aplikacją z własnym oknem.

- **Nowy interfejs graficzny**: ciemne okno z paskiem bocznym (wybór trybu, Opcje dodatkowe, Postęp, Raporty, Odinstaluj, O programie), pierścieniem postępu, listą wszystkich etapów i licznikiem „Krok k z N”.
- **Jeden plik `.exe`**: silnik, skrypty pomocnicze i grafiki są w środku — nic nie trzeba rozpakowywać ani pobierać osobno.
- **Uwagi i podpowiedzi**: gdy coś wymaga uwagi, dostajesz opis problemu i wskazówkę, co zrobić, zamiast surowej linii z dziennika. Lista pozycji z audytu bezpieczeństwa otwiera się w oknie programu i można ją skopiować.
- **Raporty i dziennik w programie**: raport HTML, raport TXT i plik diagnostyczny otwierają się w oknie aplikacji, bez przeglądarki i Notatnika.
- **Opcje dodatkowe na osobnej stronie**: Malwarebytes, KVRT i aktualizację programów (winget) uruchamiasz osobno, w dowolnej chwili, jednym kliknięciem.
- **Karty etapów skanów i kafelki programów**: widać, który skan lub program jest w kolejce, w trakcie, gotowy, pominięty albo zakończony błędem — wraz z powodem pominięcia.
- **Samoaktualizacja**: program sam sprawdza GitHub Releases, pobiera nowy `.exe`, podmienia się i uruchamia ponownie.
- **Szybszy start czyszczenia**: silnik rozgrzewa się już przy starcie aplikacji i wykonuje przygotowania niezależne od trybu.
- **Odinstalowanie z postępem na żywo** i limitami czasu dla operacji, które wcześniej potrafiły się zawiesić.
- **Wygodniejsze zamknięcie i restart**: przycisk restartu na ekranie podsumowania oraz pytanie przy zamykaniu aplikacji.

Pełna lista zmian jest w pliku `CHANGELOG`.

## Spis treści

- [Co nowego w wersji 2.0](#co-nowego-w-wersji-20)
- [O programie](#o-programie)
- [Funkcje](#funkcje)
- [Wymagania](#wymagania)
- [Instalacja i pierwsze uruchomienie](#instalacja-i-pierwsze-uruchomienie)
- [Jak wygląda praca z programem](#jak-wygląda-praca-z-programem)
- [Tryby pracy](#tryby-pracy)
- [Co dokładnie robi tryb Pełny](#co-dokładnie-robi-tryb-pełny)
- [Opcje dodatkowe](#opcje-dodatkowe)
- [Raporty i historia czyszczeń](#raporty-i-historia-czyszczeń)
- [Automatyczna aktualizacja](#automatyczna-aktualizacja)
- [Odinstalowanie](#odinstalowanie)
- [Bezpieczeństwo i uwagi](#bezpieczeństwo-i-uwagi)
- [Wsparcie projektu](#wsparcie-projektu)
- [Licencja](#licencja)
- [Autor](#autor)

## O programie

CLEANER by MAG to darmowe narzędzie do porządkowania i optymalizacji Windowsa, dystrybuowane jako pojedynczy plik `.exe` (bez instalatora i bez zależności do ręcznego pobierania — nie potrzebujesz Pythona). Przy pierwszym uruchomieniu program sam tworzy wpisy w Menu Start i skróty, dzięki czemu kolejne uruchomienia są jednym kliknięciem.

## Funkcje

- **37 kroków** czyszcząco-naprawczo-optymalizacyjnych i zabezpieczających w jednym przebiegu (pełna lista niżej)
- Graficzny interfejs z postępem krok po kroku, pierścieniem postępu i kartami uwag
- 3 tryby czyszczenia (Pełny / Szybki / Własny) oraz osobna strona Opcji dodatkowych
- Automatyczne sprawdzanie i pobieranie aktualizacji z GitHub Releases, z samopodmianą pliku `.exe`
- Tworzenie punktu przywracania systemu przed zmianami
- Wbudowane skanowanie Windows Defender i AdwCleaner
- Opcjonalne skanery na żądanie: **Malwarebytes** i **KVRT** (Kaspersky Virus Removal Tool) — pobierane, uruchamiane i sprzątane automatycznie
- Aktualizacja zainstalowanych programów przez **winget** (Menedżer pakietów Windows), z automatyczną instalacją winget, jeśli go brakuje, i powodem pominięcia przy każdym programie, którego nie udało się zaktualizować
- Utwardzanie bezpieczeństwa: reguły ASR, ochrona LSA, zabezpieczenie SMB, blokada autoodtwarzania i makr Office, przywrócenie UAC, audyt persystencji i diagnostyka zdrowia systemu
- Debloat: usuwanie zbędnych aplikacji Windows, wyłączanie zbędnych funkcji, procesów w tle i programów startowych
- Strojenie sieci: DNS, DNS over HTTPS, parametry TCP oraz reset proxy
- Czytelne podpowiedzi przy ponad 60 typowych problemach (brak internetu, wyłączona ochrona systemu, blokada przez antywirus, błędy SFC/DISM i wiele innych)
- Szczegółowy raport HTML (z awaryjnym trybem `.txt`) po każdym przebiegu, otwierany w programie, z dołączonym raportem wydajności energetycznej Windows (a na laptopach też raportem baterii i uśpienia)
- Odczyt temperatur procesora i karty graficznej z oceną kolorystyczną na ekranie wyniku
- Pływająca nakładka z licznikiem kroków, widoczna, gdy okno programu jest zminimalizowane lub nie ma fokusu
- Blokada uśpienia komputera („keep-awake") na czas długich operacji
- Samoinstalacja w Menu Start (aplikacja, skrót do raportów, skrót do odinstalowania)
- Pełny deinstalator z opcją przywrócenia ustawień sprzed instalacji
- Okno dopasowujące się do rozdzielczości i skalowania ekranu (m.in. 1920x1080 przy 125%)

## Wymagania

- Windows 10 lub Windows 11
- Uprawnienia administratora (program sam o nie poprosi przez okno UAC, jeśli ich zabraknie)
- PowerShell (wbudowany w system — silnik intensywnie z niego korzysta)
- Połączenie z internetem — potrzebne do sprawdzania aktualizacji, skanów AdwCleaner/Defender oraz pobierania Malwarebytes/KVRT

## Instalacja i pierwsze uruchomienie

1. Pobierz najnowszy plik `.exe` z zakładki [Releases](https://github.com/dineromagiero/CLEANER-by-MAG/releases/latest) i zapisz go w miejscu, w którym ma zostać na stałe.
2. Uruchom go — jeśli program nie ma uprawnień administratora, pokaże przycisk **Uruchom jako administrator** i poprosi o potwierdzenie w oknie UAC (albo kliknij plik prawym przyciskiem myszy → **Uruchom jako administrator**).
3. Przy pierwszym uruchomieniu zobaczysz krótkie okno powitalne z opisem nowej odsłony (pokazuje się tylko raz). Program tworzy wpisy w Menu Start (`CLEANER by MAG`): skrót do aplikacji, do folderu raportów oraz do odinstalowania.
4. Kolejne uruchomienia wykonuj ze skrótu w Menu Start — program sam sprawdzi, czy jest aktualny. Skróty wskazują na plik `.exe`, więc nie przenoś go ani nie usuwaj po instalacji.

## Jak wygląda praca z programem

1. **Wybierz tryb** na ekranie startowym (karty: Pełny, Szybki, Własny, Opcje dodatkowe). Przed startem program przypomina, że zostaną zamknięte przeglądarki, a folder `Windows.old` (jeśli istnieje) zostanie trwale usunięty.
2. **Obserwuj postęp** na stronie „Postęp": pierścień pokazuje ogólny postęp, lista pokazuje wszystkie etapy i ich stany, a licznik — „Krok k z N". Możesz przerwać operację przyciskiem „Przerwij", ale przerwanie w trakcie pracy może zostawić część ustawień w stanie pośrednim (np. usługi Windows Update), więc zalecane jest dokończenie.
3. **Przeczytaj wynik**: ekran „Wszystko gotowe" lub „Zakończono z uwagami" pokazuje wykonane i pominięte operacje, elementy wymagające restartu, błędy, czas oraz temperatury. Zakładka „Uwagi i podpowiedzi" tłumaczy, co oznacza każdy problem i co z nim zrobić.
4. **Zrestartuj komputer** przyciskiem na ekranie podsumowania, jeśli program to zaleca (czyszczenie zmieniło system albo Windows czeka na restart).

Podczas operacji pozostałe pozycje menu są zablokowane, a pasek boczny pokazuje, że operacja trwa. Przycisk „Dziennik" i plik diagnostyczny pozwalają zajrzeć w szczegóły techniczne bez opuszczania programu.

## Tryby pracy

| Tryb | Opis |
|---|---|
| **Pełny** (zalecany) | wszystkie 37 kroków, plus odczyt temperatur na końcu |
| **Szybki** (na co dzień) | pomija skany antywirusowe, naprawę systemu (DISM/SFC), czyszczenie starych sterowników, naprawę Windows Update, optymalizację dysku C: i odczyt temperatur |
| **Własny** (ty decydujesz) | wykonuje wyłącznie zaznaczone kategorie (lista niżej) — na stronie postępu widzisz tylko je |
| **Opcje dodatkowe** | osobna strona z Malwarebytes, KVRT i aktualizacją programów, uruchamiana niezależnie od czyszczenia |

W trybie **Własnym** zaznaczasz przełącznikami (są przyciski „Zaznacz wszystko" i „Wyczyść"):
1. skan antywirusowy (Windows Defender + AdwCleaner),
2. naprawę systemu (SFC + DISM — najdłuższy etap),
3. czyszczenie starych sterowników drukarek i urządzeń,
4. naprawę Windows Update,
5. optymalizację dysku C: (defragmentacja lub TRIM, zależnie od typu dysku),
6. sprawdzenie dysku C: (CHKDSK) — z pominięciem limitu „raz na 30 dni",
7. odczyt temperatur procesora i karty graficznej (pobiera małe narzędzie zewnętrzne, bez instalacji sterownika).

Punkt przywracania systemu (krok 1) jest tworzony, gdy wybierzesz cokolwiek poza samym odczytem temperatur — sam odczyt niczego nie zmienia, więc punkt jest wtedy zbędny i pomijany. Blok utwardzania bezpieczeństwa i końcowych porządków (kroki 31–37) nie ma jeszcze osobnej kategorii — w trybie Własnym jest zawsze pomijany.

## Co dokładnie robi tryb Pełny

<details>
<summary>Pełna lista 37 kroków (kliknij, aby rozwinąć)</summary>

Poniższe pozycje odpowiadają krokom pokazywanym na liście postępu — część z nich wykonuje po kilka powiązanych czynności naraz.

1. Punkt przywracania systemu i ograniczenie miejsca na kopie w tle (VSS)
2. Skan Windows Defender
3. Skan AdwCleaner i reset ustawień proxy
4. Harmonogramy zadań, zadania w tle, priorytety procesora i zarządzanie pamięcią
5. Rozpoznanie typu dysku (HDD/SSD)
6. Rozpoznanie urządzenia (laptop / PC) i plan zasilania
7. Wyłączenie Fast Startup i dopasowanie pliku stronicowania do ilości RAM
8. Pliki tymczasowe, Downloaded Program Files, Minidump, raporty WER, logi instalatorów, folder Prefetch (tylko HDD) oraz cache GPU we wszystkich profilach użytkowników
9. Sieć, DNS (Cloudflare/Google), DNS over HTTPS i parametry TCP
10. Dzienniki zdarzeń i Oczyszczanie dysku
11. Sprawdzenie dysku C: przy następnym uruchomieniu (CHKDSK)
12. Optymalizacja usług systemowych
13. Timer Resolution / HPET (obniżenie latencji)
14. Optymalizacja rejestru i wyłączenie Narratora
15. Zbędne aplikacje Windows, aktywne godziny (8–23), procesy w tle i autostart
16. Cache przeglądarek i aplikacji
17. Cache Microsoft Teams
18. Cache OneDrive
19. Cache Java / .NET oraz kolejka drukarki (Spooler)
20. Czyszczenie starych sterowników drukarek
21. Usuwanie przestarzałych sterowników PnP
22. Cache Microsoft Store
23. Optymalizacja dostarczania aktualizacji, wyłączenie śledzenia i sugestii, blokowanie reklam i cichych instalacji, wyłączenie historii aktywności, Recall i Copilot oraz wstępne sprawdzenie plików systemowych (SFC)
24. DISM RestoreHealth (naprawa obrazu systemu)
25. SFC — kontrola plików systemowych
26. Oczyszczenie bazy składników (porządki w WinSxS)
27. Głęboka naprawa i reset Windows Update
28. Optymalizacja Funkcji Windows i logi CBS
29. Trwałe usunięcie folderu `Windows.old` (jeśli istnieje)
30. Defragmentacja / TRIM dysku C: (dopasowane do typu dysku)
31. Utwardzanie Windows Defender i reguły ASR (Attack Surface Reduction)
32. Utwardzanie systemu, kont i protokołów — ochrona LSA/poświadczeń, wyłączenie LLMNR/NetBIOS/WPAD, zabezpieczenie SMB, blokada autoodtwarzania, przywrócenie UAC do najwyższego poziomu, blokada makr Office pobranych z internetu, widoczność rozszerzeń plików
33. Optymalizacja karty graficznej (planowanie sprzętowe, Game DVR), dysku (NTFS), sieci i USB
34. Konfiguracja Czujnika pamięci (Storage Sense) i cyklicznego zadania konserwacji
35. Kontrola bezpieczeństwa — audyt persystencji i bezpieczeństwa (tylko raportuje, niczego nie usuwa)
36. Diagnostyka stanu systemu (na laptopach dodatkowo raport kondycji baterii i raport uśpienia, dołączane do raportu końcowego)
37. Finalizacja i raport — zbędne programy startowe, cache ikon/miniatur/czcionek, naprawa Eksploratora Windows, baza WMI oraz zapis raportu

Po kroku 37 tryb Pełny wykonuje jeszcze odczyt temperatur procesora i karty graficznej (wynik trafia na ekran podsumowania i do raportu).

</details>

## Opcje dodatkowe

Dostępne na osobnej stronie „Opcje dodatkowe" (w dowolnej chwili, bez czyszczenia systemu) oraz jako propozycja po zakończeniu czyszczenia — wynik widzisz wtedy od razu, a pytanie o przejście dalej pojawia się dopiero na ekranie podsumowania. Kliknięcie „Uruchom" przy wybranej opcji startuje właśnie tę jedną opcję, bez dalszych pytań.

- **Malwarebytes** (obsługa ręczna) — pobiera instalator, instaluje program i otwiera go do ręcznego skanu, a po zamknięciu okna automatycznie go odinstalowuje. Jeśli Malwarebytes był już zainstalowany, program po prostu go otwiera (niczego nie pobiera ani nie usuwa). Gdy proces po zamknięciu okna nadal wisi w tle, możesz użyć przycisku **„Wymuś zamknięcie"** (program próbuje kolejno kilku metod, w tym zamknięcia przez ikonę w zasobniku) albo skorzystać z instrukcji „Jak zamknąć ręcznie".
- **Aktualizacja programów przez winget** — sprawdza, czy Menedżer pakietów Windows (winget) jest dostępny; jeśli go brakuje (np. na edycjach LTSC), oferuje instalację nieoficjalnym sposobem — pobranie najnowszego wydania bezpośrednio z repozytorium `microsoft/winget-cli` na GitHubie. Następnie aktualizuje po kolei wszystkie zainstalowane programy. Każdy program ma własny kafelek (w kolejce → pobieranie → instalowanie → gotowe) z wersją obecną i dostępną oraz paskiem pobierania, w którym procent jest odczytywany z usługi Delivery Optimization (a gdy jest niedostępna — z rozmiaru pobieranego pliku). Jeśli program nie zostanie zaktualizowany, kafelek wyjaśnia dlaczego (np. program jest uruchomiony, brak pasującej aktualizacji, wymagana ręczna reinstalacja albo błąd pobierania).
- **KVRT (Kaspersky Virus Removal Tool)** — pobiera najnowszą wersję i uruchamia w pełni automatyczny skan w tle, a po zakończeniu sam się usuwa wraz z danymi tymczasowymi (folder `KVRT_Data` jest sprzątany, chyba że na komputerze wykryto zainstalowany pełny produkt Kaspersky — wtedy zostaje nietknięty). Skan, który nie zakończy się w ciągu 90 minut, jest przerywany, a wynik skanu można obejrzeć przyciskiem „Pokaż wynik skanu".

> ⚠️ **Uwaga:** KVRT może w trakcie skanowania wykryć poważne zagrożenie i **zrestartować komputer bez pytania o zgodę**. Zapisz pracę przed uruchomieniem tej opcji — program blokuje nieoczekiwany restart tylko na czas własnego działania i ostrzega, jeśli KVRT próbował go wymusić.

## Raporty i historia czyszczeń

- Po każdym przebiegu zapisywany jest raport:
  `Dokumenty\CLEANER by MAG\Raport_<data>_<godzina>.html` — czytelny, stylizowany raport HTML. Jeśli jego wygenerowanie się nie powiedzie, program automatycznie zapisuje w zamian awaryjną wersję `Raport_<data>_<godzina>.txt`.
- Strona **„Raporty"** w programie pokazuje listę wszystkich zapisanych raportów (data i godzina, rozmiar) z przyciskami „Otwórz", „Wersja TXT" i „Otwórz folder". Raport otwiera się w oknie aplikacji; z podglądu można go nadal otworzyć w przeglądarce.
- Raport zawiera m.in.: datę i tryb, czas trwania, ilość zwolnionego miejsca na dysku C:, dane sprzętowe (CPU/GPU/dysk), wersję i build Windows oraz status każdej operacji (`OK` / `SKIP` / `BŁĄD` / `REST` — wymaga restartu). Jeśli krok 35 (audyt) znajdzie pozycje do przejrzenia, ich lista trafia do tego samego raportu, a w programie otwierasz ją przyciskiem „Pokaż listę".
- Na końcu raportu dołączany jest raport wydajności energetycznej Windows (`powercfg /energy`), generowany dopiero po zakończeniu pracy i przywróceniu oryginalnych ustawień uśpienia/wygaszania ekranu, dzięki czemu odzwierciedla rzeczywisty stan systemu. Na laptopach dołączane są też raport kondycji baterii i raport uśpienia.
- Skrót **„Raporty"** w Menu Start prowadzi bezpośrednio do folderu z raportami.
- Silnik nadal generuje historię ostatnich 20 sesji (ile miejsca zwalniano i ile trwało czyszczenie) na podstawie zapisanych raportów.
- **Plik diagnostyczny** (`.devlog.txt`) otwierasz w programie przyciskiem „Plik diagnostyczny". Jest zapisywany podczas czyszczenia i kasowany przy starcie następnego przebiegu, więc zawsze dotyczy ostatniej operacji.

## Automatyczna aktualizacja

- Program sprawdza w tle, czy w repozytorium [`dineromagiero/CLEANER-by-MAG`](https://github.com/dineromagiero/CLEANER-by-MAG/releases/latest) jest nowsze wydanie. Wskaźnik w lewym dolnym rogu paska bocznego pokazuje stan: łączenie, brak połączenia, najnowsza wersja albo dostępna aktualizacja.
- Gdy jest nowsza wersja, program pyta, czy ją zainstalować. Przy „Tak" pobiera nowy plik `.exe` z paskiem postępu, zamyka się, podmienia plik i uruchamia nową wersję. Wybierając „Nie", możesz zaktualizować później, klikając wskaźnik aktualizacji.
- Jeśli samodzielna podmiana nie jest możliwa (np. wydanie zawiera tylko archiwum `.zip`), plik zostaje zapisany w folderze „Pobrane" albo otwierana jest strona wydania.
- Pobieranie jest dozwolone wyłącznie z domen `github.com` i `githubusercontent.com`.
- Brak połączenia z internetem nie blokuje działania — sprawdzanie aktualizacji jest po prostu pomijane, a wskaźnik pozwala spróbować ponownie.

## Odinstalowanie

Użyj skrótu **„Odinstaluj CLEANER by MAG"** w Menu Start (wymaga uprawnień administratora) albo uruchom program z parametrem `--uninstall`. Otworzy się strona odinstalowania, a program zapyta o:

- usunięcie zapisanych raportów,
- przywrócenie oryginalnych ustawień systemowych sprzed instalacji (DNS, usługi, rejestr, HPET itd.) na podstawie stanu zapisanego przy pierwszym pełnym uruchomieniu — zaplanowane zadania wracają do swojego dokładnego stanu sprzed zmian (Włączone/Wyłączone), a nie są po prostu masowo włączane.

Przywracanie pokazuje postęp na żywo („Etap k z 17") i zapisuje dziennik do pliku `cleaner_mag_revert_log.txt` w folderze TEMP. Każdy etap ma limit czasu, więc przywracanie nie zawiesi się na pojedynczej operacji (np. funkcjach opcjonalnych Windows); pozycje, których nie udało się przywrócić w limicie, trafiają na listę z podpowiedzią, jak zrobić to ręcznie.

Po odinstalowaniu usuwane są też pozostałe wpisy w Harmonogramie zadań i w rejestrze dodane przez program, a także skróty. Sam plik `.exe` możesz usunąć ręcznie po zakończeniu.

## Bezpieczeństwo i uwagi

- Program wymaga uprawnień administratora i modyfikuje ustawienia systemowe (rejestr, usługi, DNS, harmonogram zadań, ustawienia zegara/BCD, plik stronicowania).
- Blok utwardzania bezpieczeństwa (kroki 31–32) zmienia też ustawienia sieciowe (SMB, LLMNR/NetBIOS/WPAD) i UAC — w rzadkich przypadkach (stare drukarki lub udziały sieciowe w lokalnej sieci) może to wymagać ręcznej korekty.
- Przed pierwszą pełną optymalizacją tworzony jest **punkt przywracania systemu**, a stan kluczowych ustawień jest zapisywany do ewentualnego przywrócenia przy odinstalowaniu.
- Tryby Pełny i Szybki **zamykają otwarte okna przeglądarki** oraz **trwale usuwają folder `Windows.old`**, jeśli istnieje — program informuje o tym przed rozpoczęciem.
- Po zakończeniu wszystkich kroków program uruchamia krótką (ok. 10 sekund) diagnostykę wydajności energetycznej Windows (`powercfg /energy`) na potrzeby raportu końcowego — to nieszkodliwy, wyłącznie odczytowy pomiar.
- Aplikacja nie jest podpisana cyfrowo — Windows SmartScreen lub antywirus może wyświetlić ostrzeżenie przy pierwszym uruchomieniu. Pobieraj ją wyłącznie z oficjalnego repozytorium.
- Zamknij ważne, niezapisane prace przed uruchomieniem — zwłaszcza przed skanem KVRT (patrz wyżej) i przed ewentualnym restartem na końcu przebiegu. Zamknięcie okna w trakcie czyszczenia pyta o potwierdzenie, bo przerwanie operacji może zostawić część ustawień w stanie pośrednim.
- Program działa wyłącznie w Windows 10 i 11 — korzysta z narzędzi systemowych (SFC, DISM, PowerShell).

## Wsparcie projektu

Na końcu działania program pyta, czy chcesz postawić autorowi symboliczną kawę (po kliknięciu otwiera się strona w przeglądarce); przycisk „Wesprzyj projekt" jest też dostępny w „O programie":
👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

## Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

## Autor

Autor: **MAG** | Wersja: **2.0** (31/10/2026)
