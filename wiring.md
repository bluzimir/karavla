# HVAC Controller Wiring Schema

This document covers wiring for all supported boards:
- **YD-RP2040** - No WiFi, uses Meshtastic/Serial
- **ESP32-WROOM-32** - WiFi + Bluetooth + Meshtastic
- **Xiao ESP32S3** - WiFi + Bluetooth 5.0 + Compact form factor
- **Xiao ESP32S3 + MCP23017** - Xiao with I2C GPIO expander for 16 floor valves

---

# ESP32-WROOM-32 Wiring Schema

## Features

- WiFi 802.11 b/g/n
- Bluetooth 4.2 BLE
- 34 GPIO pins (some restricted)
- Dual-core 240MHz

## Pin Assignment Table (ESP32)

| GPIO | Function | Interface | Config File |
|------|----------|-----------|-------------|
| GPIO4 | DS18B20 Temperature Sensors | OneWire | config_core.json, config_mixing.json |
| GPIO16 | Meshtastic RX | UART2 | config_meshtastic.json |
| GPIO17 | Meshtastic TX | UART2 | config_meshtastic.json |
| GPIO21 | SSD1306 Display SDA | I2C | config_core.json |
| GPIO22 | SSD1306 Display SCL | I2C | config_core.json |
| GPIO25 | mix_radiator Motor Forward | Digital Out | config_mixing.json |
| GPIO26 | mix_radiator Motor Backward | Digital Out | config_mixing.json |
| GPIO27 | mix_floor Motor Forward | Digital Out | config_mixing.json |
| GPIO14 | mix_floor Motor Backward | Digital Out | config_mixing.json |
| GPIO32 | Floor valve: dnevna | Digital Out | config_floor.json |
| GPIO33 | Floor valve: predsoba | Digital Out | config_floor.json |
| GPIO18 | Floor valve: kuhinja | Digital Out | config_floor.json |
| GPIO19 | Floor valve: hodnik | Digital Out | config_floor.json |
| GPIO13 | Button: Next Page | Digital In | config_core.json |
| GPIO15 | Button: Prev Page | Digital In | config_core.json |

## ESP32 Wiring Diagram

```text
                    ESP32-WROOM-32
                 ┌──────────────────┐
                 │    [Antenna]     │
                 │                  │
    DS18B20 ─────┤ GPIO4            │
                 │                  │
                 │ GPIO16  GPIO17   ├───── Meshtastic UART
                 │  RX      TX      │      (RX↔TX crossed)
                 │                  │
    SSD1306 ─────┤ GPIO21  GPIO22   │
     I2C         │  SDA     SCL     │
                 │                  │
                 │ GPIO25  GPIO26   ├───── mix_radiator Motor
                 │  FWD     BWD     │
                 │                  │
                 │ GPIO27  GPIO14   ├───── mix_floor Motor
                 │  FWD     BWD     │
                 │                  │
                 │ GPIO32  GPIO33   ├───── Floor valves (dnevna, predsoba)
                 │ GPIO18  GPIO19   ├───── Floor valves (kuhinja, hodnik)
                 │                  │
    Buttons ─────┤ GPIO13  GPIO15   │
                 │  NEXT    PREV    │
                 │                  │
                 │  3V3     GND     ├───── Power
                 │   5V     GND     │
                 │                  │
                 │    [WiFi Built-in]│
                 └──────────────────┘
```

## ESP32 GPIO Notes

⚠️ **Avoid these GPIOs:**

| GPIO | Reason |
|------|--------|
| GPIO0 | Boot mode (strapping pin) |
| GPIO2 | Boot mode, onboard LED |
| GPIO6-11 | Internal flash SPI |
| GPIO12 | Boot mode (strapping pin) |
| GPIO34-39 | Input only, no pull-up |

✅ **Safe GPIOs for outputs:** 4, 13, 14, 16, 17, 18, 19, 21, 22, 23, 25, 26, 27, 32, 33

## ESP32 Config Files

Update `config_network.json`:
```json
{
  "NETWORK_ENABLED": true,
  "WIFI_SSID": "your_network",
  "WIFI_PASSWORD": "your_password",
  "MQTT_BROKER": "broker.hivemq.com",
  "MQTT_PORT": 1883
}
```

Update `config_core.json` for ESP32 button pins:
```json
{
  "BTN_PAGE_NEXT_PIN": 13,
  "BTN_PAGE_PREV_PIN": 15
}
```

Update `config_mixing.json` for ESP32:
```json
{
  "MIXING_VALVES": [
    {
      "name": "mix_radiator",
      "forward_pin": 25,
      "backward_pin": 26
    },
    {
      "name": "mix_floor",
      "forward_pin": 27,
      "backward_pin": 14
    }
  ]
}
```

Update `config_floor.json` for ESP32:
```json
{
  "FLOOR_ROOMS": [
    { "name": "dnevna", "pin": 32 },
    { "name": "predsoba", "pin": 33 },
    { "name": "kuhinja", "pin": 18 },
    { "name": "hodnik", "pin": 19 }
  ]
}
```

---

# Xiao ESP32S3 Wiring Schema

## Features

- WiFi 802.11 b/g/n
- Bluetooth 5.0 BLE
- 11 GPIO pins (compact board)
- Single-core 240MHz
- USB-C connector
- Ultra-compact 21x17.5mm

## Pin Assignment Table (Xiao ESP32S3)

| Pin | GPIO | Function | Interface | Config File |
|-----|------|----------|-----------|-------------|
| D0 | GPIO1 | DS18B20 Temperature Sensors | OneWire | config_core.json, config_mixing.json |
| D1 | GPIO2 | mix_radiator Motor Forward | Digital Out | config_mixing.json |
| D2 | GPIO3 | mix_radiator Motor Backward | Digital Out | config_mixing.json |
| D3 | GPIO4 | mix_floor Motor Forward | Digital Out | config_mixing.json |
| D4 | GPIO5 | mix_floor Motor Backward | Digital Out | config_mixing.json |
| D5 | GPIO6 | Floor valve: dnevna | Digital Out | config_floor.json |
| D6 | GPIO43 | Meshtastic TX | UART | config_meshtastic.json |
| D7 | GPIO44 | Meshtastic RX | UART | config_meshtastic.json |
| D8 | GPIO7 | SSD1306 Display SCL | I2C | config_core.json |
| D9 | GPIO8 | SSD1306 Display SDA | I2C | config_core.json |
| D10 | GPIO9 | Button: Next Page | Digital In | config_core.json |

## Xiao ESP32S3 Wiring Diagram

```text
                    Xiao ESP32S3
                 ┌──────────────────┐
                 │   [USB-C]        │
                 │                  │
    DS18B20 ─────┤ D0 (GPIO1)       │
                 │                  │
                 │ D1 (GPIO2)       ├───── mix_radiator Motor FWD
                 │ D2 (GPIO3)       ├───── mix_radiator Motor BWD
                 │                  │
                 │ D3 (GPIO4)       ├───── mix_floor Motor FWD
                 │ D4 (GPIO5)       ├───── mix_floor Motor BWD
                 │                  │
                 │ D5 (GPIO6)       ├───── Floor valve (dnevna)
                 │                  │
                 │ D6 (GPIO43)      ├───── Meshtastic TX
                 │ D7 (GPIO44)      ├───── Meshtastic RX
                 │                  │      (RX↔TX crossed)
                 │                  │
    SSD1306 ─────┤ D8 (GPIO7)  SCL  │
     I2C    ─────┤ D9 (GPIO8)  SDA  │
                 │                  │
    Button  ─────┤ D10 (GPIO9)      │
                 │                  │
                 │  3V3        GND  ├───── Power
                 │  5V         GND  │
                 │                  │
                 │   [WiFi Built-in]│
                 └──────────────────┘
```

## Xiao ESP32S3 GPIO Notes

⚠️ **Limited GPIO Warning:**
The Xiao ESP32S3 has only 11 usable GPIOs. This limits the number of floor valves to 1 without expansion (I2C GPIO expander recommended for more valves).

⚠️ **Avoid these pins:**

| Pin | Reason |
|-----|--------|
| GPIO0 | Strapping pin (boot mode) |
| GPIO45 | Strapping pin |
| GPIO46 | Strapping pin |

✅ **All D0-D10 pins are safe** for general use.

## Xiao ESP32S3 Config Files

Update `config_network.json`:
```json
{
  "NETWORK_ENABLED": true,
  "WIFI_SSID": "your_network",
  "WIFI_PASSWORD": "your_password",
  "MQTT_BROKER": "broker.hivemq.com",
  "MQTT_PORT": 1883
}
```

Update `config_core.json` for Xiao ESP32S3:
```json
{
  "TEMP_SENSOR_PIN": 1,
  "I2C_SDA_PIN": 8,
  "I2C_SCL_PIN": 7,
  "BTN_PAGE_NEXT_PIN": 9
}
```

Update `config_mixing.json` for Xiao ESP32S3:
```json
{
  "MIXING_VALVES": [
    {
      "name": "mix_radiator",
      "forward_pin": 2,
      "backward_pin": 3
    },
    {
      "name": "mix_floor",
      "forward_pin": 4,
      "backward_pin": 5
    }
  ]
}
```

Update `config_floor.json` for Xiao ESP32S3:
```json
{
  "FLOOR_ROOMS": [
    { "name": "dnevna", "pin": 6 }
  ]
}
```

**Note:** For additional floor valves, use an I2C GPIO expander (e.g., PCF8574 or MCP23017) connected to D8/D9.

Update `config_meshtastic.json` for Xiao ESP32S3:
```json
{
  "MESHTASTIC_TX_PIN": 43,
  "MESHTASTIC_RX_PIN": 44,
  "MESHTASTIC_BAUD": 115200
}
```

---

# Xiao ESP32S3 + MCP23017 Wiring Schema

This variant uses an MCP23017 I2C GPIO expander to provide 16 additional GPIO pins for floor heating valves.

## Features

- All features of Xiao ESP32S3
- 16 additional GPIOs via MCP23017
- Supports up to 16 floor heating valves
- Single I2C bus shared with OLED display

## Required Modules

- `hvac_modules/mcp23017.py` - MCP23017 driver
- `hvac_modules/floor_heating_mcp.py` - Floor heating controller for MCP23017

## Pin Assignment Table (Xiao ESP32S3 + MCP23017)

### Xiao ESP32S3 Pins

| Pin | GPIO | Function | Interface |
|-----|------|----------|-----------|
| D0 | GPIO1 | DS18B20 Temperature Sensors | OneWire |
| D1 | GPIO2 | mix_radiator Motor Forward | Digital Out |
| D2 | GPIO3 | mix_radiator Motor Backward | Digital Out |
| D3 | GPIO4 | mix_floor Motor Forward | Digital Out |
| D4 | GPIO5 | mix_floor Motor Backward | Digital Out |
| D5 | GPIO6 | (Available) | - |
| D6 | GPIO43 | Meshtastic TX | UART |
| D7 | GPIO44 | Meshtastic RX | UART |
| D8 | GPIO7 | I2C SCL (Display + MCP23017) | I2C |
| D9 | GPIO8 | I2C SDA (Display + MCP23017) | I2C |
| D10 | GPIO9 | Button: Next Page | Digital In |

### MCP23017 Pins (I2C Address 0x20)

| MCP Pin | Function | Config Key |
|---------|----------|------------|
| GPA0 | Floor valve: dnevna | mcp_pin: 0 |
| GPA1 | Floor valve: predsoba | mcp_pin: 1 |
| GPA2 | Floor valve: kuhinja | mcp_pin: 2 |
| GPA3 | Floor valve: hodnik | mcp_pin: 3 |
| GPA4 | Floor valve: kopalnica | mcp_pin: 4 |
| GPA5 | Floor valve: spalnica | mcp_pin: 5 |
| GPA6 | Floor valve: otroska | mcp_pin: 6 |
| GPA7 | Floor valve: garaza | mcp_pin: 7 |
| GPB0-GPB7 | (8 additional valves available) | mcp_pin: 8-15 |

## Wiring Diagram

```text
                    Xiao ESP32S3                          MCP23017
                 ┌──────────────────┐                  ┌─────────────────┐
                 │   [USB-C]        │                  │                 │
                 │                  │                  │ GPA0 ───────────┼── Floor: dnevna
    DS18B20 ─────┤ D0 (GPIO1)       │                  │ GPA1 ───────────┼── Floor: predsoba
                 │                  │                  │ GPA2 ───────────┼── Floor: kuhinja
                 │ D1 (GPIO2)       ├── mix_rad FWD    │ GPA3 ───────────┼── Floor: hodnik
                 │ D2 (GPIO3)       ├── mix_rad BWD    │ GPA4 ───────────┼── Floor: kopalnica
                 │                  │                  │ GPA5 ───────────┼── Floor: spalnica
                 │ D3 (GPIO4)       ├── mix_floor FWD  │ GPA6 ───────────┼── Floor: otroska
                 │ D4 (GPIO5)       ├── mix_floor BWD  │ GPA7 ───────────┼── Floor: garaza
                 │                  │                  │                 │
                 │ D5 (GPIO6)       │  (available)     │ GPB0-7 ─────────┼── (8 more valves)
                 │                  │                  │                 │
                 │ D6 (GPIO43)      ├── Meshtastic TX  │                 │
                 │ D7 (GPIO44)      ├── Meshtastic RX  │                 │
                 │                  │                  │                 │
    I2C Bus ─────┤ D8 (GPIO7)  SCL ─┼──────────────────┼─ SCL            │
            ─────┤ D9 (GPIO8)  SDA ─┼──────────────────┼─ SDA            │
                 │                  │                  │                 │
    Button  ─────┤ D10 (GPIO9)      │       3V3 ───────┼─ VDD            │
                 │                  │       GND ───────┼─ VSS            │
                 │  3V3        GND  │                  │                 │
                 └──────────────────┘       GND ───────┼─ A0, A1, A2     │
                                                       └─────────────────┘
                        │
                 ┌──────┴──────┐
                 │  SSD1306    │ (also on I2C bus)
                 │   OLED      │
                 │  0x3C       │
                 └─────────────┘
```

## MCP23017 Wiring Details

| MCP23017 Pin | Connection |
|--------------|------------|
| VDD | 3.3V |
| VSS | GND |
| SDA | Xiao D9 (GPIO8) |
| SCL | Xiao D8 (GPIO7) |
| A0 | GND (address bit 0) |
| A1 | GND (address bit 1) |
| A2 | GND (address bit 2) |
| RESET | 3.3V (or leave floating with pull-up) |

**I2C Address:** With A0=A1=A2=GND, address is **0x20** (32 decimal).

## Config File: config_floor_xiao_mcp.json

```json
{
  "FLOOR_HEATING_ENABLED": true,
  "FLOOR_USE_MCP23017": true,
  "MCP23017_ADDRESS": 32,
  "MCP23017_SDA_PIN": 8,
  "MCP23017_SCL_PIN": 7,
  "FLOOR_DT": 0.5,
  "FLOOR_ROOMS": [
    { "name": "dnevna", "mcp_pin": 0 },
    { "name": "predsoba", "mcp_pin": 1 },
    { "name": "kuhinja", "mcp_pin": 2 },
    { "name": "hodnik", "mcp_pin": 3 },
    { "name": "kopalnica", "mcp_pin": 4 },
    { "name": "spalnica", "mcp_pin": 5 },
    { "name": "otroska", "mcp_pin": 6 },
    { "name": "garaza", "mcp_pin": 7 }
  ]
}
```

## Usage in Code

```python
from hvac_modules.floor_heating_mcp import create_floor_controller_from_config

# Create controller from config file
floor_ctrl = create_floor_controller_from_config()

# Or create manually
from hvac_modules.floor_heating_mcp import FloorHeatingControllerMCP

floor_ctrl = FloorHeatingControllerMCP(
    floor_rooms=[
        {"name": "dnevna", "mcp_pin": 0},
        {"name": "predsoba", "mcp_pin": 1},
    ],
    dt=0.5,
    mcp_address=0x20,
    sda_pin=8,
    scl_pin=7,
)

# Update temperatures from Meshtastic
floor_ctrl.update_room_set_temp("dnevna", 22.0)
floor_ctrl.update_room_temp("dnevna", 20.5)

# Run control loop
import uasyncio as asyncio
asyncio.run(floor_ctrl.run())
```

---

# YD-RP2040 Wiring Schema

## Features

- No WiFi/Bluetooth (use Meshtastic or Serial)
- 30 GPIO pins (GPIO 0-29)
- Dual-core 133MHz

## Pin Assignment Table (RP2040)

| GPIO | Function | Interface | Config File |
|------|----------|-----------|-------------|
| GP4 | DS18B20 Temperature Sensors | OneWire | config_core.json, config_mixing.json |
| GP16 | Meshtastic RX | UART2 | config_meshtastic.json |
| GP17 | Meshtastic TX | UART2 | config_meshtastic.json |
| GP21 | SSD1306 Display SDA | I2C | config_core.json |
| GP22 | SSD1306 Display SCL | I2C | config_core.json |
| GP25 | mix_radiator Motor Forward | Digital Out | config_mixing.json |
| GP26 | mix_radiator Motor Backward | Digital Out | config_mixing.json |
| GP27 | mix_floor Motor Forward | Digital Out | config_mixing.json |
| GP28 | mix_floor Motor Backward | Digital Out | config_mixing.json |

## Floor Heating Valves (config_floor.json)

⚠️ **Note:** Current pin assignments (32-35) are invalid for RP2040 (GPIO 0-29 only). Update required.

| Room | Current Pin | Suggested Pin |
|------|-------------|---------------|
| dnevna | 32 | GP2 |
| predsoba | 33 | GP3 |
| kuhinja | 34 | GP5 |
| hodnik | 35 | GP6 |

---

## Wiring Diagram

```text
                    YD-RP2040
                 ┌─────────────┐
                 │             │
    DS18B20 ─────┤ GP4         │
                 │             │
                 │ GP16   GP17 ├───── Meshtastic UART
                 │  RX     TX  │      (RX↔TX crossed)
                 │             │
    SSD1306 ─────┤ GP21   GP22 │
     SDA          │  SDA    SCL │
                 │             │
                 │ GP25   GP26 ├───── mix_radiator Motor
                 │  FWD    BWD │      (H-Bridge/Relay)
                 │             │
                 │ GP27   GP28 ├───── mix_floor Motor
                 │  FWD    BWD │      (H-Bridge/Relay)
                 │             │
                 │ GP2    GP3  ├───── Floor valves (dnevna, predsoba)
                 │ GP5    GP6  ├───── Floor valves (kuhinja, hodnik)
                 │             │
                 │ 3V3    GND  ├───── Power
                 └─────────────┘
```

## Connection Details

### DS18B20 Temperature Sensors (OneWire Bus)

- All sensors share **GP4** (OneWire protocol)
- Each sensor identified by unique 64-bit address
- Requires 4.7kΩ pull-up resistor between DATA and 3V3

```text
3V3 ──────┬─────────── DS18B20 VDD
          │
         4.7kΩ
          │
GP4 ──────┴─────────── DS18B20 DATA
                       
GND ──────────────────  DS18B20 GND
```

### SSD1306 OLED Display (I2C)

- **SDA**: GP21
- **SCL**: GP22
- I2C Address: 0x3C (typical)
- Power: 3.3V

### Meshtastic Radio Module (UART)

- **RP2040 TX (GP17)** → Meshtastic RX
- **RP2040 RX (GP16)** → Meshtastic TX
- Baudrate: 115200

### Mixing Valve Motors

Each valve motor requires H-bridge or dual relay:

| Valve | Forward | Backward | Action |
|-------|---------|----------|--------|
| mix_radiator | GP25 HIGH | GP26 LOW | Open valve |
| mix_radiator | GP25 LOW | GP26 HIGH | Close valve |
| mix_floor | GP27 HIGH | GP28 LOW | Open valve |
| mix_floor | GP27 LOW | GP28 HIGH | Close valve |



> Written with [StackEdit](https://stackedit.io/).
