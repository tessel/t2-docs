## access point
Networking hardware which creates a local area network over Wifi

## analog
In an electrical signal, existing in a continuous range, as opposed to the discrete ranges of digital. [Sparkfun has a good writeup on analog versus digital](https://learn.sparkfun.com/tutorials/analog-vs-digital) if you want to learn more.

## API
Application program interface: a set of tools and protocols which can be used to build software. In the case of Tessel, the hardware API is a library you can use to connect to modules, manipulate LEDs, and call out specific pins

## CLI
Command line interface: a tool which lets you interact with a program (or in the case of Tessel, with the Tessel) via text-based commands in the command line, or terminal

## communication protocols
Defined ways of communicating, e.g. a map between timed toggling of digital signals and hexadecimal values

## digital
In an electrical signal, existing at discrete/binary values: on or off, zero or one. This can be used simply, e.g. on/off states of a switch or light, or you can toggle digital signals very quickly to encode complex messaging with [communication protocols](/Tutorials/Communication_Protocols.html). [Sparkfun has a good writeup on analog versus digital](https://learn.sparkfun.com/tutorials/analog-vs-digital) if you want to learn more.

## GPIO
General purpose input and output: electrical pins which can be configured for various purposes

## I2C
A hardware communication protocol which uses two wires: SCL and SDA. Read more about it in the [Communication Protocols doc](/Tutorials/Communication_Protocols.html)

## interrupt
An electrical signal which tells the processor that something needs immediate attention

## LAN
Local area network

## LED
Light-emitting diode

## master
A machine or device directly controlling another machine or device

## MDNS
Multicast domain name system, a way to map IPs to devices in a small network

## MISO
Master input/slave output: in a SPI interface, the wire which carries commands to the controlling device from the device(s) under control

## MOSI
Master output/slave input: in a SPI interface, the wire which carries commands from the controlling device to the device(s) under control

## module port
A standard interface for connecting wires and modules to Tessel, configured as 10 "pins" or points of electrical connection

## pins
Points of electrical connection with an integrated circuit

## ports
Interfaces for electrical connections

## PWM
Pulse-width modulation: a hardware communication protocol where there is a digital signal fluctuating between values at a set period, and the message is encoded by changing the percentage of that period in which the signal stays at one of the values. This can be used to approximate an analog signal on a pin which only supports digital signaling. A common application is in motor controllers, mapping a servo motor position to the percentage of the time a signal is held high

## RX
Receive: the wire in a UART interface which receives data from the controlled device

## SCK
Serial clock: the clock wire of a SPI interface, which ensures that the communicating hardware components are in sync and therefore able to understand each other

## SCL
Serial clock line: the clock wire of an I2C interface, which ensures that the communicating hardware components are in sync and therefore able to understand each other

## SDA
Serial data line: the data-carrying wire of an I2C interface

## serial
See: UART

## slave
A device or part of a device which is directly controlled by another

## SPI
A hardware communication protocol which uses SCK, MISO, MOSI, and SS/CS (chip select) lines. Learn more in the [Communication Protocols doc](/Tutorials/Communication_Protocols.html)

## T2
Tessel 2

## Tessel module
A piece of hardware built to interface with Tessel's module ports, paired with a software library

## TX
Transmit: the wire in a UART interface which sends data to the controlled device

## UART
Also referred to as serial. A communication protocol using the RX and TX lines. Learn more in the [Communication Protocols doc](/Tutorials/Communication_Protocols.html)
