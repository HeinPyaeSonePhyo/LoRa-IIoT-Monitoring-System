# LoRa IIoT Monitoring System


A point-to-point LoRa monitoring system built using two Arduino MKR WAN 1310 boards. The Trigger Node sends a request when a button is pressed, and the Sensor Node reads temperature and humidity from a DHT22 sensor and returns the readings with a timestamp from an RTC.

The system uses LoRa communication at 868 MHz to exchange messages between the two nodes.

## How It Works

The system consists of two nodes:

### Trigger Node

- Waits for a button press on pin 2.
- Sends a `READ` request to the Sensor Node.
- Waits for a response and displays the received data through the Serial Monitor.
- Uses a debounce delay to reduce repeated button triggers.

### Sensor Node

- Listens for incoming LoRa requests.
- Processes a valid reading request.
- Reads temperature and humidity from the DHT22 sensor connected to pin 6.
- Obtains a timestamp from the RTC.
- Sends the readings back to the Trigger Node.

### Communication Flow

1. The user presses the Trigger Node button.
2. The Trigger Node sends a request over LoRa.
3. The Sensor Node receives the request and reads the sensors.
4. The Sensor Node sends a response containing the timestamp, temperature and humidity.
5. The Trigger Node receives and displays the response.

## Hardware Requirements

- 2 × Arduino MKR WAN 1310 boards
- 1 × DHT22 temperature and humidity sensor
- 1 × Push button
- RTC functionality provided by the RTCZero library
- Breadboard and jumper wires
- USB cables for programming and serial monitoring

Both boards must use compatible LoRa radio settings and be configured for the same communication frequency and sync word.

## Wiring

| Component | Node | Pin |
|---|---|---|
| Push button | Trigger Node | Digital pin 2 |
| DHT22 data | Sensor Node | Digital pin 6 |

Connect the DHT22 power and ground according to the sensor and board requirements. Check your actual hardware wiring and sketch before powering the circuit.

## Project Structure

```text
LoRa-IIoT-Monitoring-System/
├── README.md
└── Arduino/
    ├── trigger/
    │   └── trigger.ino
    ├── sensor/
    │   └── sensor.ino
    └── libraries/
```

The `libraries/` directory contains bundled Arduino libraries. Only the libraries required by the sketches need to be installed or included for compilation.

## Software Requirements

- Arduino IDE
- Board support for Arduino MKR WAN 1310
- LoRa library — 0.8.0
- DHT sensor library — 1.4.6
- Adafruit Unified Sensor — 1.1.15
- RTCZero — 1.6.0

Install the required libraries through the Arduino IDE Library Manager or use the compatible bundled versions supplied with the project.

## Setup and Run

### 1. Prepare the boards

Connect each Arduino MKR WAN 1310 to your computer using USB.

### 2. Open the sketches

Open the Trigger Node sketch:

```text
Arduino/trigger/trigger.ino
```

Open the Sensor Node sketch:

```text
Arduino/sensor/sensor.ino
```

### 3. Check the configuration

Ensure both sketches use compatible LoRa settings, including the same frequency and sync word.

### 4. Compile and upload

Select the appropriate Arduino MKR WAN 1310 board and its corresponding port in the Arduino IDE. Compile and upload each sketch to its respective board.

### 5. Start the system

1. Open the Serial Monitor for each board at **9600 baud**.
2. Allow the sketches to initialise.
3. Press the button connected to the Trigger Node.
4. Check the serial output for the request and returned sensor readings.

Both sketches wait for the Serial Monitor to open before continuing, so open it after uploading to allow the program to proceed.

## Configuration

| Setting | Value | Purpose |
|---|---|---|
| LoRa frequency | `868E6` | Radio communication frequency |
| Sync word | `0xF3` | Identifies the intended LoRa network |
| XOR key | `D51` | Shared key used for basic message obfuscation |
| Serial baud rate | `9600` | Serial Monitor communication speed |
| Button pin | `2` | Trigger Node button input |
| DHT22 data pin | `6` | Sensor Node temperature and humidity input |

The two nodes must use compatible radio settings and the same XOR key to interpret the messages correctly.

## Message Format

The Sensor Node returns data in the following format:

```text
Time:DD-MM-YYYY HH:MM:SS,Temp:23.50,Hum:45.20
```

The example values are illustrative.

- **Time** — timestamp provided by the RTC
- **Temp** — temperature reading
- **Hum** — relative humidity reading

## Limitations

- **Point-to-point communication:** the system uses direct LoRa communication, not LoRaWAN.
- **Basic obfuscation:** repeating-key XOR is not secure encryption and should not be used to protect sensitive data.
- **RTC initialisation:** if the RTC is not set to the current time, timestamps may start from a default date rather than represent the actual date and time.
- **No replay protection:** the protocol does not include a mechanism to prevent old messages from being replayed.
- **Blocking response wait:** the Trigger Node can wait up to three seconds for a response, which may temporarily block other processing.
- **Button debounce:** a 50 ms debounce interval is used to reduce accidental repeated triggers.
- **Radio range:** communication reliability depends on the environment, antenna placement and interference.

## Author

Developed by Hein Pyae Sone Phyo.

[GitHub Profile](https://github.com/HeinPyaeSonePhyo)
