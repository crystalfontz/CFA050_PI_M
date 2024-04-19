# Crystalfontz CFA050-PI-M Getting Started Guide

This document outlines how to install the Raspberry Pi operating system for use with the CFA050A0-PI-MBxT family of displays.
The Raspberry Pi OS is a light-weight, free operating system based on Debian GNU/Linux.  

The following steps should be performed preferably on a Windows PC/Laptop, and assumes the user has an intermediate level of Linux OS knowledge (console use, root privileges, etc).  

Part variants:
+ CFA050A0-PI-MBCT: Capacitive touch display
+ CFA050A0-PI-MBNT: Non-touch display

## OS Configuration

The required OS configurations for use with the CFA050A0-PI-MBxT family of displays are outlined below. The Raspberry Pi foundation supplies a very easy-to-use utility to write the Raspberry Pi OS to a microSD card/eMMC storage.
It is advised that you follow this method unless you have a specific reason not to do so. Already have the OS configured? Jump to the [config.txt changes](#configuring-the-os-for-the-cfa050a0-pi-mbxt).

### Setting up the image

1. Download and install the Raspberry Pi Imager utility from the Raspberry Pi software page available here: https://www.raspberrypi.com/software/
2. Once installed, open the utility and from the device dropdown select “Raspberry Pi 4”
3. Select Choose OS and from the window that opens, select “Raspberry Pi OS (other)”
4. Choose any 64-bit OS of your preference

#### For usage with a CM4-lite (without eMMC)
5.	Plug in the SD card to be used for the OS installation
6.  Select Choose Storage and select the previously inserted SD card
7.	Select Next on the utility and when prompted to apply OS customization settings, select Edit Settings and configure the fields as desired. This step is highly recommended as it prevents the first boot setup by default
8.	Once the settings have been configured, select Yes, and the SD card will begin to be written to by the utility

#### For usage with a CM4 (with eMMC)
5.  Install the required USB drivers and utility for the flashing process, namely, usbboot, provided by Raspberry Pi and available here: https://github.com/raspberrypi/usbboot
6.  While shorting TP3 and pin 4 of J7 (the pin closest to the uUSB port), plug in a microUSB cable between the board and PC. For shorting the two points, a tweezer works well. Please be careful to ensure the tweezer does not come into contact with pin 1 of J7 as this will short the board between VDD and GND and may cause permanent damage to the CFA050A0-PI-MBxT and/or the CM4
8.  If using Windows, to ensure the device has been booted in the correct mode, navigate to Device Manager and look for BCMXXXX Boot under "Universal Serial Bus devices"
8.  Run the previously installed rpiboot utility (this is part of the usbboot package). This will make the CM4 eMMC storage visible on the PC allowing it to be written to via Raspberry Pi Imager
9.  Select Next on the utility and when prompted to apply OS customization settings, select Edit Settings and configure the fields as desired. This step is highly recommended as it prevents the first boot setup by default
10.	Once the settings have been configured, select Yes, and the eMMC storage will begin to be written to by the utility

### Configuring the OS for the CFA050A0-PI-MBxT 
Once the Raspberry Pi OS has been written to the microSD card/eMMC storage, some boot options need to be changed to set it up for the CFA050A0-PI-MBxT family of displays.  
1. On the newly flashed SD card/eMMC navigate to the root directory of the storage medium and open the file named “config.txt” (bootfs partition)
2. Add the following lines to the bottom of the file. Any duplicate parameters in the file will be overwritten
```
#Settings for the Crystafontz CFA050-PI-M series of modules
#enable I2C and SPI peripherals
dtparam=i2c_arm=on
dtparam=spi=on
#enable I2C-0 pins 44/45 (appears as i2c-10)
dtparam=i2c_vc=on
#enable the required video driver to use DSI1
dtoverlay=vc4-kms-v3d
ignore_lcd=1
#display orientation (0=default, 1=90deg, 2=180deg, 3=270deg)
display_lcd_rotate=0
#enable the overlay for the CFA050-PI-M
dtoverlay=crystalfontz-cfa050_pi_m
#misc settings
camera_auto_detect=0
max_framebuffers=2
#enable USB on-the-go mode
otg_mode=1
```
3.	In the case that a CFA050A0-PI-MBCT (capacitive touch display variant) is used, the following line needs to be changed from:
```
dtoverlay=crystalfontz-cfa050_pi_m
```
to
```
dtoverlay=crystalfontz-cfa050_pi_m,captouch=on
```

## Additional Configurations

### Correcting the brightness of the display panel on first boot

It is likely that on the first boot, the display panel of the CFA050A0-PI-MBxT will be quite dim. To correct the brightness of the display, open Terminal, navigate to /sys/class/backlight/lcd-backlight, and open the file named brightness. The command to do so is as below:
```
sudo nano /sys/class/backlight/lcd-backlight/brightness
```
Change the value in the file to 255 to increase the panels' brightness to maximum (alternatively, to any brightness level in the range of 0-255 that the user may prefer)

### Correcting the orientation of the display 

When manually setting up the OS, the display may appear upside down. To correct the orientation, with the CFA050A0-PI-MBxT booted up, navigate to Start, Preferences, and Screen Configuration.

When the subsequent Screen Layout Editor window opens, navigate to Layout, Screens, DSI-1, Orientation, and select “inverted”. This will invert the display and all touch co-ordinates (if the capacitive touch variant of the display is used) to the correct orientation

### On-screen keyboard

If an on-screen keyboard is needed, use the "Keyboard" utility which can be found in the Desktop main menu under Accessories


