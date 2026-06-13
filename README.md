# stm32-rfid-smart-trolley
# 🛒 STM32 RFID Smart Trolley

An embedded smart shopping trolley system built on the STM32F103 microcontroller. Items are identified via RFID tags, tracked in real-time on an OLED display, and a final bill is generated on demand — no checkout queue needed.



## ✨ Features

- RFID-based item detection (MFRC522)
- Add and remove items via dedicated push buttons
- Real-time item list and price update on SSD1306 OLED display
- Final bill generation with auto-reset after checkout
- Low-power peripheral control (RFID and OLED power gating)
- Interrupt-driven button handling — no polling delay

---

## 🔧 Hardware

| Component        | Part              |
|-----------------|-------------------|
| Microcontroller | STM32F103C8T6     |
| RFID Reader     | MFRC522 (SPI)     |
| Display         | SSD1306 0.96" OLED (I2C) |
| Buttons         | 3× Tactile Push Button |

---

## 🔌 Pinout / Wiring

### MFRC522 → STM32 (SPI1)

| MFRC522 Pin | STM32 Pin | Function     |
|-------------|-----------|--------------|
| SDA (CS)    | PA4       | Chip Select  |
| SCK         | PA5       | SPI Clock    |
| MOSI        | PA7       | SPI MOSI     |
| MISO        | PA6       | SPI MISO     |
| RST         | PA3       | Reset        |
| 3.3V        | 3.3V      | Power        |
| GND         | GND       | Ground       |

### SSD1306 OLED → STM32 (I2C2)

| OLED Pin | STM32 Pin | Function   |
|----------|-----------|------------|
| SDA      | PB11      | I2C2 SDA   |
| SCL      | PB10      | I2C2 SCL   |
| VCC      | 3.3V      | Power      |
| GND      | GND       | Ground     |

> I2C address: `0x3C`

### Buttons → STM32 (Active LOW, internal pull-up)

| Button  | STM32 Pin | Function          |
|---------|-----------|-------------------|
| ADD     | PB0       | Scan & add item   |
| REMOVE  | PB1       | Scan & remove item|
| BILL    | PA8       | Generate final bill|

---

## 🏗️ Project Structure

```
├── Core/
│   ├── Src/
│   │   └── main.c          # Application logic
│   └── Inc/
├── Drivers/
│   ├── mfrc522.c / .h      # RFID driver
│   ├── ssd1306.c / .h      # OLED driver
│   └── fonts.h             # Font data
└── README.md
```

---

## 🧠 How It Works

1. On boot, a welcome screen is shown on the OLED.
2. Press **ADD** → system enters a 5-second scan window; place an RFID tag to add the item.
3. Press **REMOVE** → same scan window, but decrements the item count.
4. Press **BILL** → final bill is displayed for 5 seconds, then the system resets.
5. Each RFID tag has a hardcoded UID mapped to an item name and price.

---

## 📦 Supported Items (Demo)

| UID (Hex)               | Item     | Price  |
|------------------------|----------|--------|
| `33 B4 E3 59`          | Kurkure  | ₹10    |
| `13 6A FA 29`          | HideSeek | ₹30    |

> To add more items, extend the UID check block in `main.c` inside the `MFRC522_Anticoll` handler.

---

## 👤 Author

**Bhavya** — ECE Student, CHARUSAT University  
_Embedded Systems | VLSI | FPGA_

---

## 📄 License

This project is open for personal and educational use.
