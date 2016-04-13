# Tessel 2 Command Line Interface

* [Installation](#installation)
* [Updating Tessel](#updating-tessel-2-on-board-osfirmware)
* [Setup](#setup)
* [Usage/CLI commands](#usage)

## Installation
Prerequisites for installation: [Node.js](https://nodejs.org/) 4.2.x or greater

`npm install -g t2-cli`

The best place to go next is [the Tessel 2 start experience](http://tessel.io/t2-cli), which will walk you through a tutorial.

## Updating Tessel 2 On-board OS/Firmware

### How do I know if I need to update my T2?
The easiest way is to run `t2 update` and it will update automatically if there is a newer firmware version available. Otherwise, you can run `t2 version` to get the version running on your Tessel, and then `t2 update -l` to see the 10 newest versions available.

### Updating
Simply run `t2 update`. If you want to update to a specific version, run `t2 update -v VERSION_NUM` where `VERSION_NUM` is one of the versions returned by `t2 update -l` (like `t2 update -v 0.0.6`).

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
* `t2 init` in the current directory, create a package.json and index.js with Hello World code.
* 
### Project Files
Along with the package.json and index.js included in the `t2 init` process, there are some other files that may be useful for your project:
* `.tesselignore` similar to .gitignore or [.npmignore](https://docs.npmjs.com/misc/developers#keeping-files-out-of-your-package), this file should list any files or directories you want ignored by the T2 bundling and deployment process. This is handy when using the `--full` flag, which tells T2 to bundle everything in the project directory.
* `.tesselinclude` the overriding and opposite behavior of `.tesselignore`, this file should list any files or directories you want included with the T2 bundling and deployment process. This is handy for including non-JavaScript assets, like HTML, CSS, and images, for use within your project.

### Tessel Management
* `t2 provision` authorize your computer to access a Tessel over SSH (USB-connected Tessel only)
* `t2 list` show what Tessels are available over WiFi and USB.
* `t2 rename` change the name of a Tessel

### Code Deployment 
During code deployment, CLI looks for `.tesselignore` and `.tesselinclude` files to let it know which files it should bundle up and push over to Tessel. In the default bundling process, CLI takes the file passed into the `run` or `push` commands and finds all its dependencies by following the 'require' statements (we use [Browserify](https://github.com/substack/browserify-handbook#how-browserify-works) to do this).
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
