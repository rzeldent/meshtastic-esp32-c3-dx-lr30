# Meshtastic-esp32-c3-dx-lr30
DIY Meshtastic node with ESP32-C3 supermini and DX-LR30 LoRa module

This repository contains the schematic, PCB layout, and Gerber files needed to manufacture a PCB on https://easyeda.com/.
It also includes the modifications required to use the Meshtastic firmware with this hardware design.

The firmware is an adaptation of the `helltec_esp32c3` board configuration, with custom pin changes to support the DX-LR30 module and the supermini board wiring.

## Sourcing the DX-LR30 module

This design targets the SX1262-based DX-LR30 LoRa module. A known AliExpress listing is:

- [https://nl.aliexpress.com/item/1005009833029680.html](https://nl.aliexpress.com/item/1005009833029680.html)

Important: choose the **868 MHz version** of the LoRa module for this board. Do not use the 433 MHz or 915 MHz variants.

The ESP32-C3 supermini board used in this project can also be sourced from AliExpress:

- [https://nl.aliexpress.com/item/1005006056663228.html](https://nl.aliexpress.com/item/1005006056663228.html)

The OLED module is a standard I2C Oled module and can also be sources from AliExpress:

- [https://nl.aliexpress.com/item/1005007551771400.html](https://nl.aliexpress.com/item/1005007551771400.html)

## What is included

- `schematic/` - project schematic files for the ESP32-C3 supermini and DX-LR30 module.
- `pcb/` - PCB layout files and board design exported from EasyEDA.
- Gerber files for PCB fabrication.
- Modified Meshtastic firmware support for the Helltec ESP32-C3 variant with updated pin assignments.

## ESP32-C3 to DX-LR30 pin mapping

This board uses the Heltec ESP32-C3 `pins_arduino.h` and `variant.h` definitions. The SX1262-based DX-LR30 wiring is mapped as follows:

| Arduino pin name | ESP32-C3 GPIO | DX-LR30 signal | Function |
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

> Note: Pin names and numbering reflect the Heltec ESP32-C3 board variant. Use this table as the reference for the custom Meshtastic firmware adaptation and PCB wiring in this project.


## Building

- Use platformIO to build the esp32 firmware (I did not use the development container)
- Before compiling, replace the files in the Heltec esp32c3 directory with the files provided in the meshtastic directory; these will change the target and the pin mapping.
- Upload to the ESP32-C3 
- Use the Meshtastic app (from the Apple store to connect)

## Final result

The result is a compact Mestastic device

![](assets/IMG_7261.jpg)

![](assets/IMG_7262.jpg)
