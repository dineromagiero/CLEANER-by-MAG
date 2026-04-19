# 🛠️ CLEANER by MAG

**CLEANER by MAG** to zaawansowane, w pełni zautomatyzowane narzędzie serwisowe w formie skryptu `.bat`. Zostało zaprojektowane, aby przywrócić system Windows do szczytowej formy poprzez głębokie czyszczenie, inteligentną optymalizację sprzętową oraz naprawę kluczowych komponentów systemowych.

---

## 💎 Co wyróżnia wersję 1.03?

Najnowsza aktualizacja wprowadza inteligentne raportowanie oraz dedykowane moduły dla często zaniedbywanych obszarów systemu:

* **📊 Inteligentny System Raportowania:** Po zakończeniu pracy skrypt generuje szczegółowy log `C:\CLEANER by MAG - WYNIKI.txt` oraz wyświetla podsumowanie z szacunkową ilością odzyskanego miejsca na dysku.
* **🖨️ Serwis Drukowania:** Automatyczne czyszczenie kolejki wydruku oraz usuwanie zbędnych, starych sterowników drukarek.
* **🛍️ Microsoft Store Fix:** Kompleksowe odświeżanie pamięci podręcznej Sklepu Windows i naprawa usług zależnych.
* **🖼️ Naprawa Interfejsu:** Czyszczenie baz danych ikon i miniatur (`IconCache` / `ThumbCache`) dla płynniejszego działania Eksploratora.

---

## 🚀 Kluczowe Funkcje

### 🧠 Optymalizacja Sprzętowa (Hardware-Aware)
* **Wykrywanie Typu PC:** Skrypt rozpoznaje czy pracujesz na laptopie, czy komputerze stacjonarnym, dopasowując plan zasilania (**Wysoka Wydajność** dla PC).
* **SSD/NVMe Tuning:** Rozpoznaje model dysku i optymalizuje jego pracę (np. wyłączając zbędną hibernację).
* **Dynamiczny RAM & PageFile:** Oblicza rozmiar pliku stronicowania na podstawie realnej ilości pamięci RAM w Twoim komputerze.

### 🛡️ Bezpieczeństwo i Naprawa
* **System Restore:** Automatyczne tworzenie punktu przywracania przed zmianami.
* **Multi-Scanner:** Integracja z Windows Defender oraz automatyczne pobieranie/skanowanie za pomocą **AdwCleaner** i (opcjonalnie) **Malwarebytes**.
* **Deep Repair:** Wykonywanie procedur `SFC /scannow`, `DISM /RestoreHealth` oraz naprawa repozytorium **WMI**.

### 🧹 Totalne Oczyszczanie
* **Browser Cleaner:** Obsługa Chrome, Edge, Firefox, Brave, Opera oraz Opera GX.
* **Windows Update:** Czyszczenie bazy `SoftwareDistribution` i naprawa usług Windows Update.
* **System Trash:** Usuwanie logów CBS, plików tymczasowych, zrzutów pamięci (Minidump) i raportów błędów WER.

---

## 📋 Instrukcja Uruchomienia

1.  Pobierz plik `CLEANER.by.MAG.v.1.03.bat`.
2.  Kliknij Prawym Przyciskiem Myszy i wybierz **"Uruchom jako administrator"** (wymagane do zmian w rejestrze i usługach).
3.  Podążaj za instrukcjami w kolorowej konsoli.
4.  Po zakończeniu zapoznaj się z **Raportem Czyszczenia** i zrestartuj komputer.

---

## ⚠️ Ostrzeżenie

> **Używasz na własną odpowiedzialność.** Narzędzie dokonuje głębokich zmian w konfiguracji usług i rejestru systemowego. Skrypt automatycznie tworzy punkt przywracania, jednak zawsze zaleca się posiadanie kopii zapasowej ważnych danych.

---

**Autor:** Mag  
**Wersja:** 1.03 (19/04/2026)  
**Status:** Stabilna
