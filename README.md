# File Integrity Monitor (HIDS)

## Opis projektu
Projekt implementuje system monitorowania integralności plików (Host-based Intrusion Detection System - HIDS) działający w czasie rzeczywistym. Narzędzie wykrywa nieautoryzowane modyfikacje lub usunięcia plików w zdefiniowanym katalogu i automatycznie przywraca ich pierwotną wersję na podstawie wcześniej przygotowanych wzorców.

System wykorzystuje algorytm SHA-256 do weryfikacji sum kontrolnych oraz bibliotekę watchdog do asynchronicznego monitorowania zdarzeń systemowych.

### Kontekst akademicki
Projekt został przygotowany jako praca zaliczeniowa z przedmiotów:
* Systemy Operacyjne 2 (zarządzanie usługami systemd, procesy w tle, operacje na systemie plików).
* Automatyzacja procesów w inżynierii oprogramowania (automatyzacja wdrażania za pomocą skryptów Bash, mechanizmy samonaprawy systemu).

---

## Struktura plików
Aby skrypt instalacyjny działał poprawnie, pliki w repozytorium muszą być ułożone w następujący sposób:
* src/main.py: Główna logika monitorująca i obsługa zdarzeń.
* src/integrity.py: Moduł odpowiedzialny za obliczanie skrótów kryptograficznych.
* install.sh: Skrypt powłoki (Bash) automatyzujący instalację i konfigurację usługi systemowej.
* .gitignore: Konfiguracja wykluczeń dla systemu kontroli wersji.

---

## Instalacja i konfiguracja

### Wymagania systemowe
* System operacyjny: Linux (zalecany Debian/Ubuntu).
* Język: Python w wersji 3.x.
* Uprawnienia: Wymagany dostęp administratora (root).

### Proces instalacji
1. Nadaj uprawnienia do wykonywania skryptu instalacyjnego:
   chmod +x install.sh
2. Uruchom instalator:
   sudo ./install.sh

Po zakończeniu instalacji system zostanie zarejestrowany jako usługa systemd o nazwie file-monitor.service.

---

## Procedura dodawania plików do monitorowania
Aby system objął ochroną nowy plik, należy przeprowadzić procedurę synchronizacji:

1. Przygotowanie wzorca:
   sudo cp /sciezka/do/pliku /opt/my_monitor/backups/
2. Inicjalizacja pliku roboczego:
   sudo cp /opt/my_monitor/backups/nazwa_pliku /opt/my_monitor/target_dir/
3. Aktualizacja rejestru haszy:
   sudo systemctl restart file-monitor.service
   
---

## Bezpieczeństwo i odpowiedzialność
Uwaga: Narzędzie to jest prototypem o charakterze edukacyjnym (Proof of Concept). Nie zaleca się używania go w środowiskach produkcyjnych bez dodatkowych zabezpieczeń, takich jak szyfrowanie katalogu wzorców czy ograniczenie uprawnień do plików wykonywalnych.
