## Team and roles

| Name | Role |
|------|------|
| Kireet Singh | Undecided |
| Justin P.George | Undecided |
| Brandon | Undecided |


Everyone writes tests and docs for their own module .

---

# PIC24 Lux Logger

A light-intensity (lux) data logger built on the **Microchip PIC24F Starter Kit 1**.
The board reads an ambient light sensor, timestamps each reading, saves it to a **USB stick**,
and lets the user browse logged data **by day** on the OLED screen using the capacitive touch pad.

> University of Siegen. Built on the `pic24fStarter` base project by kvl@eti.uni-siegen.de.

---

## Features

| # | Feature | Status |
|---|---------|--------|
| 1 | Read light sensor via ADC and convert to lux | ⬜ To do |
| 2 | Periodic logging with timestamps (RTCC) | ⬜ To do |
| 3 | Save logs to USB stick (CSV, FAT file system) | ⬜ To do |
| 4 | OLED UI: live lux value + day selection menu | ⬜ To do |
| 5 | Graph of a selected day on the OLED | ⬜ To do |
| 6 | Status feedback on RGB LED (logging / USB error / idle) | ⬜ To do |
| 7 | Potentiometer (slider) to set the log interval or scroll the graph | ⬜ Optional |

Status key: ⬜ To do · 🟨 In progress · ✅ Done

---

## Hardware

| Part | Details |
|------|---------|
| Board | PIC24F Starter Kit 1 (DM240011) |
| MCU | **PIC24FJ256GB106**: 16-bit, 256 KB Flash, 16 KB RAM, USB On-The-Go |
| Display | 128×64 monochrome OLED, SH1101A controller (parallel master port) |
| Input | 5 capacitive touch pads (up / right / down / left / center) read with CTMU, plus a potentiometer |
| Output | RGB LED (PWM) |
| Storage | USB stick on the USB-A host port |
| Sensor | 3-pin ambient light sensor (analog output to an ADC pin). *Record the exact part number and wiring here.* |
| Clock | 12 MHz crystal → 96 MHz PLL → 32 MHz system clock (16 MIPS) |

### Sensor wiring (fill in once confirmed)

| Sensor pin | Board pin | Notes |
|------------|-----------|-------|
| VCC | 3.3 V | |
| GND | GND | |
| OUT | AN? / RB? | Load resistor value: ? Ω |

---

## Software and tools

- **IDE:** [MPLAB X IDE](https://www.microchip.com/en-us/tools-resources/develop/mplab-x-ide)
- **Compiler:** MPLAB XC16 v2.10
- **Programmer/debugger:** on-board starter kit debugger (`SKDEPIC24F1`), connected over the mini-USB port
- **USB host and file system:** Microchip Libraries for Applications (MLA): USB Host MSD + MDD File System (FSIO).
  These library files are **not committed** (see `.gitignore`). Each member copies them into the project folder locally (`USB/`, `MDD File System/`, `FSconfig.h`).

### Getting started

1. Clone the repo:
   ```bash
   git clone https://github.com/<org-or-user>/pic24-lux-logger.git
   ```
2. Open the folder as a project in MPLAB X (`File → Open Project`).
3. Install XC16 v2.10 if MPLAB doesn't find it (`Project Properties → Compiler Toolchain`).
4. Connect the board's mini-USB debug port, then click **Make and Program Device**.
5. Check: the starter demo shows a circle on the OLED that moves when you touch the pads, and the RGB LED changes colour.

---

## Project structure

```
.
├── main.c              # Entry point / main loop
├── PIC24FStarter.h     # Config bits, clock init, includes all drivers
├── SH1101A.c/.h        # OLED display driver (pixels, clear, reset)
├── TouchSense.c/.h     # CTMU touch pads + potentiometer ADC
├── RGBLeds.c/.h        # RGB LED PWM driver
├── nbproject/          # MPLAB X project files (commit these, except private/)
├── Makefile
└── docs/               # Diagrams, datasheets notes, meeting notes
```

Planned new modules:

| Module | Purpose |
|--------|---------|
| `LightSensor.c/.h` | ADC sampling, averaging, raw value → lux conversion |
| `RTC.c/.h` | Real-time clock (RTCC) for date and time, setting the time |
| `Logger.c/.h` | Buffering samples and writing CSV files to USB |
| `UI.c/.h` | Menu and screen states, day picker, graph drawing |
| `Font.c/.h` | Small bitmap font for text and numbers on the OLED |

### Log file format (proposal)

One CSV file per day on the USB stick, e.g. `LUX_20261124.CSV` (8.3 file names):

```
time,lux
08:00:00,312
08:01:00,318
```

---

## Workflow

- `main` always builds and runs on the board. **Do not push directly to `main`.**
- Branch per feature: `feature/light-sensor`, `feature/usb-logging`, `feature/ui-day-select`, …
- Open a pull request, get **1 review**, test it on the board, then merge.
- Commit messages: short and in the imperative, e.g. `Add ADC driver for light sensor`.
- Track tasks in GitHub Issues or Projects. Link each PR to its issue (`Closes #12`).
- Do not commit build output (`build/`, `dist/`, `*.hex`) or MLA library sources.

## Milestones

| Week | Goal |
|------|------|
| 1 | Repo set up, roles assigned, everyone can build and flash the starter demo |
| 2 | Sensor reads raw ADC values and shows them on the OLED; RTC keeps time |
| 3 | Writing a CSV file to the USB stick works |
| 4 | Periodic logging integrated; day-selection UI |
| 5 | Daily graph, error handling, power and robustness tests |
| 6 | Final integration, documentation, presentation |

---

## License

GPL-3.0. See [LICENSE](LICENSE).
