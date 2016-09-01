# Accessing root on Tessel 2

If you wish to access root on Tessel 2 and explore its Linux filesystem, you have a few options.

The simplest option is `t2 root`, which gives SSH access to a Tessel 2 you are authorized with.

However, while you're building tools for Tessel, you may need to access Tessel in ways not exposed by the CLI. Here are two options:

## SSH

If you can connect to your Tessel 2 over LAN, SSH or `t2 root` are your best options.

### Setup

Make sure your Tessel is [online](http://tessel.github.io/t2-start/wifi.html). Check that it shows up on LAN with `t2 list`.

Run `t2 provision` to authorize your computer with your Tessel.

### SSH in

Use the command `ssh root@<tesselname>.local -i ~/.tessel/id_rsa`.

The `-i` command lets you specify the filepath to the RSA keys written by `t2 provision`.

### Getting out

When you need out, type `exit`.

## dterm

Dterm lets you talk to Tessel over USB.

### Setup

#### OS X

*Unfortunately, dterm isn't supported on El Capitan and later. You may be able to use [screen](http://www.kinnetica.com/2011/05/29/using-screen-on-mac-os-x/) instead.*

Install dterm with `brew install dterm`.

Figure out what USB port your Tessel is plugged into by typing `ls /dev/tty.usbmodem` – see whether it auto-completes to e.g. `/dev/tty.usbmodem1412` or a different number.

#### Linux

Download and build dterm from http://www.knossos.net.nz/resources/free-software/dterm/, or use screen, which comes with most Linux distros.

The path to the serial port is `/dev/ttyACM0` or /`dev/serial/by-id/usb-Technical_Machine_Tessel_2_<serial number>-if01` - find out by typing `ls` for those locations.

#### Windows

Not sure, sorry... if you know how to access serial on Windows, please PR.

### dterm in

Run `dterm /dev/tty.usbmodem1412` (or a different port number).
You will have to press enter twice.

If you're going to do this a lot, it might be a good idea to alias the command: `alias v2="dterm /dev/tty.usbmodem1412"`.

If you need your ip address (for example if you'd like to ssh in next time), you can run `ifconfig` to get it.

### Getting out

`CTRL+]`

then

`CTRL+C`
