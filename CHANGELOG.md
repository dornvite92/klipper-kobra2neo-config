# CHANGELOG - Anycubic Kobra 2 Neo + Klipper

Dziennik zmian i rozszerzeń funkcjonalności ponad standardową instalację Klipper/Mainsail/Moonraker.

---

## [2026-01-01] Spoolman - Zarządzanie szpulami filamentu

### Dodano
- **Spoolman** - system zarządzania szpulami filamentu
  - Serwis systemd na porcie 7912
  - Integracja z Moonraker (`spoolman_connected: true`)
  - Proxy nginx pod `/spoolman/`
  - Baza SQLite do śledzenia zużycia

### Makra Klipper
- `SET_ACTIVE_SPOOL ID=<n>` - ustawia aktywną szpulę
- `CLEAR_ACTIVE_SPOOL` - usuwa aktywną szpulę

### Konfiguracja
- `moonraker.conf`: sekcja `[spoolman]`
- `makro.cfg`: makra integracji Spoolman
- `spoolman.service`: unit systemd
- nginx: location `/spoolman/` z proxy

---

## [2025-12-31] Filament Dashboard - Panel zużycia filamentu

### Dodano
- **Filament Dashboard React** - zaawansowany panel monitorowania zużycia
  - Stack: React 19 + TypeScript + Vite + Material-UI
  - Wykresy: Recharts (słupkowe, kołowe)
  - Eksport: CSV/Excel (SheetJS)

### Funkcjonalności
- Dashboard z 6 kartami statystyk
- Wykres dziennego zużycia (14 dni)
- Wykres kołowy materiałów
- Historia druków z filtrami (daty, nazwa, materiał, status)
- Sortowanie i paginacja
- Responsywny design (mobile-first)

### Pliki
- `/home/damian/filament_dashboard/` - build produkcyjny
- `/home/damian/filament_dashboard_react/` - kod źródłowy
- nginx: location `/filament/`

---

## [2025-12-27] System profili druku

### Dodano
- **print_profiles.cfg** (288 linii, 26 makr)
  - Profile jakościowe: QUALITY, BALANCED, SPEED, ASSEMBLY
  - Wersje "SAFE" dla trudnych modeli
  - Automatyczne dostosowanie velocity/accel/pressure_advance

### Makra
- `PROFILE_QUALITY` - wysoka jakość (wolniejszy)
- `PROFILE_BALANCED` - zbalansowany (domyślny)
- `PROFILE_SPEED` - szybki druk
- `PROFILE_ASSEMBLY` - części mechaniczne
- `PROFILE_*_SAFE` - bezpieczne wersje z niższą prędkością
- `PROFILE_STATUS` - pokazuje aktywny profil
- `PROFILE_LIST` - lista dostępnych profili
- `PROFILE_RESET` - reset do domyślnych

### Dodano
- **temp_profiles.cfg** (230 linii, 19 makr)
  - Profile temperatur dla materiałów
  - Integracja z START_PRINT

### Makra temperatur
- `TEMP_SET_PLA`, `TEMP_SET_PLA_HOT`, `TEMP_SET_PLA_COLD`
- `TEMP_SET_PETG`, `TEMP_SET_PETG_HOT`
- `TEMP_SET_TPU`, `TEMP_SET_ABS`, `TEMP_SET_ASA`
- `TEMP_STATUS` - pokazuje aktualny profil
- `TEMP_COOLDOWN` - schładzanie

---

## [2025-12-25] Makra startowe i kalibracyjne

### Dodano
- **makro.cfg** - główne makra użytkownika (395 linii, 17 makr)

### Makra START/END
- `START_PRINT` - inteligentny start z:
  - Adaptive Mesh Bed Leveling
  - Automatyczny dobór profilu materiału
  - Pauza na aplikację adhezji (opcjonalna)
  - Linia czyszcząca dostosowana do materiału
- `END_PRINT` - bezpieczne zakończenie z chłodzeniem

### Makra kalibracyjne
- `PLA_KALIBRACJA` - pełna kalibracja PID + Mesh dla PLA
- `PETG_KALIBRACJA` - j.w. dla PETG
- `TPU_KALIBRACJA` - j.w. dla TPU
- `ABS_KALIBRACJA` - j.w. dla ABS
- `ASA_KALIBRACJA` - j.w. dla ASA

### Narzędzia serwisowe
- `POZIOMOWANIE_SRUBY` - asystent poziomowania mechanicznego
- `CZYSZCZENIE_DYSZY` - test ekstruzji/grzania
- `EMERGENCY_STOP` - zatrzymanie awaryjne

### Sterowanie warstwami
- `LAYER_CHANGE` - wywoływane przez slicer
- `SET_FAN_FOR_LAYER` - automatyczne sterowanie wentylatorem

---

## [2025-12-25] Obsługa filamentu

### Dodano
- **scripts.cfg** (156 linii, 4 makra)

### Makra
- `LOAD_FILAMENT` - ładowanie filamentu z grzaniem
- `UNLOAD_FILAMENT` - wyładowanie filamentu
- `DRY_FILAMENT` - suszenie filamentu (grzanie stołu przez czas)
- `CANCEL_DRY_FILAMENT` - anulowanie suszenia

---

## [2025-12-XX] Integracje zewnętrzne

### Obico (AI Monitoring)
- Zdalne monitorowanie druku
- Wykrywanie błędów przez AI (spaghetti detection)
- Powiadomienia push
- Pliki: `moonraker_obico_macros.cfg`, `moonraker-obico-update.cfg`

### OctoEverywhere
- Zdalny dostęp przez chmurę
- Tunel bez VPN/port forwarding
- Pliki: `octoeverywhere-system.cfg`

### Crowsnest (Kamera)
- Streaming MJPG na porcie 8080
- Kamera RPi 5MP
- nginx proxy: `/webcam/`

### Sonar
- Beacon/sonar sensor support
- Update manager w moonraker.conf

---

## [2025-12-XX] Input Shaper - Kompensacja rezonansów

### Konfiguracja
```ini
[input_shaper]
shaper_freq_x: 54.8   # Hz
shaper_type_x: ei     # Extra Insensitive
shaper_freq_y: 34.0   # Hz
shaper_type_y: mzv    # Modified Zero Vibration
```

### Limity
- Max accel X: ~6000 mm/s²
- Max accel Y: ~3500 mm/s² (limitujący)
- Bezpieczne max_accel: 3500 mm/s²

---

## [2025-12-XX] Konfiguracja bazowa

### printer.cfg (407 linii)
- Kinematyka kartezjańska zoptymalizowana
- Stepper drivers z TMC UART
- Probe offset skalibrowany
- Bed mesh 9x9
- Exclude objects enabled
- Firmware retraction

### Parametry drukarki
| Parametr | Wartość |
|----------|---------|
| max_velocity | 250 mm/s |
| max_accel | 3500 mm/s² |
| square_corner_velocity | 8.0 mm/s |
| max_z_velocity | 8 mm/s |

---

## Podsumowanie rozszerzeń

### Statystyki
- **Pliki konfiguracyjne:** 11 (.cfg)
- **Łączna liczba linii:** ~1500+
- **Makra G-code:** 56+
- **Serwisy dodatkowe:** 4 (Spoolman, Obico, OctoEverywhere, Crowsnest)
- **Dashboardy web:** 2 (Mainsail + Filament Dashboard)

### Funkcjonalności ponad standard
1. ✅ Input Shaper (kalibracja ADXL345)
2. ✅ Adaptive Bed Mesh
3. ✅ Profile druku (jakość/prędkość/zbalansowany)
4. ✅ Profile temperatur dla materiałów
5. ✅ Automatyczne kalibracje PID per materiał
6. ✅ Inteligentne makra START/END
7. ✅ Zarządzanie filamentem (load/unload/dry)
8. ✅ Spoolman - tracking szpul
9. ✅ Filament Dashboard - statystyki zużycia
10. ✅ AI monitoring (Obico)
11. ✅ Zdalny dostęp (OctoEverywhere)
12. ✅ Streaming kamery (Crowsnest)
13. ✅ Automatyczny backup git

---

## Struktura repozytorium

```
klipper-kobra2neo-config/
├── printer.cfg          # Główna konfiguracja
├── makro.cfg            # Makra START/END/kalibracje
├── scripts.cfg          # LOAD/UNLOAD/DRY filament
├── print_profiles.cfg   # Profile jakości druku
├── temp_profiles.cfg    # Profile temperatur
├── moonraker.conf       # Konfiguracja API
├── CHANGELOG.md         # Ten plik
└── README.md            # Dokumentacja
```

---

*Ostatnia aktualizacja: 2026-01-01*
*Autor: Damian Misko via Claude Code*
