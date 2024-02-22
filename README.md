# Crystalfontz CFA050-PI-M Series Information

## Hardware Introduction

The Crystalfontz CFA050-PI-M series of modules are primarily based around our 5-inch, 4-lane MIPI DSI TFT offerings, namely, our [CFAF71201280-050TC](https://www.crystalfontz.com/product/cfaf7201280a0050tc-720x1280-capactive-touchsceen-tft-lcd-display) and [CFAF7201280-050TN](https://www.crystalfontz.com/product/cfaf7201280a0050tn-720x1280-mipi-dsi-tft-display), and a Raspberry Pi Compute Module 4 (CM4).
The use of a Raspberry Pi CM4 (instead of a normal, full-sized Pi 4) makes for a very powerful, compact, robust and versatile computing or Human Machine Interface (HMI) module.

Features include:
+ 5 inch TFT, 1280x720 high pixel density, high-brightness display
+ Multi-touch capacitive or non-touchscreen options
+ [Raspberry Pi CM4](https://www.raspberrypi.com/products/compute-module-4/) based (quad-core 64bit SoC @ 1.5GHz, 1/2/4/8GB RAM options, microSD or 8/16/32GB eMMC storage options)
+ A full Raspberry Pi standard 40 pin IO header
+ Compact expansion header access to PCI-E, HDMI, SPI and Ethernet
+ An extra six GPIO lines from an on-board I2C IO expander IC
+ Battery backed up real time clock
+ Flexible power supply options (barrel-jack, Micro USB or PCB header)

Here are links to our active products:\
[CFA050A0-PI-MBCT](https://www.crystalfontz.com/product/cfa050a0pimbct) (Capacitive touch variant)\
[CFA050A0-PI-MBNT](https://www.crystalfontz.com/product/cfa050a0pimbnt) (Non-touch variant)

## Quick Start Guide

### Raspberry Pi CM4 Installation

The CFA050A0-PI-MBxT must be fitted with a Raspberry Pi CM4 module. All variants of the official Raspberry Pi CM4 module are supported (Lite, EMMC, with or without Wifi, etc).
The mating connectors on both the CM4 and CFA050A0-PI-MBxT are fragile, so care is required when plugging the compute module. Make sure the CM4 is aligned and oriented correctly (please refer to the CFA050A0-PI-MBxT datasheet for example images), then press down evenly and firmly on the CM4 until it snaps into place. Repeated removal and re-fitting of the CM4 may damage the connectors.
It is recommended that M2.5 screws are used to hold the CM4 in place. If using a heatsink, fit as per the heatsinks instructions.

### Power Supply Configuration

The CFA050A0-PI-MBxT supports a variety of power configurations via solder-jumpers for flexibility of use.
For more information on power supply configuration options, please refer to the module's datasheet.
Most commonly, the CFA050A0-PI-MBxT will be powered with a 5V supply via the barrel connector, the USB connector, the J3 header, the J7 header, or the J12 header.

The 5V power supply should be of good quality and be capable of supplying 1.5 amps at a minimum. The Raspberry Pi CM4 module is quite sensitive to voltage fluctuations and may not operate correctly if the supplied voltage drops below 4.8V at any time (as measured at the CFA050A0-PI-MBxT).

To supply power through the barrel jack, JMP7 needs to be soldered closed (bridged), connecting the power nets of the barrel jack and board.
Note: in this configuration, a power supply connected to the barrel jack will also supply 5V to the USB connector (this may be useful for supplying power to USB connected devices, like a keyboard, mouse, etc).

### Operating System Configuration

The official [Raspberry Pi OS distribution](https://www.raspberrypi.com/software/operating-systems/) as of the May 3rd, 2023 release includes support for the Crystalfontz CFA050A0-PI-MBxT modules.

For a complete guide on configuring an OS for use with the CFA050A0-PI-MBxT family of displays, please refer to our [Getting Started Guide](OS-Setup.md).

### First Power-On / Login

On applying power to the CFA050A0-PI-MBxT module, a green power/activity LED on the rear of the module should light and then start blinking with EMMC/MicroSD drive read activity.
After approximately 30 seconds (boot time may be longer on first boot due to extra OS initialisation routines), the LCD panel should be lit and display the Linux OS desktop.
For the CFA050A0-PI-MBNT, a USB mouse and/or keyboard will need to be attached to interact with the OS desktop (see USB device connection section below).

### USB Device Connection (Keyboard/Mouse)

By default, the CFA050A0-PI-MBxT and Raspberry Pi CM4 are configured in USB On-The-Go (OTG) mode (JMP3 is closed).
If a USB OTG adaptor cable is plugged into the CFA050A0-PI-MBxT J7 header, common USB peripheral devices like a keyboard, mouse, USB hub, etc may be then connected and used as they would with any other computer.
When using a USB OTG adaptor cable, the CFA050A0-PI-MBxT will need to be powered with 5V via the barrel connector, J3, or J12 (see the power supply configuration section above). The 5V power supply must be able to supply enough current to power the CFA050A0-PI-MBxT, plus any USB devices that are connected.

## Further Information

### GPIOs

The "gpiod" package is required to control the additional 6-pin GPIOs.
Running "gpiodetect" will list the available GPIO chips. The Raspberry Pi 40-pin header will be listed as "raspberrypi-exp-gpio". The Extra 6 GPIOs on the CFA050A0-MBxT will be listed as "pcf8574" (the pcf8574 has 8 GPIO pins, but the first two are not available for user control).
The command "gpioinfo" will display a full list of GPIOs, and their names.
To view the state of a GPIO pin use the command "gpioget". To set the state of a GPIO pin, use "gpioset".

### USB Options

+ JMP2: When closed - connects 5V power to the USB device connected on J7 (useful when using devices such as a keyboard and mouse)
+ JMP3: When closed - connects the USB connector OTG_ID pin to the CM4's OTG_ID pin (the cable determines USB host or slave mode)
+ JMP4: When closed - connects the CM4's OTG_ID pin to a pull-down resistor (forces the CM4 to behave as a USB host)
  
If the device connected to the CFA050A0-PI-MBxT USB connector should determine device or host mode, close JMP3 and leave JMP4 open to force the CFA050A0-PI-MBxT to be a USB host.

### Power Supply Options

+ JMP1: When closed - connects the center pin of the barrel jack power connector to the USB connectors 5V pin
+ JMP2: When closed - connects the USB connectors 5V pin to the CFA050A0-PI-MBxT and CM4's VDD (5V) supply
+ JMP6: When closed - connects J12 pin 3 to the CFA050-PI-M's and CM4's VDD (5V) supply
+ JMP7: When closed - connects the center pin of the barrel jack power connector to the CM4's VDD (5V) supply

