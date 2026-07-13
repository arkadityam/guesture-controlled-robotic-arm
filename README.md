# 🦾 Gesture-Controlled Robotic Arm

A minimalist, dual-module embedded system that translates real-time hand gestures and physical button inputs into robotic movements using **I2C communication**[span_1](start_span)[span_1](end_span).

---

## 🎯 System Highlights
* **I2C Protocol:** Reliable Master-Slave inter-board communication[span_2](start_span)[span_2](end_span).
* **Gesture Mapping:** Maps raw MPU6050 IMU spatial data directly to servo angles.
* **Smart Bounds:** Uses software constraints (`constrain()`) to protect servo hardware limits.

---

## 🔌 Hardware Setup

### 🛰️ Master (Glove Controller)
* **MCU:** Arduino Nano / Uno
* **Sensors:** MPU6050 IMU & 1x Push Button
* **Pins:** `A4 (SDA)`, `A5 (SCL)`, `Pin 7 (Button)`

### 🎛️ Slave (Robotic Arm)
* **MCU:** Arduino Nano / Uno
* **Actuators:** 3x PWM Servos
* **Pins:** `Pin 9 (Z-Servo)`, `Pin 8 (Y-Servo)`, `Pin 7 (Gripper)`

> ⚠️ **Pro-Tip:** Connect all **GND** lines together, and use an external 5V/6V power supply for the servos to prevent system resets!

---

## 📊 Data Mapping Profile

Data packets are cleanly serialized into an 8-bit struct (`Address: 0x08`)[span_3](start_span)[span_3](end_span):

| Input Source | Signal Processing Bounds | Physical Output Range | Actuator target |
| :--- | :--- | :--- | :--- |
| **MPU6050 Z-Axis** | Mapped $0-255$ $\rightarrow$ Constrained `40 to 245` | **$50^\circ$ to $160^\circ$** | Base Rotation Servo |
| **MPU6050 Y-Axis** | Mapped $0-255$ $\rightarrow$ Constrained `30 to 205` | **$180^\circ$ to $0^\circ$** | Arm Pitch Servo |
| **Push Button** | Binary State (`0` or `1`)[span_4](start_span)[span_4](end_span) | **$35^\circ$ (Rest) / $90^\circ$ (Pressed)** | Gripper Servo |

---

## 🚀 Quick Start
1. Install `Wire.h`, `Servo.h`, and `MPU6050.h` in your Arduino IDE.
2. Flash the **Master** code to the controller glove.
3. Flash the **Slave** code to the robotic arm hardware.
4. Open the Serial Monitor at **9600 Baud** for live tracking diagnostics!

---
*Built for optimal real-time performance and seamless system integration.* ⚡
