# HomeAssistant-MQTT PlatformIO Project (ESP8266)

This is a PlatformIO version of the HomeAssistant-MQTT example for ESP8266.

## Requirements

- PlatformIO CLI or PlatformIO IDE
- ESP8266 development board (NodeMCU, Wemos D1 Mini, etc.)
- dscKeybusInterface library

## Setup

### 1. Install PlatformIO

If you haven't already, install PlatformIO:
```bash
pip install platformio
```

Or use VS Code with the PlatformIO extension.

### 2. Configure Your Settings

Edit `src/main.cpp` and update the following constants with your configuration:

```cpp
const char* wifiSSID = "your-wifi-ssid";
const char* wifiPassword = "your-wifi-password";
const char* accessCode = "your-access-code";
const char* mqttServer = "your-mqtt-server";
const char* mqttUsername = "mqtt-username";  // Optional
const char* mqttPassword = "mqtt-password";  // Optional
```

### 3. Wire Your ESP8266

Follow the wiring diagram in the code comments:

- **DSC Yellow** (Clock) → 33k resistor → **D1 (GPIO 5)** + 10k resistor to Ground
- **DSC Green** (Data) → 33k resistor → **D2 (GPIO 4)** + 10k resistor to Ground
- **DSC Aux(+)** → 5V voltage regulator → **5V**
- **DSC Aux(-)** → **GND**
- **Virtual Keypad** (optional):
  - DSC Green → NPN collector
  - NPN emitter → Ground
  - NPN base → 1k resistor → **D8 (GPIO 15)**

For Classic series support, uncomment `#define dscClassicSeries` and connect:
- **DSC PGM** → 1k resistor → DSC Aux(+) and 33k resistor → **D7 (GPIO 13)** + 10k resistor to Ground

### 4. Build the Project

```bash
platformio run
```

Or if using PlatformIO IDE, click the Build button.

### 5. Upload to ESP8266

```bash
platformio run --target upload
```

### 6. Monitor Serial Output

```bash
platformio device monitor
```

Or use the Monitor button in PlatformIO IDE.

## MQTT Topics

The sketch publishes to and subscribes from various MQTT topics. Configure your Home Assistant `configuration.yaml` with the appropriate MQTT settings and entity definitions. See the code comments for example configurations.

### Published Topics
- `dsc/Get/Partition1` - Partition 1 status
- `dsc/Get/Zone1` - Zone 1 status
- `dsc/Get/Fire1` - Partition 1 fire alarm
- `dsc/Get/PGM1` - PGM output 1 status
- `dsc/Get/Trouble` - System trouble status
- `dsc/Status` - MQTT availability status

### Subscribed Topic
- `dsc/Set` - Commands to control the panel

## Classic Series Support

To use with DSC Classic series (PC1500/PC1550):

1. Uncomment `#define dscClassicSeries` in `src/main.cpp`
2. Ensure your panel is configured with PC-16 output for PGM
3. Wire the PGM pin to D7 (GPIO 13) as described above

## Troubleshooting

- **WiFi connection issues**: Check your SSID and password
- **MQTT connection issues**: Verify server address, port, and credentials
- **No Keybus data**: Check your wiring, especially the voltage dividers
- **Buffer overflow warnings**: Increase `dscBufferSize` in the library header files

## License

This code is in the public domain. See the original repository for more information.

## Repository

https://github.com/taligentx/dscKeybusInterface

## OTA update
How to Use OTA Updates:
Once the device is running and connected to WiFi, you can update it wirelessly:

From PlatformIO IDE:
```
platformio run --target upload --upload-port dsc-keybus-esp8266.local
```

Or from the CLI:
```
pio run -t upload --upload-port 192.168.x.x
```
