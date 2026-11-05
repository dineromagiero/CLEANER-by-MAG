<p align="center">
  <img src="https://github.com/user-attachments/assets/0aa49271-1742-43af-a5e3-be9e190a355a" alt="Logo CLEANER by MAG" width="160">
</p>

<h1 align="center">CLEANER by MAG</h1>

<p align="center">
  <img alt="Wersja" src="https://img.shields.io/badge/wersja-2.1.1-blue">
  <img alt="Platforma" src="https://img.shields.io/badge/platforma-Windows%2010%20%2F%2011-0078D6">
  <img alt="Technologia" src="https://img.shields.io/badge/aplikacja-Python%20%2B%20Qt%20%2B%20PowerShell-lightgrey">
</p>

**Aplikacja do głębokiego czyszczenia, naprawy, optymalizacji i zabezpieczania systemu Windows** — z przejrzystym oknem zamiast konsoli, temperaturami na żywo, samoaktualizacją, raportami i historią podsumowań otwieranymi w programie, zgłaszaniem problemów prosto z aplikacji oraz opcjonalnymi skanerami antywirusowymi. Dostarczana jako jeden plik `.exe`.

Za całe czyszczenie odpowiada ten sam sprawdzony silnik co w wersjach 1.x, 2.0 i 2.1, więc kroki i ich kolejność pozostają takie, jakie znasz.

<p align="center">
  <img alt="Ekran startowy CLEANER by MAG z wyborem trybu" src="https://github.com/user-attachments/assets/958a0775-1462-4555-97b3-8d04f23dc5c2" width="760" />
<br>
  <em>Ekran startowy: wybór trybu czyszczenia (Windows 10).</em>
</p>

## Co nowego w wersji 2.1.1

Wersja 2.1.1 to wydanie poprawkowe: silnik czyszczenia, kroki i ich kolejność zostają bez zmian. Zmiany dotyczą **historii podsumowań** oraz **aktualizacji programów (winget)**.

- **Importuj — na stałe w historii**: podsumowania zaimportowane na stronie „Historia” (z dziennika, paczki zgłoszenia `.zip` lub pliku `podsumowanie.json`, także z kilku plików naraz) zostają na liście na stałe. Wcześniej znikały po przejściu do innego menu. Zaimportowane wpisy mają chłodniejszy odcień i ikonę „i”, żeby było widać, że pochodzą z importu. Wpisy, które już są w historii, nie są dublowane.
- **Historia już nie znika**: gdy plik historii jest chwilowo zajęty (np. przez antywirus albo OneDrive), lista nie wygląda już na pustą i nic nie jest nadpisywane. Do kopii `.bad` trafia tylko plik z naprawdę uszkodzoną zawartością, a poprawne wpisy z takich kopii wracają do historii przy starcie programu.
- **„Odtwórz z dziennika”**: nowy przycisk na stronie „Historia” dopisuje do listy podsumowania zapisane w dzienniku, których w niej brakuje. Istniejących wpisów nie zmienia i można go klikać wielokrotnie.
- **„Wszystkie programy są aktualne”**: gdy aktualizacja programów w „Opcjach dodatkowych” nie znajdzie nic do zrobienia, w podsumowaniu zobaczysz pozycję „Aktualizacja programów: wszystkie aktualne” i uwagę z wyjaśnieniem — zamiast pustej listy.
- **Programy przenośne aktualizują się**: programy przenośne zmienione poza wingetem (np. yt-dlp, FFmpeg) nie są już pomijane. Program ponawia próbę raz w trybie wymuszonym, a instalator nadal jest weryfikowany. Naprawiono też fałszywy komunikat o problemie z internetem, który pojawiał się przy udanej weryfikacji instalatora.
- **Spójne nazewnictwo**: „Plik diagnostyczny” nazywa się teraz po prostu **„Dziennik”** (w „O programie”, w oknie „Zgłoś problem” i przy imporcie historii).
- **Animowane strzałki** „Zobacz »” na przycisku „Co nowego” w „O programie”.

<details>
<summary>Co wprowadziła wersja 2.1 (kliknij, aby rozwinąć)</summary>

Zmiany dotyczyły przede wszystkim **temperatur**, **uwag przy krokach**, **zgłaszania problemów**, strony **„Przywracanie”** (dawniej „Odinstaluj”) oraz tego, **gdzie program się instaluje** i na co wskazują skróty w Menu Start.

- **Temperatury na żywo w pasku bocznym**: widżet pokazuje aktualną temperaturę procesora i karty graficznej (odświeżanie co 2 s), także w trakcie czyszczenia. Kolor odczytu zmienia się wraz z progiem, a czerwony delikatnie pulsuje. Kliknięcie widżetu wstrzymuje i wznawia odczyt, a podpowiedź pokazuje źródło wartości albo powód, dla którego czegoś brakuje.
- **Minimalistyczna karta „Temperatury” w podsumowaniu**: jedna cienka linia (**Temperatury  CPU X  GPU X**) ze średnią z całego przebiegu, każda wartość w kolorze progu. Po najechaniu na kartę otwiera się duży dymek ze szczegółami.
- **Statystyki temperatur z całego przebiegu**: zbierane od startu silnika do końca przebiegu, także w trybie Szybkim i w „Opcjach dodatkowych”. Trafiają do historii podsumowań i do dziennika diagnostycznego.
- **Niezawodniejszy odczyt temperatur**: źródła są ułożone warstwowo, a każdy odczyt jest sprawdzany pod kątem sensowności. Krok „Temperatury” w silniku bierze wartości z widżetu.
- **Uwagi przy krokach**: krok z uwagą ma ikonę dzwonka (w kolorze ważności), a po najechaniu pokazuje dymek z opisem i wskazówką. W podsumowaniu najechanie na operację przewija panel „Uwagi i podpowiedzi” do odpowiedniej karty.
- **Zgłaszanie problemów**: przycisk **„Zgłoś problem”** w pasku bocznym składa paczkę ZIP z opisem i wybranymi plikami diagnostycznymi, którą wysyłasz autorowi jednym kliknięciem — bez zakładania konta i bez GitHuba.
- **Przywracanie ustawień z wyborem elementów**: zaznaczasz, które z **17 elementów** mają wrócić do stanu sprzed optymalizacji, z postępem na żywo i podsumowaniem takim jak po czyszczeniu.
- **Ikona „Co nowego”**: po aktualizacji obok numeru wersji pojawia się ikona nowości, która znika po otwarciu okna „Co nowego”.
- **Stała kopia programu i stały skrót**: program kopiuje się do `C:\Program Files\CLEANER by MAG\CLEANER by MAG.exe` i na ten plik wskazuje skrót w Menu Start. Kolejne aktualizacje podmieniają tylko kopię, więc skrót (także przypięty do Start) nie zmienia celu.
- **Prostszy tryb Własny**: kategorii „Temperatury podzespołów” nie trzeba już wybierać — temperatury pokazuje widżet.

> **Uwaga przy aktualizacji z 2.0:** przy pierwszym uruchomieniu 2.1 skrót został raz przepisany na nową ścieżkę. Jeśli do Start przypięty był sam plik `.exe` z folderu Pobrane (a nie skrót „CLEANER by MAG” z listy programów), trzeba go przypiąć ponownie. Skrót odinstalowania nazywa się **„Odinstaluj”** — stary skrót jest usuwany automatycznie.

</details>

<details>
<summary>Co wprowadziła wersja 2.0 (kliknij, aby rozwinąć)</summary>

CLEANER by MAG przestał być skryptem `.bat` w oknie konsoli i stał się pełnoprawną aplikacją z własnym oknem.

- **Nowy interfejs graficzny**: ciemne okno z paskiem bocznym (wybór trybu, Opcje dodatkowe, Postęp, Raporty, Historia, Przywracanie, O programie), pierścieniem postępu, listą wszystkich etapów i licznikiem „Krok k z N”.
- **Jeden plik `.exe`**: silnik, skrypty pomocnicze i grafiki są w środku — nic nie trzeba rozpakowywać ani pobierać osobno.
- **Uwagi i podpowiedzi**: gdy coś wymaga uwagi, dostajesz opis problemu i wskazówkę, co zrobić, zamiast surowej linii z dziennika. Lista pozycji z audytu bezpieczeństwa (oraz wykluczeń Windows Defender) otwiera się w oknie programu i można ją skopiować.
- **Historia podsumowań**: po każdym zakończonym przebiegu program zapisuje ekran wyniku, a z menu „Historia” otworzysz go ponownie w takiej samej postaci.
- **Raporty i dziennik w programie**: raport HTML, raport TXT i dziennik otwierają się w oknie aplikacji, bez przeglądarki i Notatnika.
- **Opcje dodatkowe na osobnej stronie**: Malwarebytes, KVRT i aktualizację programów (winget) uruchamiasz osobno, w dowolnej chwili.
- **Karty etapów skanów i kafelki programów**: widać, co jest w kolejce, w trakcie, gotowe, pominięte lub zakończone błędem — wraz z powodem pominięcia.
- **Samoaktualizacja** z GitHub Releases, **szybszy start czyszczenia** i **wygodniejsze zamknięcie i restart** (przycisk restartu na ekranie podsumowania, pytanie przy zamykaniu).

</details>

## Spis treści

- [Co nowego w wersji 2.1.1](#co-nowego-w-wersji-211)
- [O programie](#o-programie)
- [Funkcje](#funkcje)
- [Wymagania](#wymagania)
- [Instalacja i pierwsze uruchomienie](#instalacja-i-pierwsze-uruchomienie)
- [Jak wygląda praca z programem](#jak-wygląda-praca-z-programem)
- [Tryby pracy](#tryby-pracy)
- [Co dokładnie robi tryb Pełny](#co-dokładnie-robi-tryb-pełny)
- [Temperatury](#temperatury)
- [Opcje dodatkowe](#opcje-dodatkowe)
- [Raporty i historia czyszczeń](#raporty-i-historia-czyszczeń)
- [Zgłaszanie problemów](#zgłaszanie-problemów)
- [Automatyczna aktualizacja](#automatyczna-aktualizacja)
- [Przywracanie i odinstalowanie](#przywracanie-i-odinstalowanie)
- [Bezpieczeństwo i uwagi](#bezpieczeństwo-i-uwagi)
- [Wsparcie projektu](#wsparcie-projektu)
- [Licencja](#licencja)
- [Autor](#autor)

## O programie

CLEANER by MAG to darmowe narzędzie do porządkowania i optymalizacji Windowsa, dystrybuowane jako pojedynczy plik `.exe` (bez instalatora i bez zależności do ręcznego pobierania — nie potrzebujesz Pythona). Przy pierwszym uruchomieniu program sam kopiuje się do stałego folderu i tworzy wpisy w Menu Start, dzięki czemu kolejne uruchomienia są jednym kliknięciem.

## Funkcje

- **37 kroków** czyszcząco-naprawczo-optymalizacyjnych i zabezpieczających w jednym przebiegu (pełna lista niżej)
- Graficzny interfejs z postępem krok po kroku, pierścieniem postępu i kartami uwag
- 3 tryby czyszczenia (Pełny / Szybki / Własny) oraz osobna strona Opcji dodatkowych
- **Temperatury procesora i karty graficznej na żywo** w pasku bocznym, z osobnymi progami kolorów dla CPU i GPU oraz wyjaśnieniem, gdy odczyt jest niemożliwy
- **Statystyki temperatur z przebiegu** (średnia na karcie, najniższa i najwyższa z godziną w dymku) w podsumowaniu, historii i dzienniku
- Uwagi przy krokach z ikoną dzwonka i dymkiem z opisem oraz powiązaniem z kartą uwag w podsumowaniu
- **Zgłaszanie problemów** z poziomu programu (paczka ZIP z opisem, zrzutem okna i dziennikiem, z ukrywaniem nazwy użytkownika)
- **Przywracanie ustawień z wyborem elementów** (17 pozycji) z postępem na żywo oraz osobne usuwanie raportów i historii
- **Historia podsumowań** dostępna w programie (ostatnie 100 wpisów) — odporna na chwilowe blokady pliku, z **importem** z dziennika, paczki zgłoszenia lub pliku JSON oraz z **odtwarzaniem z dziennika**
- Automatyczne sprawdzanie i pobieranie aktualizacji z GitHub Releases, z samopodmianą pliku `.exe`; stała kopia programu i stały skrót w Menu Start
- Tworzenie punktu przywracania systemu przed zmianami
- Wbudowane skanowanie Windows Defender i AdwCleaner
- Opcjonalne skanery na żądanie: **Malwarebytes** i **KVRT** (Kaspersky Virus Removal Tool) — pobierane, uruchamiane i sprzątane automatycznie
- Aktualizacja zainstalowanych programów przez **winget** (Menedżer pakietów Windows), z automatyczną instalacją winget, jeśli go brakuje, z obsługą programów przenośnych i z powodem pominięcia przy każdym programie, którego nie udało się zaktualizować
- Utwardzanie bezpieczeństwa: reguły ASR, ochrona LSA, zabezpieczenie SMB, blokada autoodtwarzania i makr Office, przywrócenie UAC, audyt persystencji i diagnostyka zdrowia systemu
- Debloat: usuwanie zbędnych aplikacji Windows, wyłączanie zbędnych funkcji, procesów w tle i programów startowych
- Strojenie sieci: DNS, DNS over HTTPS, parametry TCP oraz reset proxy
- Czytelne podpowiedzi przy ponad 50 typowych problemach (brak internetu, wyłączona ochrona systemu, blokada przez antywirus, błędy SFC/DISM i wiele innych)
- Szczegółowy raport HTML (z awaryjnym trybem `.txt`) po każdym przebiegu, otwierany w programie, z dołączonym raportem wydajności energetycznej Windows (a na laptopach też raportem baterii i uśpienia)
- Pływająca nakładka z licznikiem kroków, widoczna, gdy okno programu jest zminimalizowane lub nie ma fokusu
- Blokada uśpienia komputera („keep-awake”) na czas długich operacji
- Samoinstalacja w Menu Start (aplikacja, skrót do raportów, skrót „Odinstaluj”)
- Pełny deinstalator z opcją przywrócenia ustawień sprzed instalacji
- Okno dopasowujące się do rozdzielczości i skalowania ekranu (m.in. 1920x1080 przy 125%)

## Wymagania

- Windows 10 lub Windows 11
- Uprawnienia administratora (program sam o nie poprosi przez okno UAC, jeśli ich zabraknie; bez nich nie da się też odczytać temperatury procesora)
- PowerShell (wbudowany w system — silnik intensywnie z niego korzysta)
- Połączenie z internetem — potrzebne do sprawdzania aktualizacji, skanów AdwCleaner/Defender, pobierania Malwarebytes/KVRT oraz jednorazowego pobrania biblioteki czujników LibreHardwareMonitor (odczyt temperatur procesora)

## Instalacja i pierwsze uruchomienie

1. Pobierz najnowszy plik `.exe` z zakładki [Releases](https://github.com/dineromagiero/CLEANER-by-MAG/releases/latest) i uruchom go z dowolnego folderu.
2. Jeśli program nie ma uprawnień administratora, pokaże przycisk **Uruchom jako administrator** i poprosi o potwierdzenie w oknie UAC (albo kliknij plik prawym przyciskiem myszy → **Uruchom jako administrator**).
3. Przy pierwszym uruchomieniu program kopiuje się do `C:\Program Files\CLEANER by MAG\CLEANER by MAG.exe` (nazwa pliku jest zawsze taka sama, bez numeru wersji) i tworzy wpisy w Menu Start (`CLEANER by MAG`): skrót do aplikacji, do folderu raportów oraz **„Odinstaluj”**. Zobaczysz też krótkie okno powitalne z opisem nowej odsłony (pokazuje się tylko raz; później znajdziesz nowości w „O programie” → „Co nowego”).
4. Kolejne uruchomienia wykonuj ze skrótu w Menu Start — program sam sprawdzi, czy jest aktualny. Skróty (także przypięty do Start) wskazują na stałą kopię, więc pobrany plik możesz usunąć. Uruchomienie nowszego pliku `.exe` (z dowolnej nazwy i folderu) tylko podmienia tę kopię; starszy plik nigdy nie nadpisze nowszej kopii.
5. Jeśli skopiowanie się nie uda (np. brak uprawnień), skróty wskażą bieżący plik — wtedy nie przenoś go ani nie usuwaj. Przyczynę zapisuje plik `%TEMP%\cleaner_mag_install.log`.

## Jak wygląda praca z programem

1. **Wybierz tryb** na ekranie startowym (karty: Pełny, Szybki, Własny, Opcje dodatkowe). Przed startem program przypomina, że zostaną zamknięte przeglądarki, a folder `Windows.old` (jeśli istnieje) zostanie trwale usunięty.
2. **Obserwuj postęp** na stronie „Postęp”: pierścień pokazuje ogólny postęp, lista pokazuje wszystkie etapy i ich stany, a licznik — „Krok k z N”. Możesz przerwać operację przyciskiem „Przerwij”, ale przerwanie w trakcie pracy może zostawić część ustawień w stanie pośrednim (np. usługi Windows Update), więc zalecane jest dokończenie. Widżet temperatur w pasku bocznym działa przez cały czas.
3. **Przeczytaj wynik**: ekran „Wszystko gotowe” lub „Zakończono z uwagami” pokazuje wykonane i pominięte operacje, elementy wymagające restartu, błędy, czas oraz kartę temperatur. Panel „Uwagi i podpowiedzi” tłumaczy, co oznacza każdy problem i co z nim zrobić — najechanie na operację z dzwonkiem przewija go do właściwej uwagi. Każde podsumowanie trafia też do strony „Historia”.
4. **Zrestartuj komputer** przyciskiem na ekranie podsumowania, jeśli program to zaleca (czyszczenie zmieniło system albo Windows czeka na restart).

Podczas operacji pozostałe pozycje menu są zablokowane, a pasek boczny pokazuje, że operacja trwa. Przycisk „Dziennik” i zakładka „Dziennik” na stronie „Postęp” pozwalają zajrzeć w szczegóły techniczne bez opuszczania programu.

## Tryby pracy

| Tryb | Opis |
|---|---|
| **Pełny** (zalecany) | wszystkie 37 kroków, plus odczyt temperatur na końcu |
| **Szybki** (na co dzień) | pomija skany antywirusowe, naprawę systemu (DISM/SFC), czyszczenie starych sterowników, naprawę Windows Update, optymalizację dysku C: i krok odczytu temperatur; krok prywatności i reklam (bez wstępnej kontroli plików systemowych) wykonuje |
| **Własny** (ty decydujesz) | wykonuje wyłącznie zaznaczone kategorie (lista niżej) — na stronie postępu widzisz tylko je |
| **Opcje dodatkowe** | osobna strona z Malwarebytes, KVRT i aktualizacją programów, uruchamiana niezależnie od czyszczenia |

W trybie **Własnym** zaznaczasz przełącznikami (są przyciski „Zaznacz wszystko” i „Wyczyść”):
1. skan antywirusowy (Windows Defender + AdwCleaner),
2. naprawę systemu (wstępna kontrola plików systemowych + DISM + SFC + porządki w bazie składników — najdłuższy etap),
3. czyszczenie starych sterowników drukarek i urządzeń,
4. naprawę Windows Update,
5. optymalizację dysku C: (defragmentacja lub TRIM, zależnie od typu dysku),
6. sprawdzenie dysku C: (CHKDSK) — z pominięciem limitu „raz na 30 dni”.

Temperatur nie trzeba wybierać — pokazuje je widżet w pasku bocznym, a statystyki z przebiegu pojawiają się w podsumowaniu w każdym trybie. Punkt przywracania systemu (krok 1) jest tworzony, gdy wybierzesz cokolwiek z powyższej listy. Jeśli nie zaznaczysz żadnej kategorii, program od razu przechodzi do dodatkowych opcji. Blok utwardzania bezpieczeństwa i końcowych porządków (kroki 31–37) nie ma jeszcze osobnej kategorii — w trybie Własnym jest zawsze pomijany.

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

Po kroku 37 tryb Pełny wykonuje jeszcze odczyt temperatur procesora i karty graficznej — bierze bieżące wartości z widżetu (wynik trafia na ekran podsumowania i do raportu).

</details>

## Temperatury

**Widżet w pasku bocznym** pokazuje aktualną temperaturę procesora (CPU) i karty graficznej (GPU), odświeżaną co 2 s — także w trakcie czyszczenia. Kolor kropki przy odczycie zależy od progu, a czerwony odczyt delikatnie pulsuje. Progi są osobne dla CPU i GPU, bo ta sama temperatura znaczy co innego (karta graficzna zaczyna zwalniać wcześniej):

| Kolor | CPU | GPU |
|---|---|---|
| zielony | poniżej 65 °C | poniżej 60 °C |
| pomarańczowy | od 65 °C | od 60 °C |
| czerwony | od 80 °C | od 75 °C |

Brak odczytu jest szary. Kliknięcie widżetu wstrzymuje i wznawia odczyt, a w podpowiedzi widać źródło wartości oraz — gdy czegoś brakuje — przyczynę (np. brak uprawnień administratora, nieudane załadowanie biblioteki czujników, procesor wykryty bez udostępnionej temperatury).

**Skąd pochodzą odczyty** (źródła są sprawdzane po kolei, a każdy odczyt trafia przez test sensowności; wartości spoza zakresu 1–125 °C są odrzucane):

- **karta graficzna**: NVML (biblioteka sterownika NVIDIA) → LibreHardwareMonitor (AMD / Intel) → `nvidia-smi`,
- **procesor**: LibreHardwareMonitor (Package / Tdie / Tctl) → strefy termiczne ACPI (wartość przybliżona).

Biblioteka LibreHardwareMonitor jest pobierana raz do trwałego folderu, a proces pomocniczy działa przez cały czas pracy widżetu.

**Karta „Temperatury” w podsumowaniu** to jedna cienka linia: **Temperatury  CPU X  GPU X** — średnia z całego przebiegu, każda wartość w kolorze progu (gdy nie ma statystyk, pokazywany jest odczyt z kroku „Temperatury”). Mała ikona „i” na końcu linii podpowiada, że **po najechaniu na kartę** otwiera się duży dymek ze szczegółami: najniższa i najwyższa temperatura z godziną pomiaru, średnia z liczbą odczytów, okres pomiarów i legenda kolorów. Gdy nie uda się odczytać ani temperatury procesora, ani karty graficznej, karty nie ma wcale.

Statystyki są zbierane od startu silnika do końca przebiegu, także w trybie Szybkim i w „Opcjach dodatkowych”. Przywracanie ich nie liczy. Zapisują się w historii podsumowań oraz w dzienniku (sekcja „Temperatury w trakcie przebiegu”).

## Opcje dodatkowe

Dostępne na osobnej stronie „Opcje dodatkowe” (w dowolnej chwili, bez czyszczenia systemu) oraz jako propozycja po zakończeniu czyszczenia — wynik widzisz wtedy od razu, a pytanie o przejście dalej pojawia się dopiero na ekranie podsumowania. Kliknięcie „Uruchom” przy wybranej opcji startuje właśnie tę jedną opcję, bez dalszych pytań. Skany Malwarebytes i KVRT są pokazywane jako lista kart etapów (Malwarebytes: 4 etapy, KVRT: 3 etapy), a pierścień „Etapy skanu” i nakładka pokazują numer bieżącego etapu.

- **Malwarebytes** (obsługa ręczna) — pobiera instalator, instaluje program i otwiera go do ręcznego skanu, a po zamknięciu okna automatycznie go odinstalowuje. Jeśli Malwarebytes był już zainstalowany, program po prostu go otwiera (niczego nie pobiera ani nie usuwa). Gdy proces po zamknięciu okna nadal wisi w tle, możesz użyć przycisku **„Wymuś zamknięcie”** (program próbuje kolejno kilku metod, w tym zamknięcia przez ikonę w zasobniku) albo skorzystać z instrukcji „Jak zamknąć ręcznie”.
- **Aktualizacja programów przez winget** — sprawdza, czy Menedżer pakietów Windows (winget) jest dostępny; jeśli go brakuje (np. na edycjach LTSC), oferuje instalację nieoficjalnym sposobem — pobranie najnowszego wydania bezpośrednio z repozytorium `microsoft/winget-cli` na GitHubie. Następnie aktualizuje po kolei wszystkie zainstalowane programy. Każdy program ma własny kafelek (w kolejce → pobieranie → instalowanie → gotowe) z wersją obecną i dostępną oraz paskiem pobierania, w którym procent jest odczytywany z usługi Delivery Optimization (a gdy jest niedostępna — z rozmiaru pobieranego pliku).
  - Jeśli program nie zostanie zaktualizowany, kafelek wyjaśnia dlaczego (np. program jest uruchomiony, brak pasującej aktualizacji, wymagana ręczna reinstalacja albo błąd pobierania).
  - **Programy przenośne** (np. yt-dlp, FFmpeg) zmienione poza wingetem — np. przez własną samoaktualizację — są aktualizowane po jednorazowej ponownej próbie w trybie wymuszonym; skrót instalatora jest nadal weryfikowany. Jeśli i to się nie uda, program podpowiada zamknięcie aplikacji lub ponowną instalację.
  - Gdy winget nie znajduje żadnej aktualizacji, ekran wyniku pokazuje **„Aktualizacja programów: wszystkie aktualne”** wraz z uwagą informacyjną (nie zmienia ona podsumowania „Wszystko gotowe”).
- **KVRT (Kaspersky Virus Removal Tool)** — pobiera najnowszą wersję i uruchamia w pełni automatyczny skan w tle, a po zakończeniu sam się usuwa wraz z danymi tymczasowymi (folder `KVRT_Data` jest sprzątany, chyba że na komputerze wykryto zainstalowany pełny produkt Kaspersky — wtedy zostaje nietknięty). Skan, który nie zakończy się w ciągu 90 minut, jest przerywany, a wynik skanu można obejrzeć przyciskiem „Pokaż wynik skanu”.

> ⚠️ **Uwaga:** KVRT może w trakcie skanowania wykryć poważne zagrożenie i **zrestartować komputer bez pytania o zgodę**. Zapisz pracę przed uruchomieniem tej opcji — program blokuje nieoczekiwany restart tylko na czas własnego działania i ostrzega, jeśli KVRT próbował go wymusić.

## Raporty i historia czyszczeń

- Po każdym przebiegu zapisywany jest raport:
  `Dokumenty\CLEANER by MAG\Raport_<data>_<godzina>.html` — czytelny, stylizowany raport HTML. Jeśli jego wygenerowanie się nie powiedzie, program automatycznie zapisuje w zamian awaryjną wersję `Raport_<data>_<godzina>.txt`.
- Strona **„Raporty”** w programie pokazuje listę wszystkich zapisanych raportów (data i godzina, rozmiar) z przyciskami „Otwórz”, „Wersja TXT” i „Otwórz folder”. Raport otwiera się w oknie aplikacji; z podglądu można go nadal otworzyć w przeglądarce.
- Raport zawiera m.in.: datę i tryb, czas trwania, ilość zwolnionego miejsca na dysku C:, dane sprzętowe (CPU/GPU/dysk), wersję i build Windows oraz status każdej operacji (`OK` / `SKIP` / `BŁĄD` / `REST` — wymaga restartu). Jeśli krok 35 (audyt) znajdzie pozycje do przejrzenia, ich lista trafia do tego samego raportu, a w programie otwierasz ją przyciskiem „Pokaż listę”.
- Na końcu raportu dołączany jest raport wydajności energetycznej Windows (`powercfg /energy`), generowany dopiero po zakończeniu pracy i przywróceniu oryginalnych ustawień uśpienia/wygaszania ekranu, dzięki czemu odzwierciedla rzeczywisty stan systemu. Na laptopach dołączane są też raport kondycji baterii i raport uśpienia.
- Skrót **„Raporty”** w Menu Start prowadzi bezpośrednio do folderu z raportami.
- Silnik nadal generuje historię ostatnich 20 sesji (ile miejsca zwalniano i ile trwało czyszczenie) na podstawie zapisanych raportów.
- **Dziennik** (`.devlog.txt`) otwierasz w programie przyciskiem „Dziennik” w „O programie”. Jest zapisywany podczas czyszczenia i kasowany przy starcie następnego przebiegu, więc zawsze dotyczy ostatniej operacji.

### Historia podsumowań

Strona **„Historia”** przechowuje podsumowania zakończonych przebiegów (Pełny, Szybki, Własny i Opcje dodatkowe), razem ze statystykami temperatur i powiązaniami operacji z uwagami. Przycisk „Pokaż” otwiera wybrane podsumowanie w tym samym widoku co zaraz po przebiegu (tylko do odczytu), a pojedyncze wpisy lub całą historię można usunąć („Wyczyść historię”). Program przechowuje ostatnie 100 wpisów w pliku `Historia_podsumowan.json` w folderze raportów (`Dokumenty\CLEANER by MAG`). Usunięcie historii nie kasuje raportów z folderu Dokumenty.

- **Importuj** — wczytuje podsumowania z dziennika (`.txt`), paczki zgłoszenia (`.zip`) lub pliku `podsumowanie.json` (można zaznaczyć kilka plików naraz) i dopisuje je do historii **na stałe**. Zaimportowane wpisy mają inny odcień i ikonę „i”. Podsumowania, które już są w historii, nie są dublowane, a dwa różne przebiegi o tym samym czasie zakończenia nie są traktowane jako duplikat. Jedno znalezione podsumowanie otwiera się od razu. To przydatne m.in. przy analizie zgłoszeń przysłanych przez innych użytkowników.
- **Odtwórz z dziennika** — dopisuje do historii podsumowania zapisane w bieżącym dzienniku, których w niej brakuje. Istniejących wpisów nie zmienia i można go klikać wielokrotnie. Odtworzy tylko tyle, ile jest w dzienniku (dziennik jest przycinany do 1 MB, a starsze wersje programu nie zapisywały w nim podsumowań).
- **Odporność na błędy pliku** — chwilowy błąd odczytu (plik zajęty przez antywirus lub OneDrive) jest ponawiany i nigdy nie powoduje nadpisania historii. Do kopii `.bad` trafia tylko plik z faktycznie uszkodzoną zawartością, a przy starcie poprawne wpisy z takich kopii wracają do historii. Przy migracji program szuka też w starym folderze `%USERPROFILE%\Documents\CLEANER by MAG`.

## Zgłaszanie problemów

Przycisk **„Zgłoś problem”** w pasku bocznym otwiera okno, w którym opisujesz, co poszło nie tak (na którym kroku, co się stało, czego się spodziewałeś) i wybierasz, co dołączyć do zgłoszenia:

- **zrzut ekranu okna programu** (z podglądem),
- **dziennik** z ostatniego przebiegu,
- **podsumowanie wyniku** (dane ekranu wyniku, żeby autor mógł go odtworzyć),
- **informacje o systemie** (wersja programu i Windows, architektura, uprawnienia administratora, tryb ostatniego przebiegu oraz — gdy widżet nie widzi temperatury procesora — jedna linia diagnostyczna z przyczyną).

Pozycji, których jeszcze nie ma (np. dziennik przed pierwszym przebiegiem), nie da się wybrać. Z zaznaczonych elementów program składa **paczkę ZIP** i zapisuje ją w folderze `Dokumenty\CLEANER by MAG\Zgloszenia`. Przyciskiem **„Wyślij do autora”** wysyłasz ją bez zakładania konta i bez logowania, opcjonalnie z kontaktem do siebie, jeśli chcesz dostać odpowiedź. Każde zgłoszenie dostaje **sześcioznakowy numer** (np. `AB12CD`), który jest w nazwie pliku ZIP, w notatce w środku i w wiadomości u autora. Zgłoszenia nie trafiają na GitHub.

- **Prywatność**: nic nie wychodzi z komputera automatycznie, tylko po kliknięciu „Wyślij do autora”. Przełącznik **„Ukryj nazwę użytkownika”** (domyślnie włączony) zamienia nazwę użytkownika Windows w dzienniku i podsumowaniu na `<uzytkownik>`, a nazwę komputera na `<komputer>`. Po jego wyłączeniu okno ostrzega, że nazwa zostanie w paczce w niezmienionej postaci, a zrzut ekranu pokazuje to, co widać w oknie.
- **Wygoda**: niewysłany opis jest zapamiętywany, więc po zamknięciu okna nie musisz pisać go od nowa. Błąd zapisu lub wysyłki kończy się komunikatem, a nie awarią programu.

## Automatyczna aktualizacja

- Program sprawdza w tle, czy w repozytorium [`dineromagiero/CLEANER-by-MAG`](https://github.com/dineromagiero/CLEANER-by-MAG/releases/latest) jest nowsze wydanie. Wskaźnik w lewym dolnym rogu paska bocznego pokazuje stan: łączenie, brak połączenia, najnowsza wersja albo dostępna aktualizacja.
- Gdy jest nowsza wersja, program pyta, czy ją zainstalować. Przy „Tak” pobiera nowy plik `.exe` (obok bieżącego, z paskiem postępu), zamyka się, podmienia plik za pomocą ukrytego pomocnika i uruchamia nową wersję. Wybierając „Nie”, możesz zaktualizować później, klikając wskaźnik aktualizacji.
- Po zwykłej aktualizacji obok numeru wersji pojawia się ikona „Co nowego” — znika po otwarciu okna nowości. Przy świeżej instalacji ikony nie ma, bo nowości są wtedy w oknie powitalnym.
- Jeśli samodzielna podmiana nie jest możliwa (np. wydanie zawiera tylko archiwum `.zip`), plik zostaje zapisany w folderze „Pobrane” albo otwierana jest strona wydania.
- Pobieranie jest dozwolone wyłącznie z domen `github.com` i `githubusercontent.com`; przekierowania poza GitHub są odrzucane.
- Brak połączenia z internetem nie blokuje działania — sprawdzanie aktualizacji jest po prostu pomijane, a wskaźnik pozwala spróbować ponownie.

## Przywracanie i odinstalowanie

Strona **„Przywracanie”** otwiera się z menu bocznego albo skrótem **„Odinstaluj”** w Menu Start (wymaga uprawnień administratora; skrót uruchamia program z parametrem `--uninstall` i przewija stronę na sam dół). Masz tu trzy osobne czynności:

- **Przywróć wybrane** — zaznaczasz, które z **17 elementów** mają wrócić do stanu sprzed optymalizacji: rejestr, usługi Windows, funkcje opcjonalne, BCD, TCP / RSS, DNS, DoH, plik stronicowania, Defender, SMB, NetBIOS, zasilanie kart sieciowych, plan zasilania i hibernacja, Oczyszczanie dysku, zadania harmonogramu, kopie w tle VSS, aplikacje UWP. Dostępne są „Zaznacz wszystko” i „Wyczyść”, licznik „Wybrano n z 17” oraz potwierdzenie z liczbą wybranych elementów. Zadania harmonogramu wracają do swojego dokładnego stanu sprzed zmian (Włączone/Wyłączone), a nie są masowo włączane.
- **Usuń raporty i historię** — bez odinstalowywania programu.
- **Odinstaluj program całkowicie** — pyta, czy przywrócić ustawienia i czy usunąć raporty (razem z nimi znika też historia podsumowań).

Przywracanie wygląda jak tryb Własny: strona „Postęp” pokazuje „Krok k z N” i listę wybranych elementów, pierścień dochodzi do 100%, a nakładka pokazuje licznik k/N. Ekran wyniku podaje faktyczne liczby (wykonane, pominięte, do restartu, błędy), a pozycje z błędem lub pominięte trafiają do jednej uwagi na element, z pełną listą pod przyciskiem. Elementy, których zmiana zadziała w pełni dopiero po restarcie (rejestr, usługi, funkcje opcjonalne Windows, BCD, plik stronicowania, SMB), liczą się do „Do restartu”, a program proponuje restart. Każdy etap ma limit czasu, więc przywracanie nie zawiesi się na pojedynczej operacji; pozycje, których nie udało się przywrócić w limicie, trafiają na listę z podpowiedzią, jak zrobić to ręcznie. Dziennik tej operacji otwierasz przyciskiem **„Otwórz dziennik”** na ekranie wyniku (zapisuje go też plik `cleaner_mag_revert_log.txt` w folderze TEMP).

Pełne odinstalowanie usuwa też wpisy w Harmonogramie zadań i w rejestrze dodane przez program, skróty oraz stałą kopię programu (`C:\Program Files\CLEANER by MAG`). Pobranego pliku `.exe`, z którego uruchomiono program, nie usuwa — zrobisz to ręcznie (informacja o tym jest na stronie „Przywracanie”).

## Bezpieczeństwo i uwagi

- Program wymaga uprawnień administratora i modyfikuje ustawienia systemowe (rejestr, usługi, DNS, harmonogram zadań, ustawienia zegara/BCD, plik stronicowania).
- Blok utwardzania bezpieczeństwa (kroki 31–32) zmienia też ustawienia sieciowe (SMB, LLMNR/NetBIOS/WPAD) i UAC — w rzadkich przypadkach (stare drukarki lub udziały sieciowe w lokalnej sieci) może to wymagać ręcznej korekty.
- Przed pierwszą pełną optymalizacją tworzony jest **punkt przywracania systemu**, a stan kluczowych ustawień jest zapisywany do ewentualnego przywrócenia na stronie „Przywracanie”.
- Tryby Pełny i Szybki **zamykają otwarte okna przeglądarki** oraz **trwale usuwają folder `Windows.old`**, jeśli istnieje — program informuje o tym przed rozpoczęciem.
- Po zakończeniu wszystkich kroków program uruchamia krótką (ok. 10 sekund) diagnostykę wydajności energetycznej Windows (`powercfg /energy`) na potrzeby raportu końcowego — to nieszkodliwy, wyłącznie odczytowy pomiar.
- Odczyt temperatur korzysta z biblioteki LibreHardwareMonitor pobieranej jednorazowo z internetu; jeśli Windows Defender ją zablokuje, widżet podaje przyczynę zamiast pustego pola.
- Aplikacja nie jest podpisana cyfrowo — Windows SmartScreen lub antywirus może wyświetlić ostrzeżenie przy pierwszym uruchomieniu. Pobieraj ją wyłącznie z oficjalnego repozytorium.
- Zamknij ważne, niezapisane prace przed uruchomieniem — zwłaszcza przed skanem KVRT (patrz wyżej) i przed ewentualnym restartem na końcu przebiegu. Zamknięcie okna w trakcie czyszczenia pyta o potwierdzenie, bo przerwanie operacji może zostawić część ustawień w stanie pośrednim.
- Program działa wyłącznie w Windows 10 i 11 — korzysta z narzędzi systemowych (SFC, DISM, PowerShell).

## Wsparcie projektu

Na końcu działania program pyta, czy chcesz postawić autorowi symboliczną kawę (po kliknięciu otwiera się strona w przeglądarce); przycisk „Wesprzyj projekt” jest też dostępny w „O programie”:
👉 [suppi.pl/cleanerbymag](https://suppi.pl/cleanerbymag)

## Licencja

Projekt udostępniony na licencji MIT. Możesz swobodnie używać, modyfikować i dystrybuować z zachowaniem informacji o autorze.

## Autor

Autor: **MAG** | Wersja: **2.1.1** (05/11/2026)
