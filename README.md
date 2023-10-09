# Crystalfontz CFA050-PI-M Series Information

## Hardware Introduction

The Crystalfonzt CFA050-PI-M series of modules are primarily based around a 5-inch TFT LCD panel, and a Raspberry Pi Compute Module 4 (CM4).
The use of a Raspberry Pi CM4 (instead of a normal full-sized Pi 4) makes for a very powerful, compact, robust and versatile computing or HMI module.

Features include:
+ 5 inch TFT, 1280x720 pixel, high-brightness display
+ Multi-touch capacitive or no touch screen options
+ [Raspberry Pi CM4](https://www.raspberrypi.com/products/compute-module-4/) based (quad-core 64bit SoC @ 1.5GHz, 1/2/4/8GB RAM options, microSD or 8/16/32GB eMMC storage options)
+ A full Raspberry Pi standard 40 pin IO header
+ Compact expansion header access to PCI-E, HDMI, SPI and Ethernet
+ An extra six GPIO lines from an on-board I2C IO expander IC
+ Battery backed up real time clock
+ Flexible power supply options (barrel-jack, Micro USB or PCB header)

## Quick Start Guide

### Raspberry Pi CM4 Installation

If the CFA050-PI-M is not supplied with a Raspberry Pi CM4 module, one must be fitted. All variants of the official Raspberry Pi CM4 module are supported (Lite, EMMC, with or without Wifi, etc).
The mating connectors on both the CM4 and CFA050-PI-M are fragile, so care is required when plugging the CM into the CFA050-PI-M. Make sure the CM4 is aligned and oriented correctly (see CFA050-PI-M datasheet for example images), then press down evenly and firmly on the CM4 until it snaps into place. Repeated removal and re-fitting of the CM4 may damage the connectors.
It is recommended that M2.5 screws are used to hold the CM4 in place. If using a heatsink, fit as per the heatsinks instructions.

### Power Supply Configuration

The CFA050-PI-M supports a variety of power configurations via solder-jumpers for flexibility of use.
For more information on power supply configuration options, please see the CFA050-PI-M datasheet.
Most commonly the CFA050-PI-M will be powered with a 5V supply via the barrel connector, the USB connector, the J7 header or the J12 header.

To support any of these four options, the following jumpers need to be soldered closed (bridged):
+ JMP3
+ JMP5
+ JMP6

The 5V power supply should be of good quality and be capable of supplying 1.5 amps at a minimum. The Raspberry Pi CM4 module is quite sensitive to voltage fluctuations and may not operate correctly if the supplied voltage drops below 4.8V at any time (as measured at the CFA050-PI-M).

Note: in this configuration, a power supply connected to the barrel jack will also supply 5V to the USB connector (this may be useful for supplying power to USB connected devices, like a keyboard, mouse, etc).

### EMMC Preparation

CFA050-PI-M modules sold by Crystalfontz with a Raspberry Pi CM4 already attached will already have the OS loaded into the EMMC or MicroSD card and be ready to go (you can skip this section).

(todo: EMMC instructions)

### MicroSD Card Preparation

CFA050-PI-M modules sold by Crystalfontz with a Raspberry Pi CM4 already attached will already have the OS loaded into the EMMC or MicroSD card and be ready to go (you can skip this section).

If a MicroSD card needs to be written with the OS image:
+ Install a image writing utility (rpi imager, rufus, img etcher, etc) on a PC
+ Download the CFA050-PI-M compressed OS image file from the Crystalfontz Website (todo: add link)
+ Plug the MicroSD card into the PC using a USB MicroSD card reader or similar
+ Run the image writing utility and write the CFA050-PI-M compressed image file to the card (the image file may need to be uncompressed first)
+ If using the official Raspberry Pi Imager utility, under "Operating system" select "use custom"
+ Make sure writing to the MicroSD card has completed, then remove the MicroSD card and insert in the slot on the CFA050-PI-M

### USB Device Connection (Keyboard/Mouse)

By default the CFA050-PI-M and Raspberry Pi CM4 are configured in USB On-The-Go (OTG) mode (JMP1 is closed).
If a USB OTG adaptor cable is plugged into the CFA050-PI-M, common USB peripheral devices like a keyboard, mouse, USB hub, etc may be then connected and used as they would with any other computer.
When using a USB OTG adaptor cable, the CFA050-PI-M will need to be powered with 5V via the barrel connector, J7 or J12 (see the power supply configuration section above). The 5V power supply must be able to supply enough current to power the CFA050-PI-M, plus any USB devices that are connected.
(todo: add link / more information on USB OTG cables)

### First Power-On / Login

On applying power to the CFA050-PI-M module, a green power/activity LED on the rear of the CFA050-PI-M module should light and then start blinking with EMMC/MicroSD drive read activity.
After approximately 30sec (boot time may be longer on first boot due to extra OS initialisation routines), the LCD panel should be lit and displaying the Linux OS desktop.
If the CFA050-PI-M does not have a touchscreen fitted, a USB mouse and/or keyboard will need to be attached to interact with the OS desktop (see USB device connection section above).
The username for the default user is "pi" and the password is "raspberry". The "pi" user has sudo'ers permissions. It is recommended that the default password is changed as soon as possible.

## Linux / RaspberryPi OS Setup

The official [Raspberry Pi OS distribution](https://www.raspberrypi.com/software/operating-systems/) as of the May 3rd 2023 release includes support for the Crystalfontz CFA050-PI-M modules.

Information on the OS included with the CFA050-PI-M can be [found here](OS-Setup.md).

## Further Information

### Screen Orientation

Display panel orientation can be conigured using the "Screen Configuration" utility located in the Desktop main menu under Preferences.

### On-screen Keyboard

If an on-screen keyboard is needed, use the the "Keyboard" utility which can be found in the Desktop main menu under Accessories.

### GPIOs

The "gpiod" package is installed for the 40-pin header and extra 6-pin GPIO control.
Running "gpiodetect" will list the avaliable GPIO chips. The RapberryPi 40pin header will be listed as "raspberrypi-exp-gpio". The Extra 6 GPIOs on the CFA050 will be listed as "pcf8574" (the pcf8574 has 8 GPIO pins, but the first two are not availible for user control).
The command "gpioinfo" will display a full list of GPIOs, and thier names.
To view the state of a GPIO pin use the command "gpioget". To set the state of a GPIO pin, use "gpioset".

### USB Options

+ JMP1: When closed - connects the USB connectors OTG_ID pin to the CM4's OTG_ID pin (cable determines USB host or slave mode)
+ JMP2: When closed - connects the CM4's OTG_ID pin to a pull-down resistor (forces the CM4 to behave as a USB host)
+ JMP7: When closed - forcefully switches USB connection to use the CM4 breakout connector (J13)

If the device connected to the CFA050-PI-M's USB connector should determine device or host mode, close JMP1 and leave JMP2 open.  
To force the CFA050-PI-M to be a USB host, 

### Power Supply Options

+ JMP3: When closed - connects the USB connectors 5V pin to the CFA050-PI-M's and CM4's VDD (5V) supply
+ JMP4: When closed - connects the center pin of the barrel jack power connector and J12 pin1 to the USB connectors 5V pin
+ JMP5: When closed - connects J12 pin3 to the CFA050-PI-M's and CM4's VDD (5V) supply
+ JMP6: When closed - connects the center pin of the barrel jack power connector to the CM4's VDD (5V) supply

