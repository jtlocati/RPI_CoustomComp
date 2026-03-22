# 🔌 Raspberry Pi 5 + Pico + SPI Display + Button Matrix Wiring Guide

---

# 🧠 SYSTEM OVERVIEW

            ┌──────────────────────────────┐
            │        USB-C Power Bank      │
            └──────────────┬───────────────┘
                           │ (USB-C)
                           ▼
                ┌────────────────────┐
                │   Raspberry Pi 5   │
                └────────────────────┘
                     │         │
                     │         │
          (SPI)      │         │ (USB)
                     │         ▼
                     │   ┌──────────────┐
                     │   │   Pico WH    │
                     │   └──────────────┘
                     │          │
                     │          │
                     ▼          ▼
             ┌────────────┐   Button Matrix
             │  SPI LCD   │
             └────────────┘

---

# 🖥️ DISPLAY → RASPBERRY PI 5 (SPI)

| LCD Pin         | Pi 5 GPIO        |
|----------------|------------------|
| VCC            | 3.3V (Pin 1)     |
| GND            | GND (Pin 6)      |
| SCK (CLK)      | GPIO11 (Pin 23)  |
| MOSI           | GPIO10 (Pin 19)  |
| CS             | GPIO8  (Pin 24)  |
| DC             | GPIO25 (Pin 22)  |
| RESET          | GPIO24 (Pin 18)  |
| LED (backlight)| 3.3V (or GPIO)   |

---

# 🔘 PICO → BUTTON MATRIX

## Example 4x5 Matrix

### Rows (Outputs from Pico)

GP0 → Row 1
GP1 → Row 2
GP2 → Row 3
GP3 → Row 4


### Columns (Inputs to Pico)

GP4 → Col 1
GP5 → Col 2
GP6 → Col 3
GP7 → Col 4
GP8 → Col 5


---

# 🔧 BUTTON + DIODE WIRING

Each button connection:


ROW ────┬────[SWITCH]────|>|──── COLUMN
│ diode


### ⚠️ Diode Orientation
- Arrow (`|>|`) points **toward COLUMN**
- Required to prevent ghosting

---

# 🧩 FULL MATRIX EXAMPLE

     COL1   COL2   COL3   COL4   COL5
      │      │      │      │      │
      ▼      ▼      ▼      ▼      ▼

ROW1 ──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┐
ROW2 ──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┤
ROW3 ──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┤
ROW4 ──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┼──[SW]─|>|─┘


---

# 🔌 PICO → RASPBERRY PI 5

## ✅ Recommended: USB (HID or Serial)

Pico USB → Pi USB port


- Acts as:
  - Keyboard (HID)
  - OR Serial device

---

## Optional: UART Connection

Pico GP16 (TX) → Pi GPIO15 (RX)
Pico GP17 (RX) → Pi GPIO14 (TX)
GND → GND


---

# ⚡ POWER DISTRIBUTION


Power Bank → Pi 5 (USB-C)

Pi 5 3.3V → LCD
Pi 5 GND → LCD

Pico powered via USB (from Pi)


### ⚠️ Important
- All components must share **common GND**

---

# 🧠 FINAL ARCHITECTURE


[Buttons]
↓
[Matrix + Diodes]
↓
[Pico WH] ──USB──▶ [Raspberry Pi 5] ──SPI──▶ [LCD]
↑
(scanning + debounce)