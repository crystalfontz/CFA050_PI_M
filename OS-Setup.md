# Crystalfontz CFA050-PI-M OS Setup

This document describes the process to replicate the Raspberry Pi OS that is supplied with the CFA050-PI-M when purchased from Crystalfontz.  
The Raspberry Pi OS is a light-weight free operating system based on Debian GNU/Linux.  

The following information should be performed on a Linux PC/Laptop, and assumes the user has a intermediate level of Linux OS knowledge (console use, root privileges, etc).  

## Writing the OS to a microSD card

The Raspberry Pi foundation supplies a very easy to use utility to write the Raspberry Pi OS to a microSD card.  
It is advised that you follow this method unless you have a specific reason not to do so.  

1. Insert a microSD card into your computer.
2. Download and run the [Raspberry Pi Imager available here](https://www.raspberrypi.com/software/).
3. Click "Choose OS", and select "Raspberry PI OS (other)", and then "Raspberry Pi OS (64-bit)".
4. Click "Choose Storage" and select the microSD card you inserted.
5. Click the gear icon button in the bottom right-hand corner of the software. Change the options as necessary.
  (a username of "pi" and password of "raspberry" is used on the Crystalfontz supplied microSD card)
6. Click the "Write" button to write the OS to the microSD card.
7. When complete, remove the microSD card from the computer.

## Configuring the OS for the CFA050-PI-M

Once the Raspberry Pi OS has been written to the microSD card some boot options need to be changed to set it up for the CFA050-PI-M.  

1. Re-insert the microSD card into the computer.
2. Using your favorite text editor, edit the file "config.txt" (microSD card bootfs parition).
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
	#enable USB on-the-go mode
	otg_mode=1
	```
3. If the CFA050-PI-M has a capacitive touch screen, the "captouch=on" option needs to be added. Change:
	```
	dtoverlay=crystalfontz-cfa050_pi_m
	```
	to:  
	```
	dtoverlay=crystalfontz-cfa050_pi_m,captouch=on
	```

## Extra software / configuration

### Power on/off backlight control

On power-on/reboot the RaspberryPi OS may not set the backlight brightness to the desired level as the systemd backlight service appears to be unreliable.  
To make sure this always does occour (all on the microSD card rootfs parition):
+ Add the script file '/lib/systemd/system/cfa050-backlight-on.service' with the contents:
	```
	[Unit]
	Description=Turns on CFA050 backlight on startup
	DefaultDependencies=no
	After=lightdm.service systemd-backlight@.service

	[Service]
	Type=oneshot
	ExecStart=/bin/sh -c '/bin/echo 255 > /sys/class/backlight/lcd-backlight/brightness'

	[Install]
	WantedBy=graphical.target
	```
+ The value of 255 (maximum brightness) may be changed to a different value is needed.
+ Add the symlink "ln -sf /lib/systemd/system/cfa050-backlight-on.service etc/systemd/system/graphical.target.wants/cfa050-backlight-on.service".

To make sure the backlight is turned off on CFA050 power-off:
+ Add the script file '/lib/systemd/system/cfa050-backlight-off.service' with the contents:
	```
	[Unit]
	Description=Turns off CFA050 backlight on shutdown/reboot
	DefaultDependencies=no
	Before=umount.target

	[Service]
	Type=oneshot
	ExecStart=/bin/sh -c '/bin/echo 0 > /sys/class/backlight/lcd-backlight/brightness'

	[Install]
	WantedBy=reboot.target halt.target poweroff.target
	```
+ Add the symlink "ln -sf /lib/systemd/system/cfa050-backlight-off.service etc/systemd/system/poweroff.target.wants/cfa050-backlight-off.service"
+ Add the symlink "ln -sf /lib/systemd/system/cfa050-backlight-off.service etc/systemd/system/halt.target.wants/cfa050-backlight-off.service"
+ Add the symlink "ln -sf /lib/systemd/system/cfa050-backlight-off.service etc/systemd/system/reboot.target.wants/cfa050-backlight-off.service"

### Default user / password
Default username of 'pi' and password of 'raspberry' have been used (set using the Raspberry Pi Imager during microSD card creation).

### Disable screen-blanking
By default the Raspberry Pi OS blanks the screen after 10 mins of innactivity. This is disabled.  
+ Edit "/etc/xdg/lxsession/LXDE/autostart" and "/etc/xdg/lxsession/LXDE-pi/autostart" (microSD card rootfs parition). A append both with the following:  
	```
	@xset s noblank
	@xset s off
	@xset -dpms
	```

### Change desktop icons
The default Raspberry Pi OS icon is changed to a Crystalfontz icon.  
Appropriatly sized icons are copied into the "/usr/share/icons/PiXflat/XXxXX/places" (microSD card rootfs parition) directories, where "XXxXX" are
sizes 16x16, 24x24, 32x32 and 48x48.  
(these changes may be overwritten with an OS update).

### Change OS splash image
The default Raspberry Pi OS splash screen has been changed to a Crystalfonz logo image.  
The file replaced is "/usr/share/plymouth/themes/pix/splash.png" (microSD card rootfs parition).

### Change desktop background
The default Raspberry Pi OS desktop image has been replaced by a photo with Crystalfontz logo.  
The file replaced is "/usr/share/rpd-wallpaper/clouds.png" (microSD card rootfs parition).

### Added website to chromium browser
The Crystalfontz home page is opened on the first running on the Chromium web browser (default browser in Raspberry Pi OS).  
+ Edit "/etc/chromium/master_preferences" (microSD card rootfs parition) and change the line:  
	```
	"first_run_tabs":["https://welcome.raspberrypi.org/raspberry-pi-os?id=UNIDENTIFIED"]
	```  
	to  
	```
	"first_run_tabs":["https://www.crystalfontz.com","https://welcome.raspberrypi.org/raspberry-pi-os?id=UNIDENTIFIED"]
	```

### Remove first start setup
On first boot the Raspberry Pi OS normally runs through a user setup process. This process requires a keyboad and mouse, so these are disabled:
+ Delete the files "/etc/xdg/autostart/piwiz.desktop" and "/etc/xdg/autostart/pprompt.desktop" (microSD card rootfs parition).

### Creating new image of microSD card
To create a new IMG file from the modified MicroSD card so it can be replicated easily:
+ fdisk /dev/sdX (where sdX is the MicroSD device). Note last sector number of last parition, then exit.
+ dd if=/dev/sdX of=usdimage.img bs=512 count=<last-sector-num + 1>
