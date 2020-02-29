# Changes to Sean's wiki

= Mac instructions for cat 5 on macOS initial connect
= Set DNS on the self-assigned to 1.1.1.1
= ECDSA host key `ssh-keygen -R raspberrypi.local`
= macOS share it's wifi over cat 5
= Disconnect from 802.1X wifi-profile, connect to MSFTGUEST
= on Windows: `ssh`, not `ssh.txt`
= change passwd

# Boot times
14.4 - 14.6: Bluetooth
14.96 ms ICMPv6

# 1. Change password
```
sudo passwd pi
```

# 1. Firmware update

Two EEPROMS (boot loader and USB controller VL805).
137ab ~ lower power consumption and temps w/power save

```
sudo rpi-eeprom-update

	BCM2711 detected
	BOOTLOADER: up-to-date
	CURRENT: Tue 10 Sep 10:41:50 UTC 2019 (1568112110)
	LATEST: Tue 10 Sep 10:41:50 UTC 2019 (1568112110)
	VL805: update required
	CURRENT: 000137ab
	LATEST: 000137ad
```

Then reboot. Idle before was 54.0 C; now 49.0 C.
As of 2020-02-22: 1568112110/137ad 

# Disable automatic Firmware update check
`sudo systemctl mask rpi-eeprom-update`

# Larger console font 

```
ls /usr/share/consolefonts/Uni3*
echo 'FONTFACE="TerminusBold"' | sudo tee -a /etc/default/console-setup
echo 'FONTFACE="14x28"' | sudo tee -a /etc/default/console-setup
sudo /etc/init.d/console-setup restart

video=HDMI-A-1:1920x1080M@60

# /boot/cmdline.txt
>> consider hdmi_force_hotplug=1 (added by NOOBS)
```

setup rip with cloud-init
https://dev.to/wesleybatista/setup-raspberry-pi-3-model-b-with-ubuntu-server-and-ssh-over-wifi-4d41


# Script to check CPU temperature

```
#!/usr/bin/env bash
uptime
sudo vcgencmd measure_temp
sudo vcgencmd measure_volts
```

# Set timezone
```
set timezone
sudo cp /usr/share/zoneinfo/US/Pacific /etc/localtime
```

## SSH

```
sudo touch /boot/ssh

Put a filecalled "ssh" on the /boot partition, or:
sudo systemctl enable ssh
sudo systemctl start ssh
```

# setup wifi country-code (important)
sudo raspi-config 


Strategy to  script setup via raspi-config script
https://gist.github.com/damoclark/ab3d700aafa140efb97e510650d9b1be

# Set to US locale
```
echo 'en_US.UTF-8 UTF-8' | sudo tee -a /etc/local.gen
sudo dpkg-reconfigure locales
```

boot headless
Pi Zero W
https://www.hardill.me.uk/wordpress/2017/01/23/raspberry-pi-zero-gadgets/

Pi4
https://www.hardill.me.uk/wordpress/2019/11/02/pi4-usb-c-gadget/

CalyanTether
https://marcelwiget.blog/2018/12/02/tether-rpi-to-ipad-pro-via-ethernet-over-usb-c/

Actual instructions from rip.org
https://gpiozero.readthedocs.io/en/stable/pi_zero_otg.html

Another way from a gist
https://gist.github.com/gbaman/975e2db164b3ca2b51ae11e45e8fd40a

# Remove (or set) MOTD for Moab
```
sudo rm /etc/motd
```

# Packages to install

```
sudo apt install -y neofetch
```

Try it out
```
neofetch --backend off
uname -r
```


# NOTES
> Consider ascii version of Moab
> Consider installing GUI but defaulting to CLI
> Skip eeprom check; disable bluetooth
> SolarizedLight http://github.com/joepvd/tty-solarized
> https://github.com/samartzidis/RaspiKey

# DEBUG
Bootloader failure diagnosis and fix:
https://jamesachambers.com/raspberry-pi-4-bootloader-firmware-updating-recovery-guide/

# SPEEDUP
https://github.com/samartzidis/RaspiKey/blob/master/setup/setup/setup.sh
