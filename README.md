
🦾 Gesture-Controlled Robotic Arm via I2C Communication
An advanced, dual-module embedded system that translates real-time hand gestures and physical button inputs into precise electromechanical robotic movements. The system utilizes an MPU6050 Inertial Measurement Unit (IMU) to track spatial orientation, processing the data via an Arduino Master unit before transmitting packetized data over an I2C serial bus to an Arduino Slave that drives three dedicated servo systems.
📌 Features
• 🔄 I2C Master-Slave Architecture: Efficient inter-board communication utilizing data packet serialization/deserialization.
• 📐 IMU Gesture Mapping: Uses an MPU6050 accelerometer/gyroscope to dynamically map tilt data (X, Y, Z axes) into 8-bit values (0-255).
• 🦾 Multi-Axis Servo Kinematics: Implements hardware constraint filtering (constrain()) and linear interpolation (map()) for smooth physical motion.
• 🔘 End-Effector/Gripper Control: Integrated digital push-button logic to toggle an end-effector servo between holding and resting states.
• 📊 Telemetry Feedback: Real-time debugging outputs sent over UART to the hardware serial monitor on both control modules.
🔌 Hardware Architecture & Wiring
🛰️ Master Module (Glove/Controller)
• Microcontroller: Arduino (e.g., Uno/Nano)
• Sensors: MPU6050 (6-axis IMU), 1x Push Button
• Pin Configuration: • A4 (SDA) ➡️ Connected to Slave SDA & MPU6050 SDA • A5 (SCL) ➡️ Connected to Slave SCL & MPU6050 SCL • Pin 7 ➡️ Push Button (Configured with internal INPUT_PULLUP) • Pin 13 ➡️ Local Status LED
🎛️ Slave Module (Robotic Arm Assembly)
• Microcontroller: Arduino (e.g., Uno/Nano)
• Actuators: 3x PWM Servo Motors
• Pin Configuration: • Pin 9 ➡️ Z-Axis Base/Rotation Servo • Pin 8 ➡️ Y-Axis Pitch/Elevation Servo • Pin 7 ➡️ Button-Controlled Gripper Servo • Built-in LED ➡️ Telemetry Sync Indicator
⚠️ Important Note on Wiring: Ensure all sub-systems share a Common Ground (GND). If the servos draw high current, power them using an external 5V/6V DC supply rather than the Arduino's onboard regulator to prevent system brownouts.
📦 Data Packet Structure
The two microcontrollers communicate deterministically by passing a structured byte-array packet over the I2C bus (Address: 0x08):
struct MyData {
  byte X;           // MPU6050 X-axis mapped data (0-255)
  byte Y;           // MPU6050 Y-axis mapped data (0-255)
  byte Z;           // MPU6050 Z-axis mapped data (0-255)
  byte buttonState; // Digital state: 1 = Pressed (Active), 0 = Released
};

🛠️ Software Overview
1. Master Controller Logic
• Polling cycle reads digital and inertial registers.
• Maps raw data from the accelerometer bounds (-17000 to +17000) down to a clean 0-255 format to fit perfectly within standard byte parameters.
• Direct byte-pointer serialization packages the MyData struct and broadcasts it over I2C every 500 ms.
2. Slave Actuator Logic
• Operates on an asynchronous interrupt event (Wire.onReceive(receiveEvent)) to prevent blocking the main loop execution.
• Kinematic Actuation Profiles: • Z-Axis Servo: Constrains input array between 40-245, mapping to physical safe operating limits of 50° to 160°. • Y-Axis Servo: Constrains input array between 30-205, mapping to an inverted sweep of 180° to 0°. • Gripper/Button Servo: Executes a clean step function; holding at 90° when the controller button is pressed and returning to a relaxed resting state at 35°.
🚀 Getting Started
Prerequisites
Ensure you have the following external libraries installed in your Arduino IDE:
• Wire.h (Built-in I2C protocol library)
• Servo.h (Built-in PWM Generation library)
• I2Cdev.h & MPU6050.h (Electronic Row/Jeff Rowberg IMU driver collection)
Deployment Steps
1. Flash the Master Code onto your transmitter controller framework.
2. Flash the Slave Code onto the controller physically tied to the robotic arm assembly.
3. Open your Serial Monitors at 9600 Baud to verify runtime telemetry pipeline diagnostics.
Developed with focus on real-time control, precise system integration, and electromechanical systems design. ⚡🦾
