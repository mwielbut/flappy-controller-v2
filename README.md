# Flappy Controller v2

Controller for the split-flap display system. Receives MQTT messages from AWS IoT and distributes display data to worker boards via SerialTransfer.

**This controller is actively used. See `arduino_secrets.h`.**

## Hardware

- **Board:** Arduino Nano 33 IoT
- **Platform:** atmelsam
- **Additional:**
  - ECCX08 crypto chip (built into Nano 33 IoT)
  - Serial1 connection to worker boards

## Features

- WiFi connectivity with automatic reconnection
- Secure MQTT communication with AWS IoT using certificate-based authentication
- SerialTransfer protocol for communicating with worker boards
- Supports up to 64 characters (screen size)
- Per-character correction values for fine-tuning position
- LED status indicator (ON = not connected, OFF = connected)

## Supported Characters

40-character alphabet: ` ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789.,'-`

(space, letters A-Z, numbers 0-9, period, comma, apostrophe)

## MQTT Message Format

```json
{
  "state": {
    "desired": {
      "message": "HELLO WORLD",
      "corrections": "0000000000000000000000000000000000000000000000000000000000000000"
    }
  }
}
```

- `message`: Up to 64 characters to display
- `corrections`: Per-character position correction (0-5 steps), one digit per character

## Configuration

Create `include/arduino_secrets.h`:

```cpp
#define WIFI_SECRET_SSID "your-wifi-ssid"
#define WIFI_SECRET_PASS "your-wifi-password"
#define MQTT_AWS_NAME "your-thing-name"
#define MQTT_AWS_BROKER "your-iot-endpoint.iot.region.amazonaws.com"
#define MQTT_AWS_PUBLIC_CERTIFICATE "-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----"
```

## Building and Flashing

```bash
# Build
pio run

# Upload
pio run --target upload

# Monitor serial output (115200 baud)
pio device monitor
```

## Dependencies

| Library | Version |
|---------|---------|
| powerbroker2/SerialTransfer | ^3.1.2 |
| Wire | - |
| arduino-libraries/WiFiNINA | ^1.8.14 |
| arduino-libraries/ArduinoMqttClient | ^0.1.7 |
| arduino/Arduino Cloud Provider Examples | ^1.2.1 |
| arduino-libraries/ArduinoBearSSL | ^1.7.3 |
| thijse/ArduinoLog | ^1.1.1 |
| arduino-libraries/Arduino_JSON | ^0.2.0 |
| bblanchon/ArduinoJson | ^6.21.4 |

## Changes from v1

- Removed direct Tic I2C motor control
- Added SerialTransfer for worker communication
- Changed message format from single letter to full screen message
- Added per-character corrections support
- Increased screen size to 64 characters
