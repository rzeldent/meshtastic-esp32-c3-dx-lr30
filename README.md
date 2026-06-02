# Meshtastic ESP32-C3 + DX-LR30

DIY Meshtastic node using an ESP32-C3 supermini and an SX1262-based DX-LR30 LoRa module.

This repository includes:
- PCB schematic files
- PCB layout and EasyEDA board files
- Generated Gerber files for fabrication
- Modified Meshtastic firmware support for the Heltec ESP32-C3 variant with custom pin mappings

The design is intended to provide a compact custom Meshtastic node with a small OLED display and LoRa radio.

## Hardware

### Target modules
- DX-LR30 LoRa module (SX1262)
- ESP32-C3 supermini board
- Standard I2C OLED module (SSD1306-compatible)

### Recommended sourcing
- DX-LR30 module: https://nl.aliexpress.com/item/1005009833029680.html
- ESP32-C3 supermini board: https://nl.aliexpress.com/item/1005006056663228.html
- I2C OLED module: https://nl.aliexpress.com/item/1005007551771400.html

> Important: Do not use 433 MHz DX-LR30 variants for Meshtastic if you intend to run 868 MHz firmware. This design targets 868 MHz operation (European Union frequencies).

## What is included

- `schematic/` — EasyEDA schematic files for the ESP32-C3 and DX-LR30 connections
- `pcb/` — EasyEDA PCB design files and layout
- Gerber export files for PCB fabrication
- Custom Meshtastic firmware changes for the Heltec ESP32-C3 board variant

## Firmware adaptation

This project is based on the Heltec ESP32-C3 board configuration. The firmware changes update pin definitions to match the DX-LR30 module and the custom board wiring.

To build the firmware:
1. Install PlatformIO.
2. Copy the modified files from this repository into `meshtastic/variants/esp32c3/heltec_esp32c3/`.
3. Open the Meshtastic project in PlatformIO.
4. Run the build command for the Heltec ESP32-C3 environment.
   - Example: `pio run -e heltec_esp32c3`
5. Upload to the ESP32-C3 using `pio run -e heltec_esp32c3 -t upload`.

## Pin mappings

### DX-LR30 wiring

| Board pin | ESP32-C3 GPIO | DX-LR30 signal | Function |
|---|---|---|---|
| `SS` | `GPIO8` | `NSS` / `CS` | SPI chip select |
| `SCK` | `GPIO9` | `SCK` | SPI clock |
| `MOSI` | `GPIO7` | `MOSI` | SPI MOSI |
| `MISO` | `GPIO6` | `MISO` | SPI MISO |
| `GPIO5` | `GPIO5` | `RESET` | Module reset |
| `GPIO3` | `GPIO3` | `DIO1` | LoRa interrupt / status |
| `GPIO4` | `GPIO4` | `BUSY` | LoRa busy |
| `3V3` | — | `VCC` | Power supply |
| `GND` | — | `GND` | Ground |

- `DIO0` and `DIO2` are not connected in this variant (`RADIOLIB_NC`).
- `GPIO8` is shared with the board blue LED, so radio activity may also light the LED and slightly increase power consumption.

### OLED wiring

| Board pin | ESP32-C3 GPIO | OLED signal | Function |
|---|---|---|---|
| `SDA` | `GPIO20` | `SDA` | I2C data |
| `SCL` | `GPIO21` | `SCL` | I2C clock |
| `3V3` | — | `VCC` | Power supply |
| `GND` | — | `GND` | Ground |

## Usage

- After flashing, pair the node using the Meshtastic mobile app.
- Use the app to configure region, frequency, and device settings.
- Verify the board is powered by 3.3V and that the LoRa antenna is properly connected.

## Known issues and notes

- MOSI and SCK are currently swapped in the implementation and should be fixed in a future revision.
- `GPIO8` is also connected to the blue LED, which increases idle power use.
- The red board LED cannot be turned off in this design, so it consumes some power continuously.
- An ESP32-S3 board with PSRAM is a better choice for a future version, because it supports message storage and forwarding more easily.
- I2C is remapped to `GPIO20` / `GPIO21`; the original ESP32 mapping was `GPIO1` / `GPIO0`.
- Unused GPIOs are available for additional peripherals such as a buzzer or extra I/O.

## Final result

The finished build is a compact Meshtastic node with a small OLED and DX-LR30 LoRa radio.

![Project - OLED side](assets/IMG_7261.jpg)

![Project - ESP/LoRa side](assets/IMG_7262.jpg)
