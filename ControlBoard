## Controller Board PCB — FABX CNC Controller V1.0

### Overview

Off-the-shelf CNC controller boards are designed for general-purpose machines and fall short when a project demands control over a large number of axes and heterogeneous actuators simultaneously. For our hybrid CNC/Pick-and-Place machine, we required control over **10 stepper motors** alongside a diverse set of peripherals — IR optical endstops, a suction pump relay, a hotplate SSR, and a BTS7960-based DC spindle driver — a combination no commercially available board could satisfy.

This led us to design a **custom control board from scratch**, built around the STM32F103C8T6 ("Blue Pill") microcontroller.

---

### Design Philosophy

Rather than designing two entirely separate boards for the CNC router and the Pick-and-Place machine, we adopted a **unified, modular design strategy**:

- A single PCB layout accommodates the full I/O requirements of **both machines**
- The board is populated differently per use case — 6 stepper driver slots serve the PnP head, while 4 serve the CNC router
- This approach leverages the 5-unit minimum of PCB fabrication orders, eliminating waste and reducing unit cost

Both machines share the same hardware design; firmware and connector population differentiate their roles.

---

### Key Features

| Feature | Details |
|---|---|
| **Microcontroller** | STM32F103C8T6 (Blue Pill) |
| **Stepper Drivers** | 6× A4988 sockets (scalable to combined 10-axis use) |
| **Spindle Driver** | BTS7960 H-Bridge (300W DC motor) |
| **Pump Control** | Relay-driven output (optoisolated) |
| **Hotplate Control** | SSR (Solid-State Relay) output |
| **Endstops** | 6× optical limit switch inputs (X/Y/Z, dual per axis) |
| **Thermistor Inputs** | 3× NTC thermistor channels (TH_A, TH_B, TH_C) |
| **Communication** | USB (Full-speed via USB-B), I²C header |
| **Power** | 12V input → onboard Mini360 buck to 5V → AMS1117 to 3.3V |
| **Indicators** | Motion LEDs (X/Y/Z), power LEDs (logic/module/stepper), dual buzzers |

---

### Known Issue & Workaround — Shared Endstop Pins

To conserve GPIO pins on the STM32, the left and right limit switches on each axis were initially routed to a **shared signal pin** (e.g., both X-axis endstops on a single input). This caused the sensor outputs to interfere with each other when both were active, as one could back-drive the other.

**Workaround (V1.0):** 1N4148 signal diodes were hand-soldered onto each sensor's signal wire, allowing current to flow only toward the microcontroller and preventing back-driving between sensors.

**Resolution (planned V2.0):** Each limit switch will be assigned a dedicated GPIO pin, eliminating the need for the diode fix entirely.

---

### Hardware Files

| File | Description |
|---|---|
| `Dual_Controller_Schematic.pdf` | Full 10-sheet KiCad schematic |
| `CB_V1.jpeg` | Assembled boards (two units) |
| `CB_V1_3D.png` | PCB 3D render (top layer) |
| `CB_V1_TL.png` | PCB layout — top copper layer |
| `CB_V1_BL.png` | PCB layout — bottom copper layer |

---
