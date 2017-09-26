Tested on Ubuntu 14.04

To get uploads working, you'll need to add a udev rule and set permissions correctly.

**udev Rules**

Make a new file called:

`/etc/udev/rules.d/77-arduino.rules`

In it, enter the following and save:

`ATTRS{idVendor}=="1b4f", MODE="0660", GROUP="dialout", ENV{ID_MM_DEVICE_IGNORE}="1"`

Then, restart the rules with:

`run sudo udevadm control --reload-rules`

**Permissions**

If you've successfully set your udev rules, running Arduino as root should allow uploads.  Rather then run as root all the time, make sure the user and the usb device are both in the `dialout` group.

To see what the device is comming up as, enter:

`ls -l /dev/ttyACM*`

which will list *all* ttyACM devices.  With only the LilyPad USB Plus plugged in, this should read something like:

`crw-rw---- 1 root dialout 166, 0 Sep 26 11:05 /dev/ttyACM0`

showing that the udev rule worked making the usb device `dialout`.

Use:

`groups`

to list the groups the current user is part of.  If `dialout` is not one of them, enter:

`sudo adduser username dialout`

where `username` is your user account.

Afterwards, log out and back in again and Arduino should be able to upload without running as root.
