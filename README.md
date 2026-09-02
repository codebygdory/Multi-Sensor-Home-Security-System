# Self-Calibrating Multi-Sensor Intrusion Detection System

An advanced embedded security platform built on the ATmega328P architecture featuring runtime self-calibration, configurable sensitivity profiles, and noise-immune threshold filtering.

## 📌 Technical Highlights
Unlike standard threshold-based security systems, this platform implements advanced digital signal processing and state-management techniques at the firmware level to ensure maximum reliability and noise reduction.

* **Automated Self-Calibration:** Upon boot, the system samples the ambient light environment via the photoresistor (LDR) to establish a baseline lux reference point, adapting automatically to different room lighting environments.
* **Hysteresis-Based Filtering:** Prevents "relay chatter" and false alarms caused by minor sensor noise or fluctuating environmental light by utilizing dual-threshold boundaries for state transitions.
* **Dynamic Sensitivity Modes:** Allows users to cycle through configurable operational profiles (e.g., Low, Medium, High sensitivity) to scale sensor response windows dynamically.
* **Non-Blocking Telemetry:** Utilizes a custom timer/state loop to execute a Morse Code SOS visual/audio alarm sequence without locking up sensor polling routines.

## System Architecture & Components

| Component | Function / Purpose | Interface / Protocol |
| :--- | :--- | :--- |
| **Arduino Uno** | Executes core logic, threshold filtering, and state machine | N/A |
| **Ultrasonic Sensor (HC-SR04)** | Measures spatial depth variations for intrusion tracking | Digital GPIO (Pulse Timing) |
| **Photoresistor (LDR)** | Captures ambient light lux changes relative to baseline | Analog Input (10-bit ADC) |
| **LCD Display (16x2)** | Displays live system states, active mode, and telemetry | I2C (or Parallel 4-bit) |
| **Buzzer / LED Grid** | Outputs the non-blocking Morse Code SOS alert pattern | Digital Output |

## Engineering Implementation Details

### 1. Hysteresis Filtering Implementation
To eliminate false alarms caused by sensor noise near the trigger boundary, the firmware utilizes a dual-threshold hysteresis loop. An alert is only tripped when a reading passes the high threshold, and cannot reset until the signal clears the lower safety boundary.

### 2. Morse Code SOS Sequence (Non-Blocking)
Instead of using standard `delay()` functions—which blind the system to sensor inputs during an alert—the Morse Code sequence is written using state machines and timestamp tracking (`millis()`). The system pulses the SOS pattern (`... --- ...`) seamlessly while maintaining active background tracking loops.

## How to Replicate and Test
1. Clone this repository to your local machine.
2. Open your primary `.ino` file inside your development environment.
3. Wire the physical components based on your designated I/O pin configurations.
4. Flash the code to your Arduino Uno.
5. Open the Serial Monitor at `9600 baud` to view raw debugging telemetry.
