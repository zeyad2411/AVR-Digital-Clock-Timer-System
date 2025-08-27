

# 🕒 AVR Digital Clock & Timer System

A **Real-Time Clock (RTC) and Timer System** implemented on **ATmega32 AVR microcontroller**.
This project integrates a **DS1307/DS3231 RTC module**, **LCD display**, **keypad input**, and **buzzer output** to provide:

* ⏰ Real-Time Clock (Date & Time)
* ⏱ Stopwatch
* ⏳ Countdown Timer
* 🔔 Alarm System

The project demonstrates **embedded C programming**, **AVR peripherals**, and **modular driver design** (LCD, DIO, I²C, RTC, Timer).

---

## 📂 Project Structure

```
📦 AVR-Clock-Timer-System
├── main.c             # Main program flow (menu system, mode switching)
├── lcd.c / lcd.h      # LCD driver (16x2 display via 4-bit mode)
├── dio.c / dio.h      # Digital I/O driver
├── i2c.c / i2c.h      # I2C/TWI driver (for RTC communication)
├── rtc.c / rtc.h      # Real-Time Clock module (time/date read/write)
├── timer.c / timer.h  # Countdown timer with ISR and buzzer control
├── keypad.c / keypad.h# Keypad driver for user input
└── README.md          # Documentation
```

---

## ⚙️ Features

### 🕒 Real-Time Clock

* Uses **DS1307/DS3231 RTC chip** via I²C protocol.
* Supports setting & retrieving **hours, minutes, seconds**.
* Supports setting & retrieving **day, month, year**.
* BCD <-> Decimal conversion handled internally.

### ⏳ Countdown Timer

* Configurable **hours, minutes, seconds**.
* Uses **Timer1 in CTC mode** (1-second tick @ 16 MHz, prescaler 1024).
* **ISR-based decrement** of time.
* Auto-triggers buzzer when finished.
* LCD continuously shows **time left**.

### ⏱ Stopwatch

* Start / Stop / Reset functionality.
* Accurate to **1 second** using Timer1 interrupts.
* Time displayed in `HH:MM:SS` format.

### 🔔 Alarm

* Set alarm time from keypad.
* Triggers **buzzer for 5 seconds** at the set time.
* Uses **RTC time comparison** for accuracy.

### 📟 User Interface

* **16x2 LCD** for displaying menus, time, and results.
* **4x4 keypad** for entering values (time, date, alarm).
* **Buzzer** feedback for alarms & countdown completion.

---

## 🛠️ Hardware Requirements

* **Microcontroller**: ATmega32 (16 MHz)
* **RTC Module**: DS1307 / DS3231 with coin cell battery
* **LCD Display**: 16x2 (HD44780-compatible, 4-bit mode)
* **Keypad**: 4x4 matrix keypad
* **Buzzer**: Active buzzer connected via DIO
* **Power**: 5V regulated supply

**Pin Connections (example):**

| Module    | Pin (ATmega32)       | Notes         |
| --------- | -------------------- | ------------- |
| LCD       | PORTC (RS,EN,D4-D7)  | 4-bit mode    |
| Keypad    | PORTD                | 4x4 matrix    |
| RTC (I²C) | PC0 (SCL), PC1 (SDA) | With pull-ups |
| Buzzer    | PB0                  | Active High   |

---

## 🧑‍💻 Software Requirements

* **AVR Toolchain** (avr-gcc, avr-libc)
* **AVRDUDE** (for flashing HEX to microcontroller)
* **Proteus** (for simulation, optional)

---

## ▶️ How to Build & Run

### 1. Compile

```bash
avr-gcc -mmcu=atmega32 -Os main.c lcd.c dio.c i2c.c rtc.c timer.c keypad.c -o clock.elf
avr-objcopy -O ihex clock.elf clock.hex
```

### 2. Flash to Microcontroller

```bash
avrdude -c usbasp -p m32 -U flash:w:clock.hex
```

### 3. Run

* Power the board (5V).
* LCD will display main menu.
* Use keypad to select:

  * `1` → View/Set Time
  * `2` → Stopwatch
  * `3` → Timer
  * `4` → Alarm

---

## 📋 Example Workflows

### ✅ Setting Time

1. Enter **Admin menu** via keypad.
2. Select **Set Time**.
3. Enter `HH:MM:SS` → saved to RTC.

### ✅ Countdown Timer

1. Select **Timer** from menu.
2. Enter duration (H\:M\:S).
3. Timer starts; countdown displayed.
4. Buzzer rings for 5 sec when finished.

### ✅ Stopwatch

1. Select **Stopwatch**.
2. Start/Stop using keypad key.
3. Reset to `00:00:00`.

### ✅ Alarm

1. Select **Alarm**.
2. Enter time for alarm.
3. When RTC matches alarm → buzzer rings.

---

## 📅 Implementation Details

* **RTC Communication**: I²C protocol with BCD-coded registers.
* **Timer**: AVR Timer1 in **CTC Mode** (`OCR1A = 15625`) → 1s tick.
* **Interrupts**:

  * `ISR(TIMER1_COMPA_vect)` handles stopwatch/timer decrement.
* **LCD Handling**: Custom driver for 4-bit commands & data.
* **Keypad Scanning**: Row-column technique with debouncing.

---

## 🚀 Future Improvements

* Store multiple alarms in EEPROM.
* Add **temperature sensor (DS3231 built-in)** display.
* Implement **UI navigation with menus**.
* Add **low-power sleep mode** for battery operation.

---


