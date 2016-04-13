# Tessel 2 Modules

Tessel uses a system of modules to add functionalities to the ecosystem.

A Tessel module consists of some piece of hardware, matched with a software package which explains and provides API access to the hardware.

There are three types of modules:

* Tenpin: purchaseable built-for Tessel modules following the 10-pin module format
* USB: any USB device with an API Tessel can use
* Community: any piece of hardware with an API Tessel can use, with instructions on how to connect the hardware to Tessel

## Quick links

* [See all the modules](//tessel.io/modules)
* [Learn how to use modules on the start page](//start.tessel.io/modules)
* [Metadata about tenpin, USB, and community modules, including compatibility](https://github.com/tessel/hardware-modules)
* [Hardware docs for Tessel's first-party tenpin modules](https://github.com/tessel/hardware/blob/master/modules-overview.md)
* [How to make a module](//tessel.io/diy)
* [Communication protocols 101](https://tessel.io/docs/communicationProtocols)

## Module design philosophy

Modules should be devices with clear-cut functionality. That is to say, they should have a single, well-defined purpose or a set of closely related functions, rather than an eclectic mix of capabilities onboard. This requirement is designed to reduce complexity, cost, and power consumption and maximize reusability in hardware and software.
