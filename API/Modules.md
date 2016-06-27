# Tessel 2 Modules

Tessel uses a system of modules to add functionalities to the ecosystem.

A Tessel module consists of some piece of hardware, matched with a software package which explains and provides API access to the hardware.

There are three types of modules:

* Tenpin: purchaseable built-for Tessel modules following the 10-pin module format
* USB: any USB device with an API Tessel can use
* Community: any piece of hardware with an API Tessel can use, with instructions on how to connect the hardware to Tessel

The best place to find API documentation for a Tessel module is through the links on the [modules page](//tessel.io/modules).

## Quick links

* [See all the modules and find their API documentation](https://tessel.io/modules)
* [Learn how to use modules on the start page](http://tessel.github.io/t2-start/)
* [Metadata about tenpin, USB, and community modules, including compatibility](https://github.com/tessel/hardware-modules)
* [Hardware docs for Tessel's first-party, tenpin modules](https://github.com/tessel/hardware/blob/master/modules-overview.md)
* [How to make a module](/Tutorials/Making_Your_Own_Module.html)
* [Communication protocols 101](/Tutorials/Communication_Protocols.html)

## Module design philosophy

Modules should be devices with clear-cut functionality. That is to say, they should have a single, well-defined purpose or a set of closely related functions, rather than an eclectic mix of capabilities onboard. This requirement is designed to reduce complexity, cost, and power consumption and maximize reusability in hardware and software.
