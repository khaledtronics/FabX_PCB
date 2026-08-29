# FabX — PCB Fabrication & Assembly Machine

> *"A mini PCB factory that said 'why walk between machines when I can just… not?'"*

FabX is a fully automated PCB fabrication and assembly system developed as a graduation project. It takes a blank copper-clad board through the entire manufacturing pipeline — milling, solder paste dispensing, component placement, and reflow soldering — with no human intervention between steps.

What typically requires three separate machines, a conveyor, and a lot of floor space has been consolidated into a single, compact dual-function platform.

---

## The Problem

A complete PCB manufacturing workflow normally involves:

1. A **CNC router** to mill the copper traces
2. A **Pick-and-Place machine** to populate the board with components
3. A **reflow oven or hotplate** to solder everything in place

These are three different machines, three different footprints, and three manual handoff steps. For small-scale or in-house production, this is a significant barrier — in cost, space, and complexity.

**FabX collapses all three stages into one machine.**

---

## The Solution — Dual-Mode, Single Platform

The core insight driving the mechanical design is that all three tasks are **sequential, never simultaneous**. A PCB being milled isn't being populated. A board being reflowed isn't being routed. This means the same physical space and motion system can serve multiple purposes — you just need to switch modes intelligently.

### Mechanical Architecture

FabX uses a **dual X-axis design**: two independent tool heads share the same Y and Z infrastructure but operate separately depending on the current stage of fabrication.

| Head | Role | Motion System | Why |
|---|---|---|---|
| **Router Head** | Milling copper traces | Lead screws | Rigidity and positional accuracy under cutting load |
| **PnP Head** | Paste dispensing + component placement | GT2 timing belts | High-speed traversal across the board |

Both heads ride on **MGN12 linear profile rails**, chosen for their rigidity, low friction, and precision — ensuring that neither the router's vibration nor the PnP head's rapid movements compromise positional accuracy.

### Process Flow

```
[Blank PCB] → Milling (Router Head) → Solder Paste Dispensing (PnP Head) → Component Placement (PnP Head) → Reflow (Hotplate) → [Finished PCB]
```

All steps occur on the same machine bed, with the Raspberry Pi 5 orchestrating the handoff between modes automatically.

---

## Electronics & Control

The machine is controlled by a **Raspberry Pi 5** acting as the central coordinator, communicating with two custom-designed controller PCBs — one per machine head.

Each controller board is purpose-built for its role but shares a unified hardware design (see the [Control PCB README](./ControlBoard/README.md) for full details). The shared design allowed both boards to be manufactured from the same Gerber files, keeping fabrication costs down.

### Control Stack Summary

```
Raspberry Pi 5
├── CNC Controller PCB  (STM32F103 — 4× A4988 stepper drivers, spindle, SSR, thermistors, endstops)
└── PnP Controller PCB  (STM32F103 — 6× A4988 stepper drivers, pump relay, PCA9685, endstops)
```

---

## Key Subsystems

### 🔩 CNC Router
- Mills PCB copper traces directly from Gerber files
- Lead-screw-driven X axis for rigid, vibration-resistant cutting
- Controlled via custom controller PCB with A4988 stepper drivers
- BTS7960 H-bridge for 300W DC spindle motor

### 🦾 Pick-and-Place Head
- Belt-driven for rapid positioning across the board
- Vacuum nozzle with suction pump (relay-controlled)
- Solder paste dispenser integrated into the same head
- Optical endstops on all axes for homing precision

### 🌡️ Reflow Soldering
- Hotplate-based reflow controlled via SSR (Solid-State Relay)
- Three NTC thermistor channels for temperature monitoring
- Controlled reflow profile managed by the Raspberry Pi

---

## Repository Structure

```
FabX/
├── ControlBoard/         # Custom PCB controller — schematics, layout, BOM
├── FabricationFiles/     # STL & DXF files — custom parts with cad added soon
├── Firmware/             # STM32 firmware for both controller boards
├── Software/             # Raspberry Pi control software
└── Docs/                 # Wiring diagrams, calibration guides, build notes (in progress)
```

---

## Project Status

- [x] Mechanical design & assembly
- [x] Custom controller PCB — designed, fabricated, and validated
- [x] CNC milling — operational
- [x] Pick-and-place head — operational
- [x] Solder paste dispensing — operational
- [x] Hotplate reflow — operational
- [ ] Full end-to-end automated pipeline (in progress)

---

## Team

Built with a lot of coffee and questionable sleep schedules by:

| Name | Emails |
|---|---|
| Abd El Hameed Nasr | abdelhameednasr3344@gmail.com |
| Ali Loai | xperia1v72@gmail.com |
| Kamelia Diaa | Komeiliadiaa@gmail.com |
| Khaled Nasr | knasrr14@gmail.com |
| Mohamed El Sabah | .com |
| Saif El Dein Ayman | saifeldein27@gmail.com |
| Youssef Tamer | yousseftamerieee@gmail.com |

*Graduation Project — 2025/2026*

---

## License

This project is open-source. Hardware designs, firmware, and software are released for educational and non-commercial use. See `LICENSE` for details.
