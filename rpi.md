# firmware / software notes

> test: if service systemctl kill needs to run before starting
> fix GCC warnings on the hostname, etc.
> test: default config.txt and 
> fix: dependency on /home/moab/runtime/src/config.txt

# Some notes on RPI4 image setup

```
= Mac instructions for cat 5 on macOS initial connect
= Set DNS on the self-assigned to 1.1.1.1
= ECDSA host key `ssh-keygen -R raspberrypi.local`
= macOS share it's wifi over cat 5
= Disconnect from 802.1X wifi-profile, connect to MSFTGUEST
= on Windows: `ssh`, not `ssh.txt`
= change passwd
```

# Boot times
14.4 - 14.6: Bluetooth
14.96 ms ICMPv6

35 seconds to login: from cold boot
20 seconds if you rm //etc/systemd/system/dhcpcd.service.d/wait.conf
14 seconds if steps 1 and 2 from https://himeshp.blogspot.com/2018/08/fast-boot-with-raspberry-pi.html

# Change password
```
sudo passwd pi
```

# Firmware update

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

## Disable bluetooth and unused services

```bash
echo 'dtoverlay=pi3-disable-bt' | sudo tee -a /boot/config.txt
sudo systemctl disable hciuart.service
sudo systemctl disable bluetooth.service
sudo systemctl disable triggerhappy.service
sudo systemctl disable apt-daily.service
sudo systemctl mask rpi-eeprom-update
```

## Max out HDMI at 1080p
From http://rpf.io/configtxt


# Larger console font 
manpage [CONSOLE-SETUP(5)](https://manpages.debian.org/stretch/console-setup/console-setup.5.en.html)

also: https://www.raspberrypi.org/forums/viewtopic.php?t=185758

See current resolution:
`/opt/vc/bin/tvservice -s`

Available mod
/opt/vc/bin/tvservice -m CEA

# 
```bash
echo 'FONTFACE="TerminusBold"' | sudo tee -a /etc/default/console-setup
echo 'FONTSIZE="14x28"' | sudo tee -a /etc/default/console-setup
```

Since HDMI out on RPI4 is 4k, set output to 1920x1080

```
# /boot/config.txt
video=HDMI-A-1:1920x1080M@60
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
echo 'LANG=en_US.UTF-8' | sudo tee /etc/default/locale

# echo 'en_US.UTF-8 UTF-8' | sudo tee -a /etc/local.gen
# sudo dpkg-reconfigure locales
```

# Set keyboard to US
manpage [keyboard(5)](https://manpages.debian.org/stretch/keyboard-configuration/keyboard.5.en.html)
```
echo 'XKBLAYOUT="us"' | sudo tee -a /etc/default/keyboard
echo 'XKMODEL="pc104"' | sudo tee -a /etc/default/keyboard
sudo setupcon

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

# set a suitable minimal vimrc and bashrc

# optional packages to install

```
sudo apt install -y neofetch
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

= safely shutdown rpi with a button
https://howchoo.com/g/mwnlytk3zmm/how-to-add-a-power-button-to-your-raspberry-pi
= microSD card performance
https://www.jeffgeerling.com/blog/2019/raspberry-pi-microsd-card-performance-comparison-2019
