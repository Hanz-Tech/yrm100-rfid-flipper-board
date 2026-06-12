---
stepsCompleted: [1]
sessionTopic: YRM100 UHF RFID Flipper Zero Hat PCB
user: Han
project: rfid-flipper
date: 2026-06-11
language: English
---

# Brainstorming Session: YRM100 UHF RFID Flipper Zero Hat PCB

## Session Context

**Facilitator:** BMad Brainstorming Agent  
**User:** Han  
**Project:** rfid-flipper  
**Date:** 2026-06-11  
**Language:** English

---

## Session Topic

**YRM100 UHF RFID Hat PCB for Flipper Zero**

A hat-style PCB that interfaces between a YRM100 UHF RFID module board and a Flipper Zero, providing mechanical mounting (M2-M3 screws) and GPIO connectivity.

---

## Session Objectives

- Validate and refine the existing UART pass-through design
- Explore GPIO breakout features for YRM100 flexibility
- Optimize design for small-batch production (DFM, cost, assembly)
- Achieve plug-and-play user experience

---

## Design Constraints (Confirmed)

- **Core function**: UART pass-through (RX/TX cross, 3.3V, GND)
- **Additional feature**: GPIO breakout pins for YRM100
- **Production target**: Small batch (10–100 units)
- **User experience**: Plug-and-play — no drivers, no switches, no configuration
- **Power gating**: Explicitly excluded — direct connection, YRM100 runs whenever Flipper is on

---

## Decisions Made So Far

### Power
- **Bulk capacitance**: 47 µF / 6.3 V (0805) — transient current supply for YRM100 transmit spikes
- **Decoupling**: 100 nF / 6.3 V (0402) — high-frequency bypass near YRM100 power entry
- **Reverse protection**: BAT54 Schottky diode — prevents backfeed when hat is plugged in with Flipper off
- **UART idle pull-downs**: 2× 10 kΩ on RX/TX lines — clean idle state when YRM100 disconnected
- **Power OK LED**: Green LED + 100 Ω current limiter — visual confirmation hat is powered
- **Current sense test points**: 0.1 Ω sense points on VCC rail for debugging
- **Power gating**: ❌ Excluded — direct connection, no MOSFET switch

### GPIO Breakout — Hybrid Approach (Approach C)
- **YRM100 module pinout**: `5V | GND | TX | RX | EN` (5-pin)
- **YRM100 connector**: 5-pin **JST 1.0 mm pitch**
- **Flipper Zero GPIO**: Two separate 1×8 headers (16 pins total), 2.54 mm pitch
- **Hat connector to Flipper**: Two 1×8 headers, 2.54 mm pitch, ~18 mm row spacing
- **KiCAD library**: Correct header parts already available in the project library
- **Unused pins**: Pin 17 (1-Wire) and Pin 18 (GND) — unused

### Flipper Zero GPIO Pinout (from flipper_pin_out.md)

**LEFT SIDE (Pins 1–8)** — Hat row 1:
| Pin | Signal | Hat Usage |
|-----|--------|-----------|
| 1 | 5V | YRM100 5V |
| 2 | PC0 | Breakout |
| 3 | PC1 | Breakout |
| 4 | PC2 | Breakout |
| 5 | PC3 | Breakout |
| 6 | PB3 | Breakout |
| 7 | PB2 | Breakout |
| 8 | GND | YRM100 GND |

**RIGHT SIDE (Pins 9–18)** — Hat row 2:
| Pin | Signal | Hat Usage |
|-----|--------|-----------|
| 9 | 3.3V | UART logic / EN pull-up |
| 10 | SWD | Breakout |
| 11 | GND | YRM100 GND (common) |
| 12 | SIO | Breakout |
| 13 | TX | → YRM100 RX (crossed) |
| 14 | RX | → YRM100 TX (crossed) |
| 15 | C1 | Breakout |
| 16 | C0 | Breakout |
| 17 | 1 Wire | — (unused) |
| 18 | GND | — (unused) |

### Signal Routing Summary
- **YRM100 5V** ← Flipper Pin 1 (5V)
- **YRM100 GND** ← Flipper Pin 8 / 11 (common ground)
- **YRM100 TX** ← Flipper Pin 14 (RX) — crossed
- **YRM100 RX** ← Flipper Pin 13 (TX) — crossed
- **YRM100 EN** ← pulled up to 3.3V via 10 kΩ (on-board)
- **Breakout pins**: PC0, PC1, PC2, PC3, PB3, PB2 (left) + SWD, SIO, C1, C0 (right) = 10 signals

### Power Architecture (Updated)
- **YRM100 5V supply**: From Flipper GPIO Pin 1 (5V)
- **UART logic**: 3.3V from Flipper GPIO Pin 9 (3.3V)
- **Bulk capacitance**: 47 µF / 6.3 V (0805) — transient current supply for YRM100 transmit spikes
- **Decoupling**: 100 nF / 6.3 V (0402) — high-frequency bypass near YRM100 power entry
- **Reverse protection**: BAT54 Schottky diode on 5V rail — prevents backfeed when hat is plugged in with Flipper off
- **UART idle pull-downs**: 2× 10 kΩ on RX/TX lines — clean idle state when YRM100 disconnected
- **Power OK LED**: Green LED + 100 Ω current limiter — visual confirmation hat is powered
- **Current sense test points**: 0.1 Ω sense points on VCC rail for debugging
- **Power gating**: ❌ Excluded — direct connection, no MOSFET switch

---

## Ideas

_No ideas generated yet._

---

*Session in progress. Next step: Small-Batch Production Optimization*

---

## Decisions Made So Far

### Manufacturing
- **Panelization**: ❌ Not panelized — individual boards only
- **Manufacturing route**: JLCPCB/PCBWay assembly (assumed)
- **Surface finish**: HASL leaded (cheapest, hand-solder friendly)
- **Silkscreen**: White on green
- **PCB layers**: 2-layer, 1.6 mm FR4
- **Board size**: 52 × 28 mm (individual, no panel)

### DFM Rules
- **Power traces (5V, GND)**: 0.5 mm (20 mil) minimum
- **Signal traces (UART)**: 0.25 mm (10 mil) minimum
- **Clearance**: 0.2 mm (8 mil)
- **Via size**: 0.3 mm hole / 0.6 mm pad
- **Test points**: 5 exposed pads (5V, 3.3V, TX, RX, GND)

---

## Session Status

**Brainstorming complete.** Moving to KiCAD implementation phase.

