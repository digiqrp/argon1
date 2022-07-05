#Ubuntu Mate 20.04 Argon One Fan Control
Due to an issue with a kernel flag not being set the in the Ubuntu Kernal the current Argon One fan control script has an issue, the control system attempts to access /dev/mem to setup the shutdown button, this call fails with a segmentation fault. The recommendation was to add the current user to the "gpio" group or run this as sudo, however it does not work and fails to start due to the segmentation fault.

##Update
Even basic GPIO functions with Ubuntu 20.04 are now not possible, we did attempt to use GPIOZero and this library also has issues when communicating with the GPIO in the exact same way rpi.GPIO does. This is down to a simple fact that you cannot use the older GPIO libraries with Ubuntu 20.04, there is a new version called python3.lgpio but this only functions on Ubuntu 21.04 which has an updated version of Python to version 3.9. Even updating Python to 3.9 on Ubuntu 20.04 does not permit us to install the new library.

##Introduction
The Argon One case requires a one-line script to be run on the Raspberry Pi using the case's fan function correctly this script has been patched to address this issue.

It works with raspbian however the reason for the segfault is that the Ubuntu kernel is now built with CONFIG_STRICT_DEVMEM set true. It is unset in the raspbian kernel. (It is a setting under Kernel Hacking) you can only enable this by a kernal recompile which is not ideal. 
Description
The Argon One case is a solid Aluminum alloy case for the Raspberry Pi 4 that offers both passive and active cooling (see https://www.argon40.com/argon-one-raspberry-pi-4-case.html).

This script has been adapted from the original installation script which was designed to work with Raspbian (Raspberry OS); the modified scripted has been developed for use with Ubuntu 20.04 and Ubuntu Mate 20.04.

The modified script contains three changes to the original script:

Switch to python3 since python2 support for GPIO has been nuked in Ubuntu 20.04.
Reading the temperature cannot be done using vcgencmd, so use sysfs instead.
Package dependencies have been adapted for Ubuntu & Ubuntu Mate 20.04.
Install
Feel free to review the source code of the modified script before installation.

cd /tmp/
wget https://www.digiqrp.com/storage/downloads/argon1/argon1.sh
chmod a+x argon1.sh
sudo ./argon1.sh
When the script has completed run "sudo systemctrl start argononed" and then "sudo systemctrl status argononed", there is no need to re-boot.

##Usage
Upon installation of the Argon One Pi 4 script for Ubuntu 20.04, the setting of the Argon one Pi 4 cooling system are as follows:

CPU Temp	Fan Power
55 C	10%
60 C	55%
65 C	100%

However, you may change or configure the Fan to your desired settings by using argonone-config to configure the fan behavior.

You can also from this utility set as may temperature steps that you wish with option 3, you can also edit the configuration manually in /etc/argononed.conf 

For example 

45=3
50=6
55=10
60=70
65=100

##Commands
The script will generate several scripts and config files. The main commands are

argonone-config to config the fan behavior
argonone-uninstall to remove all the scripts and services (conf file remains though)
I also added a custom command:

argonone-tempmon which monitors the temperature using the Linux sysfs.
We have been testing this for over a week now and works perfectly 

##Warning
This has been developed in an afternoon and has been only tested on my Raspberry Pi 4. No issues so far, but big disclaimer nonetheless: use at your own risk.