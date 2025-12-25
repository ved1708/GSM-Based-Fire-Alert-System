# 🔥 GSM Based Fire Alert System

## 📖 Table of Contents
1. [Project Overview](#-project-overview)
2. [Features](#-features)
3. [Hardware & Software Requirements](#-hardware--software-requirements)
4. [Circuit Implementation (Wiring)](#-circuit-implementation-wiring)
5. [Step-by-Step Installation Guide](#-step-by-step-installation-guide)
6. [How It Works](#-how-it-works)
7. [References](#-references)

---

## 📌 Project Overview
This project is a fully functional **Fire Alert System** that integrates **GSM technology** with standard fire detection sensors. Unlike traditional alarms that only sound locally, this system provides **remote alerts via SMS and Phone Calls** to designated numbers. It is designed to bridge the gap between fire detection and rapid human response, potentially saving lives and property by notifying owners even when they are away.

## 🚀 Features
* **Real-time Fire Detection:** Uses IR Flame Sensors for immediate detection of open fire.
* **Dual Alert System:**
    * **Local:** Loud Buzzer and Red LED indicators.
    * **Remote:** SMS notification and Phone Call via GSM Network.
* **Stable Power Management:** Utilizes an LM2596 Buck Converter to ensure safe power delivery to the sensitive GSM module or 3.3V can aslo be drawn directly from the Arduino board.
* **System Reset:** Capability to return to monitoring mode after an incident.

---

## 🛠 Hardware & Software Requirements

### Hardware Components
1.  **Microcontroller:** Arduino UNO (ATmega328P)
2.  **Communication:** GSM Module (SIM800L / SIM900A)
3.  **Sensors:** IR Flame Sensor Module
4.  **Power Supply:**
    * 12V 2Amp DC Adapter
5.  **Indicators:**
    * Active Buzzer
    * LEDs (Red for Fire, Green for Power/Status)
6.  **Connecting Wires:** Jumper wires (Male-Male, Male-Female)

### Software Tools
* **Arduino IDE** (for coding and uploading firmware)
* **Library:** `SoftwareSerial.h` (Built-in)

---

## 🔌 Circuit Implementation (Wiring)

The system utilizes a central 12V power source. The wiring is split into two power domains: 5V for Arduino and ~4V for the GSM module.

### 1. Power Setup
* **12V Source** -> Connects to Arduino `Vin` and `GND`.
* **12V Source** -> Connects to **LM2596 Input**.
* **LM2596 Output** -> Adjusted to **4.2V** -> Connects to GSM Module `VCC` and `GND`.
* *Note: Common Ground (GND) must be shared between Arduino, LM2596, and GSM Module.*

### 2. GSM Module (SIM800L/900)
* **TX Pin** -> Arduino Pin **D2** (Software RX)
* **RX Pin** -> Arduino Pin **D3** (Software TX)
* **GND** -> Common GND

### 3. Flame Sensor
* **VCC** -> Arduino 5V
* **GND** -> Arduino GND
* **D0 (Digital Out)** -> Arduino Pin **D4**

### 4. Alert Peripherals
* **Buzzer (+) / LED (+)** -> Arduino Pin **D13**
* **Buzzer (-) / LED (-)** -> Arduino GND

---

## 📝 Step-by-Step Installation Guide

Follow these steps to replicate the project from scratch:

### Step 1: Power Regulation (Critical)
Before connecting the GSM module, you must calibrate the power.
1.  Connect the 12V adapter to the input of the **LM2596 Buck Converter**.
2.  Use a multimeter to measure the output voltage of the LM2596.
3.  Rotate the potentiometer on the LM2596 until the output reads approx **3.7V to 4.2V**.
    * *Warning: Providing >4.4V can destroy the GSM module.*

### Step 2: Hardware Assembly
1.  Insert a valid **SIM Card** into the GSM module.
2.  Wire the components according to the [Circuit Implementation](#-circuit-implementation-wiring) section above.
3.  Ensure the Flame Sensor is positioned facing the potential fire source area.

### Step 3: Programming
1.  Open the Arduino IDE.
2.  Copy the source code from the 'code' file.
3.  **Update the Phone Number:** Look for the line `String phoneNumber = "+91XXXXXXXXXX";` and replace it with your alert number.
4.  Connect the Arduino UNO via USB and upload the code.

### Step 4: Testing
1.  Power up the system using the 12V adapter. Wait for the GSM module network LED to blink slowly (every 3 seconds), indicating a network connection.
2.  Light a lighter or matchstick in front of the **Flame Sensor**.
3.  **Observation:**
    * Buzzer sounds immediately.
    * LED flashes.
    * Within 10-15 seconds, you will receive an **SMS: "Fire Detected! Take Action."** followed by a **Phone Call**.

---

## ⚙️ How It Works
The system runs a continuous loop monitoring the digital output of the Flame Sensor.
1.  **Idle State:** The Arduino reads a logic `HIGH` (or `LOW` depending on sensor calibration) indicating no fire.
2.  **Trigger State:** When IR light from a flame is detected, the sensor output flips.
3.  **Action Sequence:**
    * Arduino detects the change.
    * Triggers digital pin D13 for the buzzer.
    * Uses `SoftwareSerial` to send AT commands (`AT+CMGS` for SMS, `ATD` for Call) to the GSM module.

---

For specific technical details regarding the microcontroller used in this implementation:
* **Arduino UNO Details:** Please refer to **Page 3, Section 4 (Components)** of the `Report No 01.pdf` included in the `docs/` folder.
