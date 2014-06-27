HLK-RM04-GPIO
=============

Repository for the GPIO client/server project

6/27/14
files - place this at the trunk level of the OpenWRT tree
HLK-RM04.dts - Device tree that enables GPIO pins 8,9 (spi pins)
openwrt-ramips-rt305x-hlk-rm04-squashfs-sysupgrade.bin - Binary image to update the HLK-RM04

--Project Currently --
Boots, runs gpio_setup, S99broadcast_agent, S99listen_agent

All boot scripts get config options from the gpio_wifi file in etc/config

gpio_setup Sets the pins on the HLK-RM04 based on the info in gpio_wifi, gpios field.  Format is: pin#i(or o) for in or out. Ex: 1o,2i  is gpio1 output, gpio2 input

broadcast_agent sends the module's IP address, litening port, hostname, gpio values, date/time in YYMMDDhhmmss format

listen_agent listens for incoming connections.  Upon connect, it sends a salt for password hashing.  It returns nothing fron any input.  Any input is handed to the gpio_worker, which does 2 things currently.

gpio_worker has "hash" and "setpin" routines.  The "hash" function generates a new salt and hashs the stored (already hashed) password into a used-once /tmp file.  When "setpin" is called, it's expected to follow this format:

"setpin,gpio#:value,hash of hashed pw using sent salt>

A salt is never used again, so a hash of the hashed PW should never be the same again.
