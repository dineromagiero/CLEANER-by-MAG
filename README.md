# 🛠️ CLEANER by MAG

**CLEANER by MAG** to potężny, zautomatyzowany skrypt wsadowy (.bat) zaprojektowany do głębokiego oczyszczania, naprawy i optymalizacji systemów Windows. 

Narzędzie inteligentnie dopasowuje ustawienia do Twojego sprzętu, dbając o maksymalną wydajność i prywatność.

---

## 🚀 Kluczowe Funkcje wersji v1.02

### 🧠 Inteligentna Optymalizacja Sprzętowa

* **Detekcja Typu Dysku:** Automatycznie rozpoznaje dyski SSD/NVMe i optymalizuje ich pracę (m.in. wyłączając zbędną hibernację).
* **Analiza Urządzenia:** Rozróżnia PC stacjonarny od laptopa. Dla komputerów stacjonarnych automatycznie aktywuje plan zasilania **"Wysoka Wydajność"**.
* **Zarządzanie Pamięcią:** Dynamicznie oblicza i ustawia optymalny rozmiar **pliku stronicowania (PageFile)** na podstawie posiadanej ilości pamięci RAM.

---

### 🛡️ Bezpieczeństwo i Diagnostyka

* **Restore Point:** Tworzy punkt przywracania systemu przed rozpoczęciem jakichkolwiek zmian.
* **Anty-Malware:** Integracja z Windows Defender, **AdwCleaner** oraz opcjonalny skan **Malwarebytes** (automatyczne pobieranie najnowszych wersji).
* **Naprawa Systemu:** Wykonuje pełną procedurę naprawczą: `SFC /scannow`, `DISM /RestoreHealth` oraz naprawę repozytorium **WMI**.

---

### 🧹 Głębokie Czyszczenie

* **Browser Cleaner:** Czyści cache i pliki tymczasowe Chrome, Edge, Firefox, Brave, Opera oraz Opera GX.
* **System Trash:** Usuwa minidumpy, raporty błędów (WER), pliki Prefetch, Downloaded Program Files oraz cache ikon.
* **Windows Update:** Czyści folder `SoftwareDistribution`, usuwając pozostałości po starych aktualizacjach.

---

### ⚙️ Prywatność i Tuning

* **Debloat:** Usuwa zbędne aplikacje systemowe (Solitaire, Bing, Skype itp.).
* **Telemetria:** Blokuje śledzenie, Cortanę oraz wyłącza zbędne harmonogramy zadań zbierające dane o użytkowniku.
* **Network Tweak:** Resetuje stos TCP/IP, czyści DNS i optymalizuje parametry sieciowe dla niższego opóźnienia.

---

## 📋 Wymagania i Uruchomienie

1. **Uprawnienia:** Skrypt **musi** być uruchomiony jako Administrator.
2. **Internet:** Wymagany do pobrania zewnętrznych skanerów przez narzędzie `curl`.
3. **Restart:** Po zakończeniu pracy skrypt zaplanuje sprawdzanie dysku `chkdsk` i poprosi o restart systemu.

---

**Autor:** Mag | **Wersja:** 1.02 (17/04/2026)
