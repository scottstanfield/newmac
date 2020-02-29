# Virtual Console (TTY)

Get a TTY console: `ctrl-alt-F3`.

VGA32x16
TerminusBold28x14
Uni30Terminus28x14


UTF-8 > Combined - Latin; Slavic Cyrillic; Greek > Terminus Bold > 14x28
```
	sudo dpkg-reconfigure console-setup
```

## Set TTY colors
https://github.com/joepvd/tty-solarized


# Mounting a USB drive

```
	sudo fdisk -l
```

## Log in as root and change password

		passwd

## Install all available updates

		apt-get update
		apt-get upgrade
		apt-get dist-upgrade

## Reboot

		reboot

## Install Fail2ban

		apt-get install fail2ban

Fail2ban is a daemon the monitors suspicious login attemps to a server
and blocks activity as it occurs. It's well configured out-of-the box. 

You can monitor it's work with `cat /var/log/fail2ban.log`

## Install a few useful things

		apt-get install htop git zsh
		apt-get install python-software-properties

## Setup timezone
Default timezone is GMT. This changes it to Pacific Time.

		cp /usr/share/zoneinfo/America/Los_Angeles /etc/localtime
		date

## Add administrative user

		adduser scott
		visudo

Now log out, then back in as the new user.

## Disable root for SSH

		sudo vi /etc/ssh/sshd_config

Make the following changes:

		LoginGraceTime 20
		PermitRootLogin no

Change `PermitRootLogin` to `no`. Save the file. Restart ssh:
Change 

		reload ssh

Logout then try to log back in as `root`. It should fail.

## Transfer public key (OPTION 1)
While you're on your local computer:

		scp ~/.ssh/id_rsa.pub scott@0.0.0.0:/home/scott
		ssh scott@0.0.0.0			# prompts for password

		mkdir .ssh
		mv id_rsa.pub .ssh/authorized_keys
		chmod 700 .ssh
		chmod 600 .ssh/authorized_keys
		exit

		ssh scott@0.0.0.0			# no password prompt

# Transfer public key (OPTION 2)

		brew install ssh-copy-id
		ssh-copy-id scott@0.0.0.0

## Disable root password
Disable logging in as root. 

		sudo usermod -p '!' root

## Harded SSH more with groups

		sudo su
		groupadd sshlogin
		usermod -a -G sshlogin scott
		vi /etc/ssh/sshd_config

Add this line to the bottom of the file:

		AllowGroups sshlogin

Save and restart with `reload ssh`.

Important! Keep this shell open and test with a new shell:

		ssh scott@0.0.0.0

Once you've confirmed that works, add a helper to your ~/.ssh/config
file:

		Host foo
			Hostname 0.0.0.0
			User scott

## Recap

At this point, you have one user, whose name is not well known, that is
allowed ssh access to your system on a non-standard ssh port. Only that
user can sudo. And only the group sshlogin is even allowed to use ssh.

Log back in as `root`

## Let Ubuntu upgrade itself automatically
From https://help.ubuntu.com/community/AutomaticSecurityUpdates
Using the "unattended-upgrades" package:

		dpkg-reconfigure -plow unattended-upgrades

## Setup log monitoring

		aptitude install logwatch
		logwatch

## Firewall
Use the simple firewall `ufw`

		ufw enable

Create a custom ufw "application":

		ufw logging on
		ufw allow ssh
		ufw limit ssh
		ufw status

## Fail2ban
Edit 



# Resources

https://help.ubuntu.com/community/StricterDefaults
http://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers
http://blog.vigilcode.com/2011/04/ubuntu-server-initial-security-quick-secure-setup-part-i/
