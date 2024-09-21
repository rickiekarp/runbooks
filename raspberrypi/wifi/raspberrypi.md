# README #

Enabling/Disabling wifi on raspberrypi 4

### How To ###

- open the file: `sudo nano /boot/config.txt`

- add `dtoverlay=pi3-disable-wifi` to the end of the file to disable wifi

- reboot

# Disable WiFi/Bluetooth

dtoverlay=pi3-disable-wifi
dtoverlay=pi3-disable-bt