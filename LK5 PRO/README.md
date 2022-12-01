# LK5 PRO KLIPPER GUIDE

## INSTALL KLIPPER
If you want to start by installing OctoPi on the Raspberry Pi computer, you can find some methods to obtain and install Klipper under this link: [OctoPi](https://www.klipper3d.org/Installation.html)

If you want to start by installing FluiddPi on the Raspberry Pi computer, you can find some methods to obtain and install Klipper under this link: [FluiddPi](https://docs.fluidd.xyz/installation/fluiddpi)

If you want to start by installing MainsailOS on the Raspberry Pi computer, you can find some methods to obtain and install Klipper under this link: [MainsailOS](https://docs.mainsail.xyz/setup/)

## OBTAIN LK5 PRO CONFIG FILE

Each printer has its own KLIPPER configuration file. The configuration file of LK5 Pro can be obtained by using the SSH utility to connect to the Raspberry Pi and running the following commands:

```
cd ~
git clone https://github.com/LONGER3D/klipper-for-longer-3d-printers
```

## BUILD & FLASH FIRMWARE

Login to the Raspberry Pi via ssh, and run the following commands to prepare the firmware:

```
cd ~/klipper/
make menuconfig
```

Select the options:

![options.png](https://s2.loli.net/2022/12/01/GetnUVFNdXHfpBM.png#pic_center)

Once the configuration is selected, press “Q” to exit, and “Yes” when asked to save the configuration. Then, run the following command to build the firmware:

```
make
```

Connect your Longer LK5 Pro to the Raspberry Pi. Then, get the port of the ATMEGA 2560 by running the following command:

```
ls /dev/serial/by-id/*
```

It should report something similar to the following:

```
/dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0
```

Each printer has its own unique serial port name. Be sure to update the FLASH_DEVICE with the printer's unique serial port name. For the micro-controller of LK5 Pro, the code can be flashed with this unique serial port name similar to:

```
sudo service klipper stop
make flash FLASH_DEVICE=/dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0
sudo service klipper start
```

## CONFIGURING KLIPPER

The next step is to copy and edit the printer configuration file (printer-longer-lk5-pro-2021) to the Raspberry Pi. That may look something like the following (be sure to update the command to use the appropriate printer config filename):

```
cp ~/klipper-for-longer-3d-printers/LK5\ PRO/printer-longer-lk5-pro-2021.cfg ~/printer.cfg
nano ~/printer.cfg
```

Then update the [mcu] section with the printer’s unique serial port name to look something similar to:

```
[mcu]
serial: /dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0
```

Once the configuration is changed, press “Ctrl + x” to exit, and “Y” when asked to save the modified buffer. Then, issue “restart” in terminal until “status” reports the printer is ready.

&nbsp;

---
# OPTIONS: USING BLTOUCH
## READY
At this stage, it is assumed you have completed the KLIPPER configuration on your Longer LK5 Pro and successfully installed BLTOUCH on your printer.

It is now time to change the KLIPPER configuration for BLTOUCH.

## STEPPER_Z PARAMETERS

![stepper_z.png](https://s2.loli.net/2022/12/01/eEuqDBmnFKgZksN.png#pic_center)

To allow proper operation of the BLTOUCH, in the stepper_z section, we need to comment the endstop_pin and the position_endstop, and uncomment the endstop_pin, as shown in the figure.

## BLTOUCH PARAMETERS

![bltouch.png](https://s2.loli.net/2022/12/01/ipBoKkfM6F5ZYhH.png#pic_center)

Uncomment the section of bltouch, bed_mesh and safe_z_home in your printer.cfg.

If you use print head with dual-blower, please adjust the x_offset value to -52 and the y_offset value to -16.

If you use print head with single-blower, please adjust the x_offset value to -36 and the y_offset value to -10.

## SAVE & RESTART

After changing the configuration, don’t forget to save config then restart klipper so that all changes are account for.