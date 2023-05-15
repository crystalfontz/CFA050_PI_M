# CFA050_PI_M

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

## Linux / RaspberryPi OS Setup

The official [Raspberry Pi OS distribution](https://www.raspberrypi.com/software/operating-systems/) as of the May 3rd 2023 release includes support for the Crystalfontz CFA050-PI-M modules.

Information on the OS included with the CFA050-PI-M can be [found here](OS-Setup.md).

## Further Information / Use

### Screen Orientation

Display panel orientation can be conigured using the "Screen Configuration" utility located in the Desktop main menu under Preferences.
It can also be changed from the command line by using "xrandr --display :0.0 --output DSI-1 --rotate \<direction\>", where \<direction\> can be normal, left, right, inverted.

### On-screen Keyboard

The "matchbox-keyboard" package has been installed to provide and on-screen keyboard for use with a touch-panel.
The "Keyboard" utility can be found in the Desktop main menu under Accessories.

### GPIOs

The "gpiod" package is installed for the 40-pin header and extra 6-pin GPIO control.
Running "gpiodetect" will list the avaliable GPIO chips. The RapberryPi 40pin header will be listed as "raspberrypi-exp-gpio". The Extra 6 GPIOs on the CFA050 will be listed as "pcf8574" (the pcf8574 has 8 GPIO pins, but the first two are not availible for user control).
The command "gpioinfo" will display a full list of GPIOs, and thier names.
To view the state of a GPIO pin use the command "gpioget". To set the state of a GPIO pin, use "gpioset".
