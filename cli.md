# Tessel 2 Command Line Interface

* [Installation](#installation)
* [Updating Tessel](#updating-tessel-2-on-board-osfirmware)
* [Setup](#setup)
* [Usage/CLI commands](#usage)

## Installation
Prerequisites for installation: [Node.js](https://nodejs.org/)

`npm install -g t2-cli`

## Updating Tessel 2 On-board OS/Firmware
There are two components to the Tessel on-board software: The OpenWRT Linux image on the MediaTek processor and the firmware image that runs on the Atmel co-processor.

### How do I know if I need to update my T2?
Unfortunately, [we have an open PR](https://github.com/tessel/t2-cli/pull/130) to detect the version of code running on T2 but it hasn't merged yet! If you aren't seeing your Tessel 2 show up with `t2 list`, then you probably have an old version of firmware (because the USB VID/PID is out of date). If you see functionality in the [Tessel API](https://github.com/tessel/t2-firmware#t2-hardware-api) that isn't defined on your board, then you probably need to update.

### Updating
Eventually, this will be [rolled into the CLI](https://github.com/tessel/t2-cli/issues/81) but it hasn't been built yet. For now:

1. Update your co-processor firmware by following the instructions [here](https://github.com/tessel/t2-firmware/#compiling)
2. Update the OpenWRT image on the primary processor by either [building it yourself](https://github.com/tessel/openwrt-tessel) or downloading [a prebuilt binary](https://kevinmehall.net/tmp/openwrt/openwrt-ramips-mt7620-tessel-squashfs-sysupgrade.bin) (if you're not sure which you want to do, use
3. Transfer it to your T2 (often accomplished with `scp -i ~/.tessel/id_rsa PATH_TO_IMAGE root@IP_ADDR_OF_T2:/tmp`) and running `sysupgrade /tmp/PATH_TO_IMAGE` on your T2. Make sure it's uploaded to the `/tmp` folder on T2 - any other folder will ruin the upgrade. If you need to find the IP Address of your T2 but don't know how, first connect it to your network (`t2 wifi -n YOUR_SSID -p YOUR_PASSWORD`)
4. Run `t2 list` to make sure it's actually connected over the LAN, and note its name.
5. Run `ping YOUR_NAME.local` in order to see the IP address (you can also give it a simpler name first with `t2 rename NEW_NAME`. If you already know what dterm is, you can just dterm into the device and run `ifconfig`.
6. Wait for it to automatically restart itself and you're good to go!

## Setup

### USB
Connecting to a Tessel 2 over USB requires no special setup.

### LAN
In order to authorize the device with your computer to work over a LAN connection, call `t2 provision` after connecting it via USB. This will place an SSH key on the device. Use the `t2 wifi` command as described below to connect Tessel 2 to a local network. You should now be able to access your Tessel 2 remotely.

### Virtual Machine
Check out the [Virtual Machine repo](https://github.com/tessel/t2-vm) for instructions on how to set up the VM. All CLI commands except `provision` and `wifi` should be functional with the VM.

## Usage
Specify which Tessel to use with the `--name <name>` option appended to any command.
If `--name` is not specified, CLI will look for an environment variable, e.g. `export TESSEL=Bulbasaur`. If none of the above are specified and there is one Tessel connected over USB, this Tessel will be preferred. Finally, if there is only one Tessel available and none of the above are specified, CLI will choose that Tessel.

### Starting Projects
* `t2 init` in the current directory, create a package.json and index.js with Hello World code. *Note that the index.js code doesn't yet work on Tessel 2*

### Tessel Management
* `t2 provision` authorize your computer to access a Tessel over SSH (USB-connected Tessel only)
* `t2 list` show what Tessels are available over WiFi and USB.
* `t2 rename` change the name of a Tessel

### Code Deployment 
* `t2 run <file>` copy the file and its dependencies into Tessel's RAM & run immediately. Use this during development of your device application.
  * `[--lan]` deploy over LAN connection
  * `[--usb]` deploy over USB connection
  * `[--slim]` true by default, copy only files needed by the program to run
  * `[--full]` the opposite of --slim, copy all the files in the project directory
* `t2 push <file>` copy the file and its dependencies into Tessel's Flash memory & run immediately. Once deployed with `push` command, the device application will automatically run every time the Tessel restarts. 
  * `[--lan]` deploy over LAN connection
  * `[--usb]` deploy over USB connection
  * `[--slim]` true by default, copy only files needed by the program to run
  * `[--full]` the opposite of --slim, copy all the files in the project directory
* `t2 erase` erase any code pushed using the `t2 push` command

### Using Wifi
* `t2 wifi` show details about an existing WiFi connection
  * `[-l]` lists the available networks
  * `[-n SSID]` required, connects to the provided SSID
  * `[-p PASS]` optional, connects with the given password
  * `[-s SECURITY]` connects with the given security type, valid options:
    * `none` open network, no need for a password
    * `wep` WEP network, password required
    * `psk` WPA Personal, password required
    * `psk2` WPA2 Personal, password required
    * `wpa` WPA Enterprise, password required
    * `wpa2` WPA2 Enterprise, password required
  * `[--off]` disconnects from the current network
  * `[--on]` connects to the last configured network

### Create An Access Point
* `t2 ap`
  * `[-n SSID]` required, creates a network with the given ssid
  * `[-p PASS]` optional for open networks, creates a network with the given password
  * `[-s SECURITY]`  creates a network with the given security, valid options:
    * `none` open network, default if no password is given
    * `wep` WEP network, password required
    * `psk` WPA Personal, password required
    * `psk2` WPA2 Personal, reccomended, password required & default if password is given without security
  * `[--trigger on/off]`
    * `on` turn on the most recently used access point
    * `off` turn off the current access point
