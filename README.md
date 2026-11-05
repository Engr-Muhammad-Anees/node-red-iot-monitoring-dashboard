
# Sensor Monitoring and Control Dashboard Using Node-RED and MQTT

An IoT-based environmental monitoring and control system developed using **Node-RED, MQTT, and Mosquitto**. The project simulates temperature and humidity sensors, displays real-time readings on a dashboard, automatically controls devices based on temperature conditions, supports manual operation, generates critical temperature alerts, and stores sensor data in a CSV file.

## Project Overview

This project demonstrates how an IoT monitoring and control system can be developed using Node-RED and MQTT without requiring physical sensors or hardware.

A sensor simulator generates temperature and humidity readings every five seconds. The readings are published through an MQTT broker and received by Node-RED for processing, visualization, device control, and data logging.

## Features

- **Sensor Simulation:** Generates random temperature readings between 20°C and 45°C and humidity readings between 30% and 90%.
- **MQTT Communication:** Uses the Mosquitto MQTT broker to publish and receive sensor data.
- **Real-Time Dashboard:** Displays temperature and humidity through gauges and historical charts.
- **Environmental Status Monitoring:** Classifies temperature and humidity as LOW, NORMAL, or HIGH.
- **Automatic Device Control:** Controls the heater and fan according to temperature conditions.
- **Manual Control:** Allows users to operate the heater and fan independently through dashboard switches.
- **Operating Modes:** Supports AUTO and MANUAL modes.
- **Critical Temperature Alert:** Generates an alert when temperature exceeds 40°C.
- **CSV Data Logging:** Saves timestamp, temperature, humidity, heater status, fan status, and operating mode.
- **Live Monitoring:** Updates dashboard readings and device statuses automatically.

## System Architecture

The system follows an MQTT-based communication architecture.

```text
       Sensor Simulator
              |
              v
       MQTT Publisher
              |
              v
       Mosquitto Broker
              |
              v
       MQTT Subscriber
              |
              v
       Device State Manager
              |
       +------+-------+----------------+
       |              |                |
       v              v                v
   Dashboard      Device Control    CSV Logging
   Monitoring     AUTO / MANUAL     sensor_log.csv
       |
       v
  Critical Temperature Alert
```

## Technologies Used

| Technology | Purpose |
|---|---|
| Node.js | Runtime environment |
| Node-RED | Flow-based programming and system integration |
| MQTT | Lightweight messaging protocol |
| Mosquitto | MQTT message broker |
| FlowFuse Dashboard 2 | Real-time dashboard visualization |
| JavaScript | Sensor simulation and control logic |
| CSV | Sensor data storage |

## Hardware and Software Requirements

### Hardware
- Computer or laptop
- Minimum 4 GB RAM recommended
- Internet connection for initial software installation

### Software
- Windows, Linux, or macOS
- Node.js and npm
- Node-RED
- Eclipse Mosquitto MQTT Broker
- FlowFuse Dashboard 2 Node-RED package

## Installation and Setup

### 1. Install Node.js

Download and install Node.js:

https://nodejs.org/

Verify installation:

```bash
node --version
npm --version
```

### 2. Install Node-RED

Install Node-RED globally:

```bash
npm install -g node-red
```

Start Node-RED:

```bash
node-red
```

On Windows PowerShell, if the execution policy blocks the PowerShell script, use:

```powershell
node-red.cmd
```

Open the Node-RED editor:

http://localhost:1880

### 3. Install Mosquitto

Download and install Eclipse Mosquitto:

https://mosquitto.org/download/

Start the Mosquitto service if it is not already running.

The default MQTT broker address used in this project is:

```text
Host: localhost
Port: 1883
```

### 4. Install FlowFuse Dashboard 2

In the Node-RED editor:

1. Open the menu.
2. Select Manage Palette.
3. Open the Install tab.
4. Search for `@flowfuse/node-red-dashboard`.
5. Install the package.

### 5. Import the Node-RED Flow

1. Open the Node-RED editor.
2. Select Menu → Import.
3. Choose the project flow JSON file.
4. Import the flow.
5. Click Deploy.

The exported flow is available at:

```text
flows/node_red_intern_task.json
```

### 6. Open the Dashboard

After deploying the flow, open:

http://localhost:1880/dashboard

## MQTT Configuration

The project uses the following MQTT configuration:

| Parameter | Value |
|---|---|
| Broker | Mosquitto |
| Host | localhost |
| Port | 1883 |
| MQTT Topic | intern/environment/sensors |
| Publishing Interval | 5 seconds |
| Payload Format | JSON |

Example MQTT message:

```json
{
  "temperature": 32.54,
  "humidity": 60.81,
  "timestamp": "2026-11-05T06:36:26.250Z"
}
```

## Automatic Device Control Logic

The system automatically controls the heater and fan based on temperature readings.

| Temperature | Heater | Fan |
|---|---|---|
| Below 35°C | ON | OFF |
| 35°C – 38°C | OFF | OFF |
| Above 38°C | OFF | ON |

Automatic control is active only when the system is operating in AUTO mode.

## Manual Control

The dashboard provides independent switches for the heater and fan.

When MANUAL mode is selected:

- Users can turn the heater ON or OFF.
- Users can turn the fan ON or OFF.
- Automatic control does not override manual settings.
- Device states remain unchanged when new sensor readings arrive.

When the system switches back to AUTO mode, device states are recalculated using the latest temperature reading.

## Environmental Status Conditions

### Temperature Status

| Condition | Status |
|---|---|
| Below 35°C | LOW |
| 35°C – 38°C | NORMAL |
| Above 38°C | HIGH |

### Humidity Status

| Condition | Status |
|---|---|
| Below 40% | LOW |
| 40% – 75% | NORMAL |
| Above 75% | HIGH |

## Critical Temperature Alert

A safety alert is generated when the temperature exceeds 40°C.

- The dashboard displays a critical temperature warning.
- A warning message is generated in the Node-RED Debug sidebar.
- The alert clears when the temperature returns to 40°C or below.

## CSV Data Logging

The system automatically stores each processed sensor reading in a CSV file.

File location:

```text
data/sensor_log.csv
```

CSV columns:

| Column | Description |
|---|---|
| Timestamp | Date and time of the reading |
| Temperature | Measured temperature in °C |
| Humidity | Measured humidity in % |
| Heater | Current heater status |
| Fan | Current fan status |
| Mode | Current operating mode |

Example:

```csv
Timestamp,Temperature,Humidity,Heater,Fan,Mode
2026-11-05T06:36:21.239Z,39.29,34.86,OFF,ON,MANUAL
2026-11-05T06:36:51.317Z,24.05,32.05,ON,OFF,AUTO
```

## Testing and Validation

The project was tested using simulated sensor readings and dashboard controls.

| Test Scenario | Expected Result |
|---|---|
| Temperature below 35°C in AUTO | Heater ON, Fan OFF |
| Temperature between 35°C and 38°C | Both OFF |
| Temperature above 38°C in AUTO | Heater OFF, Fan ON |
| Temperature above 40°C | Critical alert displayed |
| Humidity below 40% | LOW status |
| Humidity between 40% and 75% | NORMAL status |
| Humidity above 75% | HIGH status |
| MANUAL mode | Independent device control |
| Switch back to AUTO | Automatic control resumes |
| New sensor reading | CSV record appended |

## Project Structure

```text
NodeRED_IoT_Project/
│
├── flows/
│   └── node_red_intern_task.json
│
├── data/
│   └── sensor_log.csv
│
├── screenshots/
│   ├── 01-dashboard-normal.png
│   ├── 02-low-temperature.png
│   ├── 03-high-temperature.png
│   ├── 04-manual-mode.png
│   ├── 05-critical-alarm.png
│   ├── 06-node-red-flow-1.png
│   ├── 07-node-red-flow-2.png
│   └── 08-dashboard-full.png
│
└── README.md
```

## Future Improvements

- Integration with physical temperature and humidity sensors.
- Integration with ESP32 or Arduino devices.
- Remote monitoring over a network.
- Database integration for long-term data storage.
- User authentication and dashboard security.
- Advanced analytics and sensor data visualization.

## Learning Outcomes

Through this project, the following concepts were implemented and practiced:

- Node-RED flow development.
- MQTT communication and Mosquitto broker configuration.
- JavaScript-based data processing.
- Real-time dashboard development.
- Conditional logic and device control.
- AUTO and MANUAL operating modes.
- Event-based alerts.
- CSV file handling and data logging.
- Testing and debugging IoT applications.

## Author

**Muhammad Anees**

AI/ML Engineer | IoT and Automation Enthusiast

- GitHub: https://github.com/Engr-Muhammad-Anees
- LinkedIn: https://www.linkedin.com/in/muhammad-anees-a78417225

---

**Note:** This project uses simulated environmental sensor data. The heater and fan are represented as software-controlled states and are not connected to physical devices.
