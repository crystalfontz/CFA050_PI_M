# Crystalfontz CFA050-PI-M OS Setup

This document describes the process to replicate the Raspberry Pi OS that is supplied with the CFA050-PI-M when purchased from Crystalfontz.  
The Raspberry Pi OS is a light-weight free operating system based on Debian GNU/Linux.

## Writing the OS to a microSD card

The Raspberry Pi foundation supplies a very easy to use utility to write the Raspberry Pi OS to a microSD card.  
It is advised that you follow this method unless you have a specific reason not to do so.  

We suggest you use the 64-bit version of the OS with the Desktop.  

1. Insert a microSD card into your computer.
2. Download and run the [Raspberry Pi Imager available here](https://www.raspberrypi.com/software/).
3. Click "Choose OS", and select "Raspberry PI OS (other)", and then "Raspberry Pi OS (64-bit)".
4. Click "Choose Storage" and select the microSD card you inserted.
5. Click the gear icon button in the bottom right-hand corner of the software. Change the options as necessary.
6. Click the "Write" button to write the OS to the microSD card.
7. When complete, remove the microSD card from the computer.

## Configuring the OS for the CFA050-PI-M

Once the Raspberry Pi OS has been written to the microSD card some boot options need to be changed to set it up for the CFA050-PI-M.

1. Re-insert the microSD card into the computer.
2. Using your favorite text editor, edit the file "config.txt" on the microSD card
3. At the very bottom of the file add the following section:
```
[cm4]
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
```
3. If the CFA050-PI-M has a capacitive touch screen, the "captouch=on" option needs to be added. Change
```
dtoverlay=crystalfontz-cfa050_pi_m
```
to
```
dtoverlay=crystalfontz-cfa050_pi_m,captouch=on
```
## Extra Software / Configuration

### LCD Backlight Brightness

+ xfce4-power-manager, xfce4-power-manager-plugins
Add power managment to xfce panel.
Turn off automatic backlight powersave and backlight dimming.

Power on/off control:
+ Script '/lib/systemd/system/cfa050-backlight-off.service':
```
Unit]
Description=Turns off CFA050 backlight on shutdown/reboot
DefaultDependencies=no
Before=umount.target

[Service]
Type=oneshot
ExecStart=/bin/sh -c '/bin/echo 0 > /sys/class/backlight/lcd-backlight/brightness'

[Install]
WantedBy=reboot.target halt.target poweroff.target

```
+ Script '/lib/systemd/system/cfa050-backlight-on.service':
```
[Unit]
Description=Turns on CFA050 backlight on startup
DefaultDependencies=no

[Service]
Type=oneshot
ExecStart=/bin/sh -c '/bin/echo 255 > /sys/class/backlight/lcd-backlight/brightness'

[Install]
WantedBy=graphical.target

```
+ systemctl enable cfa050-backlight-on.service
+ systemctl enable cfa050-backlight-off.service
+ systemctl daemon-reload
+ reboot

## 



