# Klipper Config - Anycubic Kobra 2 Neo

> Kompletna konfiguracja Klipper dla drukarki Anycubic Kobra 2 Neo z płytą Trigorilla 4.0.1.

![Klipper](https://img.shields.io/badge/Klipper-Firmware-red)
![Moonraker](https://img.shields.io/badge/Moonraker-API-blue)
![Mainsail](https://img.shields.io/badge/Mainsail-UI-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

## Specyfikacja

| Komponent | Wartość |
|-----------|---------|
| **Drukarka** | Anycubic Kobra 2 Neo |
| **Płyta główna** | Trigorilla 4.0.1 |
| **SBC** | Raspberry Pi 4B+ (4GB) |
| **Kamera** | RPi Camera 5MP (Crowsnest) |
| **Firmware** | Klipper + Moonraker |
| **UI** | Mainsail |

## Input Shaper (przetestowane)

| Oś | Częstotliwość | Typ filtra | Max accel |
|----|---------------|------------|-----------|
| X | 54.8 Hz | EI | ~6000 mm/s² |
| Y | 34.0 Hz | MZV | ~3500 mm/s² |

## Struktura plików

```
config/
├── printer.cfg           # Konfiguracja główna (MCU, kinematyka, steppery)
├── makro.cfg             # Makra użytkownika (START_PRINT, END_PRINT, kalibracje)
├── scripts.cfg           # Obsługa filamentu (LOAD/UNLOAD/DRY)
├── pause_resume.cfg      # Makra pauzy i wznowienia
├── print_profiles.cfg    # Profile druku (jakość/prędkość)
├── temp_profiles.cfg     # Profile temperatur dla materiałów
├── filament_stats.cfg    # Statystyki zużycia filamentu
├── mainsail.cfg          # Konfiguracja UI Mainsail
├── moonraker.conf        # Konfiguracja API Moonraker
├── crowsnest.conf        # Konfiguracja kamery
├── sonar.conf            # Konfiguracja sieci
├── moonraker-obico.cfg   # Integracja Obico (AI monitoring)
├── octoeverywhere.conf   # Integracja OctoEverywhere
└── timelapse.cfg         # Konfiguracja timelapse
```

## Funkcjonalności

- **Adaptive Mesh Bed Leveling** - automatyczne dostosowanie siatki do rozmiaru wydruku
- **Input Shaper** - kompensacja rezonansów (przetestowane z ADXL345)
- **Pressure Advance** - optymalizacja dla PLA/PETG/TPU
- **Smart macros** - START_PRINT z automatycznym nagrzewaniem i kalibracją
- **Filament management** - LOAD/UNLOAD/DRY z kontrolą temperatury
- **Multi-material profiles** - gotowe profile dla PLA, PETG, TPU, ABS, ASA

## Profile materiałów

| Materiał | Dysza | Stół | Fan | Pressure Advance |
|----------|-------|------|-----|------------------|
| PLA | 200-210°C | 60°C | 100% | 0.032 |
| PETG | 235-245°C | 80°C | 50-70% | 0.045 |
| TPU | 220-230°C | 50°C | 30-50% | 0.08 |
| ABS | 245-255°C | 100°C | 0-30% | 0.04 |

## Instalacja

1. Sklonuj repozytorium do `~/printer_data/config/`:
```bash
cd ~/printer_data
rm -rf config  # lub zrób backup
git clone https://github.com/dornvite92/klipper-kobra2neo-config.git config
```

2. Dostosuj serial MCU w `printer.cfg`:
```bash
ls /dev/serial/by-id/
# Zaktualizuj linię "serial:" w printer.cfg
```

3. Zrestartuj Klipper:
```bash
sudo systemctl restart klipper
```

## Integracje

- **Obico** - AI monitoring i detekcja błędów
- **OctoEverywhere** - zdalny dostęp
- **Crowsnest** - streaming kamery
- **Moonraker Timelapse** - automatyczne timelapse

## Backup

Repozytorium jest automatycznie synchronizowane z GitHub co godzinę (cron job).

## Autor

**Damian Misko** (dornvite92)

## Licencja

MIT License - używaj i modyfikuj dowolnie.

---

*Konfiguracja optymalizowana dla jakości i niezawodności, nie maksymalnej prędkości.*
