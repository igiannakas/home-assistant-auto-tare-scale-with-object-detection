# Smart Scale with Object Detection and Auto-Tare
This is my over engineered Homeassitant integrated scale using an ESP32, esphome, proximity detection and weight (load) detection.

It implements a smart scale using an ESP32 microcontroller, designed to measure weight, detect when an object is placed on or removed from the scale and implement an auto-tare functionality to combat sensor drift over time. 

It features weight measurement, object detection using both weight change and a proximity sensor, auto-tare functionality, and visual feedback through an LED. The scale integrates with Home Assistant via ESPHome.

## Table of Contents
- [Features](#features)
- [Use Cases](#use-cases)
- [Required Components](#required-components)
- [Sourcing Components](#sourcing-components)
- [Hardware Setup](#hardware-setup)
- [Software Installation](#software-installation)
- [Calibration and Configuration](#calibration-and-configuration)
- [Additional Notes](#additional-notes)
- [License](#license)

## Features
- **Weight Measurement**: Measures the weight of objects using a load cell and HX711 amplifier.
- **Object Detection**: Detects when an object is added or removed based on weight change and confirms presence using a proximity sensor.
- **Auto-Tare Functionality**: Automatically tares the scale when no object is detected for a specified interval, ensuring consistent zero-point calibration.
- **Visual Feedback**: 
  - Green Light: Object is present on the scale.
  - Blinking Red Light: No object detected on the scale.
  - LED is exposed to Homeassistant for further control using automations
- **Configurable Thresholds**: Adjustable thresholds for object detection using weight changes and object proximity via Home Assistant.

## Use Cases
- Medication Adherence Monitoring
- Smart Kitchen Scale
- Pet Feeding Management

## Required Components
- ESP32 Development Board (Wemos D1 Mini32)
- Load Cell (eg. a 5kg load cell)
- HX711 Load Cell Amplifier Module
- APDS9960 Proximity, Light, RGB, and Gesture Sensor
- Neopixel (WS2811) LED (1 LED)
- Jumper Wires
- Power Supply (5V USB)

Optional: Enclosure or housing for the scale. An enclosure STL, STEP and F3D file is included in the project files.

## Sourcing Components
- ESP32 Development Board
- Load Cell and HX711 Amplifier
- APDS9960 Proximity Sensor
- Neopixel (WS2811) LED

## Hardware Setup
### Wiring Diagram
- **HX711 Load Cell Amplifier**: 
  - DOUT to ESP32 GPIO 27
  - CLK to ESP32 GPIO 25
  - Connect the load cell as per module instructions.
  
- **APDS9960 Proximity Sensor (I2C Connection)**:
  - SDA to ESP32 GPIO 18
  - SCL to ESP32 GPIO 19
  - VCC to 3.3V
  - GND to GND
  
- **Neopixel LED**:
  - Data pin to ESP32 GPIO 16
  - VCC to 5V
  - GND to GND

- **ESP32 Power Supply**: Use a 5V USB power source

## Software Installation
### Prerequisites
- Install [ESPHome](https://esphome.io) on your computer or Home Assistant.

### Steps
1. Download the ESPHome configuration file.
2. Modify the Wi-Fi credentials and GPIO pin assignments if needed.
3. Compile and upload the firmware to your ESP32 board.
4. Add the device to Home Assistant if using it.

## Calibration and Configuration

### Exposed entities in Home Assistant
![image](https://github.com/user-attachments/assets/5e84ce88-1a3e-4f8b-b7a9-ea0d51a7e9aa)

### Load Cell Calibration
- Place known weights on the scale and record raw values.
- Adjust calibration data points in the `hx711` sensor configuration.
- Re-upload the updated configuration.

### Threshold Configuration
- Adjust object detection thresholds via Home Assistant.
- Set switches for auto-tare and proximity confirmation as needed.

## Additional Notes
### Understanding the Logic
- Object detection is based on weight change and proximity confirmation. The initial object detection is done based on rapid weight change, when an object is lifted or placed on the scale. The rate of weight change is controlled by the Add and Remove object threshold values.
- If enabled, the proximity sensor is used to confirm whether an object is trully removed from the scale. After the configured delay, it checks the proximity sensor reading against the proximity threshold and if below, it assumes there is no object present on the scale
- If enabled, it can auto-tare the device when the proximity sensor reads below the proximity threshold after the configured time has passed. This is usefull for automatic setting of the zero-point calibration when the scale is empty, as seen by the proximity sensor. 

### Visual Feedback
- Green Light: Object detected on the scale
- Blinking Red Light: No object detected on the scale
- LED is exposed to Homeassistant for control via automations

### Troubleshooting Tips
- Inconsistent readings: Ensure stable mounting of the load cell.
- Proximity sensor not detecting: Verify I2C connections.
- LED issues: Check Neopixel wiring and power supply.

