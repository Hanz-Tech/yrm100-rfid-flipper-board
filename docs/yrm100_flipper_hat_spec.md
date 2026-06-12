# YRM100 to Flipper Zero Hat Adapter (Top-Mount)

## Goal

Build a compact adapter board inspired by the form factor shown in `white_3.JPG` — a low-profile, stacked arrangement featuring:

- **Thin adapter PCB** stacked directly on the Flipper Zero
- **Low-profile GPIO header** connecting the adapter to Flipper (minimal vertical spacing)
- **Short cable loop** from adapter to YRM100 module above
- **Minimal overall stack height** — components lay flat with little vertical buildup

## Electrical Mapping

Use 3.3V logic and cross UART TX/RX.

| YRM100 | Flipper Zero GPIO | Notes |
|--------|-------------------|-------|
| VCC | +3.3V (pin 9) | Do not use 5V for UART logic |
| GND | GND (pin 8) | Common ground |
| TX | UART RX (PA6, pin 3) | Crossed UART |
| RX | UART TX (PA7, pin 2) | Crossed UART |
| EN (if present) | Pull-up to 3.3V via 10k or optional GPIO | Keep enabled by default |

## Connectors

### 1. Flipper side (bottom)
- 1x9 male 2.54 mm pin header, straight, through-hole
- Pins point downward into Flipper GPIO socket

### 2. YRM100 side (top)
- **Primary**: 1x4 JST-PH 2.0 mm vertical through-hole (RX, TX, VCC, GND)
- **Optional fallback footprint in parallel**: 1x5 2.54 mm header (VCC, TXD, RXD, EN, GND)

> **Reason for dual option**: YRM100 variants in the field use either a 4-wire JST cable or a 5-pin breakout harness.

## Board Outline (recommended)

- Board size: 52 mm x 28 mm
- Corner radius: 2 mm
- 2-layer PCB, 1.6 mm FR4

This size is still compact but gives room for mounting slots and strain relief.

## Universal Top-Mount Mechanical Pattern

Because YRM100 board sizes differ by seller, use slots (not fixed holes):

### Left slot rail
- Slot A center: (8.0, 8.0) mm
- Slot B center: (8.0, 20.0) mm
- Slot size: 8.0 mm x 3.2 mm (for M2/M2.5 hardware)

### Right slot rail
- Slot C center: (44.0, 8.0) mm
- Slot D center: (44.0, 20.0) mm
- Slot size: 8.0 mm x 3.2 mm

Coordinate origin: lower-left board corner at (0,0).

This gives adjustable mounting width and lets you clamp different YRM100 board hole spacings.

## Placement Targets

### 1. Flipper 1x9 header
- Place along X = 26.0 mm centerline
- First pin center at (26.0, 3.5) mm
- Pin pitch 2.54 mm upward in +Y

### 2. YRM100 JST connector
- Place near top edge for short cable loop
- Suggested center: (26.0, 24.0) mm
- Key notch facing outward (toward top edge)

### 3. Optional 5-pin fallback header
- Place parallel and 6.0 mm behind JST
- Suggested center: (26.0, 17.5) mm

## Power Integrity

Place near YRM100 power entry:
- C1: 100 nF ceramic between 3.3V and GND
- C2: 22 µF bulk capacitor between 3.3V and GND
- Keep both within 8 mm of YRM100 power pins

## Routing Rules

- **Trace width**:
  - Power: 0.50 mm
  - UART/data: 0.25 mm
- **Clearance**: 0.20 mm minimum
- Keep UART traces short and avoid crossing under slot areas
- Add GND copper pour on both layers

## Silkscreen

Add clear labels on top side:
- YRM100: RX TX 3V3 GND EN
- Flipper side arrow: PIN 1 and PIN 9
- Warning text: "3.3V UART ONLY"

## Assembly Stack (low height)

1. Bottom: short male GPIO pins to Flipper
2. Mid: adapter PCB
3. Top: JST connector and YRM100 board
4. Hardware: M2 nylon standoffs, 4-6 mm length

## Validation Checklist

### 1. Continuity
- YRM100 TX to Flipper PA6
- YRM100 RX to Flipper PA7
- 3.3V and GND correct

### 2. No shorts
- 3.3V to GND resistance check before power-on

### 3. Fit check
- Board inserts fully into Flipper GPIO
- No collision with Flipper shell
- YRM100 can be bolted using slot rails

## Important Note

Large AliExpress dimension variants are often antenna size options, not always the controller PCB footprint. Using slot rails plus a cable entry connector is the most reliable way to support variant boards without redesigning the adapter each time.
