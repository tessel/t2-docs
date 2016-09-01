# Debugging USB Communications

### Introduction to USB

For an in-depth description of how USB works, read [about a code deploy example](https://github.com/tessel/onboarding/blob/master/T2-TECHNICAL-OVERVIEW.md#a-code-deploy-example). Otherwise, the TL;DR is that USB communication from the CLI gets sent to the atmel samd21 coprocessor which then routes the packets over SPI to a daemon running on OpenWRT which then passes it on to another daemon whose only job is to create processes requested over USB and pipe the standard streams back to it. If that description caused your eyes to glaze over, that's okay because the only thing you need to take out of that is that for USB comms to be working, you need:
* The coprocessor to be powered and have valid firmware
* The SPI Daemon process running on OpenWRT
* The USB Daemon process running on OpenWRT.


The coprocessor should pretty much never be a problem unless you're tinkering with the firmware or the hardware. The SPI Daemon is relatively simple and has been well battle tested so that's unlikely to be broken. The USB Daemon is very often the culprit for USB issues.

### How to debug?
The easiest way to debug USB communication issues is to check out the status of the USB and SPI daemons on OpenWRT. To do that, you need to get a root shell. Check out this [short tutorial](https://github.com/tessel/onboarding/blob/master/SSH-AND-DTERM.md) for getting a root shell over SSH or USB. I recommend using SSH because some of the programs used to route serial traffic over dterm is shared with the USB communications from the CLI so if you're already having a problem with USB comms, dterm may not work due to the same cause.

Once you have a root shell, look for the following two processes by running `ps`:
```
 1169 root       772 S    spid /dev/spidev32766.1 2 1 /var/run/tessel
 1185 root       780 S    usbexecd /var/run/tessel/usb
```

The top line is the SPI Daemon and the bottom line is the USB Daemon. If neither of them are running, you will have an issue. To start (or restart) one or both of them, run:
```
/etc/init.d/spid restart
/etc/init.d/usbexecd restart
```
Once you restart them, you can try talking to Tessel from your host machine to see if that resolves the issue (`t2 list` is a good litmus test).