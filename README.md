# Voron Trident 250mm — Klipper Config

## Hardware

| Component | Detail |
|---|---|
| Frame | Voron Trident 250mm |
| Main MCU | BigTreeTech Octopus Pro (STM32F446) |
| Toolhead MCU | BTT EBBCan 36 (STM32G0B1) via CAN bus |
| Extruder | Clockwork 2 (CW2) |
| Hotend | E3D Revo High Flow 60W, 0.4mm nozzle |
| Probe | Voron TAP |
| XY Motors | LDO-42STH48-2504AH (48V via TMC5160) |
| Z Motors | LDO-42STH40-1684L300E (24V via TMC2209) |
| XY Drivers | TMC5160 SPI |
| Z Drivers | TMC2209 UART |
| Bed | 250mm PEI flex plate |
| Display | KlipperScreen touchscreen |
| LEDs | StealthBurner RGBW, WLED chamber strip |
| Filters | Dual Nevermore V6 fans |

## Key IDs

| Item | Value |
|---|---|
| Main MCU serial | `/dev/serial/by-id/usb-Klipper_stm32f446xx_19002D001350565843333620-if00` |
| EBBCan CAN UUID | (check EBBCan.cfg canbus_uuid) |
| IP address | 192.168.1.153 |
| Hostname | trident.local |

## Software

- Klipper + Moonraker + Mainsail
- KlipperScreen
- Klippain ShakeTune
- TMC Autotune (klipper_tmc_autotune)
- Crowsnest (webcam)
- Spoolman (filament tracking — server at 192.168.1.175:7912)
- WLED (chamber strip at trident-lights.local)

## Key Tuning Values — Siraya Tech HT-HF ABS

| Setting | Value |
|---|---|
| Nozzle temp | 246°C |
| Bed temp | 110°C (first layer) / 105°C (subsequent) |
| Pressure advance | 0.043 |
| Flow ratio | 0.93 |
| Max volumetric speed | 14 mm³/s |
| Input shaper X | MZV @ 55.0Hz, damping 0.053 |
| Input shaper Y | EI @ 50.2Hz, damping 0.053 |
| Max accel | 3500 mm/s² |
| XY shrinkage | 99.4% |
| Z shrinkage | 99.2% |

## Config File Map

| File | Purpose |
|---|---|
| `printer.cfg` | Main config — kinematics, steppers, input shaper, autotune |
| `common_macros.cfg` | Shared macros — PRINT_START, PRINT_END, parking, filament |
| `macros.cfg` | Trident-specific — SMART_ZTILT, NEVERMORE, WLED hooks |
| `EBBCan.cfg` | Toolhead MCU — extruder, hotend, ADXL |
| `probe.cfg` | TAP probe config |
| `fans.cfg` | Part fan, hotend fan, Nevermore, electronics fans |
| `bed_heater.cfg` | Heated bed config |
| `thermistor.cfg` | Frame/chamber/EBBCan/RPi temperature sensors |
| `stealthburner_leds.cfg` | StealthBurner RGBW LED states |
| `wled.cfg` | WLED chamber lighting macros |
| `temperature_color.cfg` | KlipperScreen temp color mapping |
| `filament_runout.cfg` | BTT filament motion sensor |
| `config_backup.cfg` | GitHub autocommit shell command + macro |
| `moonraker.conf` | Moonraker server, update manager, WLED, Spoolman |
| `KlipperScreen.conf` | KlipperScreen display settings |
| `autocommit.sh` | Git push script — runs hourly via cron + after every print |

## Recovery

If rebuilding from scratch:
1. Flash Klipper to Octopus Pro (STM32F446, 32KiB bootloader, 12MHz crystal, USB)
2. Flash Katapult + Klipper to EBBCan via USB DFU mode, then switch to CAN
3. Install KIAUH → Klipper + Moonraker + Mainsail + KlipperScreen
4. Install ShakeTune, TMC autotune, Crowsnest via KIAUH or their install scripts
5. Clone this repo to `~/printer_data/config/`
6. Update CAN UUID in EBBCan.cfg (run `~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0`)
7. Update serial IDs in printer.cfg (check `/dev/serial/by-id/`)
8. Run PID calibration, probe calibration, ShakeTune, PA calibration

## Related Repos

- OrcaSlicer profiles: https://github.com/ken-alt/orcaslicer_profiles
- Voron 2.4 config: https://github.com/ken-alt/voron24_configs
