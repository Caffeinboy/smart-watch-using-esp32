# Smart Watch Project

## Overview

This project is an IoT-based Smart Watch system developed using an ESP32 microcontroller. The smart watch is designed to monitor important real-time health and environmental parameters while also providing wireless connectivity for remote monitoring and smart notifications.

The system integrates multiple sensors and modules to create a compact wearable prototype capable of displaying live data such as:

* Heart Rate (BPM)
* Body Temperature
* Blood Oxygen Level (SpO2)
* Motion Detection
* Step Counting
* Fall Detection
* Time Display
* Wireless Notification Support

The project is suitable for healthcare monitoring, fitness tracking, elderly safety systems, and wearable IoT applications.

---

## Features

* Real-time heart rate monitoring
* SpO2 (Blood Oxygen) measurement
* Body temperature monitoring
* Motion sensing using accelerometer/gyroscope
* Step counting functionality
* Fall detection system
* OLED/LCD display output
* Wi-Fi enabled IoT monitoring using ESP32
* Blynk app integration for remote access
* Alert system for abnormal readings
* Rechargeable portable wearable design

---

## Components Required

## Hardware

* ESP32 Development Board
* MAX30102 / MAX30105 Pulse Oximeter Sensor
* MPU6050 / MPU6500 Accelerometer + Gyroscope
* MLX90614 / Temperature Sensor
* OLED Display (0.96 inch I2C) or LCD
* Push Buttons
* Buzzer (optional)
* Li-ion Battery
* Charging Module
* Breadboard / PCB Prototype
* Jumper Wires

## Software

* Arduino IDE
* Blynk IoT Platform
* ESP32 Board Package
* Required Arduino Libraries:

  * WiFi.h
  * BlynkSimpleEsp32.h
  * Wire.h
  * MAX30105.h
  * Adafruit_MLX90614.h
  * Adafruit_GFX.h
  * Adafruit_SSD1306.h
  * MPU6050.h

---

## Working Principle

1. The MAX30102/MAX30105 sensor measures heart rate and SpO2.
2. The temperature sensor reads body temperature.
3. MPU6050/MPU6500 detects movement, orientation, and fall events.
4. ESP32 processes all sensor data in real time.
5. Live readings are shown on the OLED/LCD display.
6. Data is sent to Blynk Cloud via Wi-Fi.
7. Alerts can be triggered during abnormal conditions such as high temperature, low oxygen level, or fall detection.

---

## Pin Configuration

| Module       | ESP32 Pin |
| ------------ | --------- |
| MAX30102 SDA | GPIO 21   |
| MAX30102 SCL | GPIO 22   |
| MPU6050 SDA  | GPIO 21   |
| MPU6050 SCL  | GPIO 22   |
| OLED SDA     | GPIO 21   |
| OLED SCL     | GPIO 22   |
| Buzzer       | GPIO 25   |
| Push Button  | GPIO 18   |

> I2C devices share SDA and SCL lines.

---

## Blynk Configuration

Create a new template in Blynk IoT and configure the following virtual pins:

| Parameter      | Virtual Pin |
| -------------- | ----------- |
| Heart Rate     | V0          |
| SpO2           | V1          |
| Temperature    | V2          |
| Fall Detection | V3          |
| Step Count     | V4          |

Update the following values in your code:

```cpp
#define BLYNK_TEMPLATE_ID "YOUR_TEMPLATE_ID"
#define BLYNK_TEMPLATE_NAME "YOUR_TEMPLATE_NAME"
#define BLYNK_AUTH_TOKEN "YOUR_AUTH_TOKEN"
```

Also update Wi-Fi credentials:

```cpp
char ssid[] = "YOUR_WIFI_NAME";
char pass[] = "YOUR_WIFI_PASSWORD";
```

---

## Installation Steps

### Step 1: Install Arduino IDE

Download and install Arduino IDE.

### Step 2: Install ESP32 Board Package

Add ESP32 Board Manager URL and install ESP32 boards.

### Step 3: Install Required Libraries

Install all required sensor libraries using Library Manager.

### Step 4: Upload the Code

* Select correct ESP32 board
* Select correct COM port
* Upload the smart watch code

### Step 5: Setup Blynk Dashboard

Create widgets for each parameter and assign the correct virtual pins.

### Step 6: Monitor Live Data

Use Serial Monitor, OLED display, and Blynk dashboard for testing.

---

## Example Applications

* Personal Health Monitoring
* Fitness Tracking System
* Elderly Fall Detection Device
* Remote Patient Monitoring
* Wearable Biomedical Device
* Smart Sports Watch Prototype
* IoT Healthcare Research Projects

---

## Future Improvements

* GPS live location tracking
* GSM emergency alert system
* Bluetooth smartphone notifications
* Sleep monitoring
* ECG sensor integration
* AI-based health prediction
* Voice assistant support
* Full smartwatch casing design

---

## Folder Structure

```text
Smart-Watch-Project/
│
├── code/
│   └── smart_watch.ino
│
├── circuit_diagram/
│   └── wiring.png
│
├── images/
│   └── prototype_setup.png
│
└── README.md
```

---

## Important Note

Do not upload your real Wi-Fi password, Blynk Auth Token, or private credentials to public GitHub repositories.
Always replace them with placeholders before pushing code.

Example:

```cpp
#define BLYNK_AUTH_TOKEN "YOUR_AUTH_TOKEN"
```

---

## Author

Project developed for wearable IoT healthcare monitoring and smart watch prototype applications using ESP32.

---

## License

This project is open for educational and learning purposes.
You may modify, improve, and extend it as needed.
