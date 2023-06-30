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

To be completed:
+ Powering on
+ User login
+ Mouse / Keyboard connection

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

+ JMP1: When closed - connects the USB connector OTG_ID pin to the CM4's OTG_ID pin.
+ JMP2: When closed - connects the CM4's OTG_ID pin to a pull-down resistor.

If the device connected to the CFA050-PI-M's USB connector should determine device or host mode, close JMP1 and leave JMP2 open.  
To force the CFA050-PI-M to be a USB host, 

### Power Supply Options

+ JMP3: When closed - connects the USB connectors 5V pin to the CFA050-PI-M's and CM4's VDD (5V) supply.
+ JMP4: When closed - connects the center pin of the barel power connector, and J12 pin1 to the USB connectors 5V pin.
+ JMP5: When closed - connects J12 pin3 to the CFA050-PI-M's and CM4's VDD (5V) supply.
