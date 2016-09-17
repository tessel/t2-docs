# Hardware Overview of Tessel 2

## Quick specs

At a glance, the main hardware components of Tessel 2 are:

*  580MHz WiFi router system on chip ([Mediatek MT7620n](http://www.anz.ru/files/mediatek/MT7620_Datasheet.pdf)) running linux ([OpenWRT](https://openwrt.org/))
* 64 MB of DDR2 RAM
* 32 MB of flash storage
* 2 High-speed USB 2.0 ports
* micro USB port
* 10/100 Ethernet port (RJ-45 jack)
* 48MHz ARM Cortex M0 microcontroller ([Atmel SAMD21](http://www.atmel.com/Images/Atmel-42181-SAM-D21_Datasheet.pdf))
* Two flexible module ports
* 4 LEDs
* Reset button

Above-and-beyond features:

* Router-grade 802.11b/g/n WiFi (2.4 GHz), including access point mode ([Tessel can be a router](http://tessel.github.io/t2-start/ap.html))
* 16 GPIO broken out as a pair of multi-purpose module ports
* Individual control over and protection for all outward-facing power buses (USB and module ports)
* A form factor designed for abstraction and flexibility in the hardware, software, and mechanical worlds as you scale from prototype to production

## Tessel 2 Layout

On the board, Tessel 2 is divided into three functional blocks:

![](http://67.media.tumblr.com/83018b7f2595c1f4b3877f74eb86d134/tumblr_inline_nkz56hKzbm1s75tgz.png)

These can be abstracted as follows:

![](http://65.media.tumblr.com/559c4d45074644a5c335987bec7c698e/tumblr_inline_nkz56bL5SC1s75tgz.png)

### Abstraction boundaries of Tessel 2

The board employs a processor/coprocessor architecture. The Mediatek runs your user code, hosts USB devices, handles the network connections (be they wired, wireless, or cellular over USB), and communicates with the SAMD21.

The SAM acts as a coprocessor and handles real-time, low-level IO through the module ports, USB comms through the Micro USB port, and programming the device as a whole.

The two chips are connected by a SPI bridge that also includes the onboard flash (the [readme for Tessel 2’s firmware repo goes into more detail here](https://github.com/tessel/v2-firmware/blob/master/README.md)).

The whole system is powered from the single micro USB (device) port. Alternately, power can be supplied over the 5VIN pin:

![](https://tessel-discourse.s3.amazonaws.com/448470e28b84ce3e1fcfead5acdf82b039fb563b8cb6_479x500.png)

The additional 5V pin shown in the yellow box is a source of 5V power.

## Power architecture of Tessel 2

The below table explains how the major components of Tessel 2 are powered.

Component          | Powered by       | Enabled or reset by
-------------------|------------------|-------------------------------------------------
5V                 | USB cable        | Presence of USB cable with upstream power
3.3V Mediatek      | 5V               | GPIO on coprocessor, pull resistor to ENABLE
Mediatek chip      | 3.3V Mediatek    | GPIO on coprocessor, pull resistor to !RESET
Flash chip         | 3.3V Mediatek    | GPIO on both the SAM and the Mediatek
1.8V (not exposed) | 5V               | 3.3V Mediatek
RAM for Mediatek   | 1.8V             | Mediatek (involuntary)
3.3V coprocessor   | 5V               | 5V
Coprocessor chip   | 3.3V Coprocessor | GPIO on Mediatek, pull resistor to !RESET
3.3V module port A | 3.3V Coprocessor | GPIO on coprocessor, pull resistor to OFF
3.3V module port B | 3.3V Coprocessor | GPIO on coprocessor, pull resistor to OFF
USB hub IC         | 5V               | 3.3V Mediatek
Power to USB ports | 5V               | Mediatek via USB hub IC, pull resistor to off

What this means in practice is that the coprocessor (SAMD21) has control over both the "power plug" and the reset switch for the Mediatek, while the Mediatek only has control over the SAM's reset line. The rationale here is that the SAM uses a negligible amount of power, but it might be beneficial to be able to shut down the Mediatek in order to save power.

That said, there is no true "off switch" in the hardware that enables a permanent shutdown so long as external power is applied. Quiescent draw is currently estimated to be <10mW.

Triggering a Mediatek reset from the SAM results in a clean reboot. I believe the SAM does currently save some state, but it can be reset by the Mediatek, so this isn't really a concern.

Because the flash must be on in order for the Mediatek to boot (that's where [our build of OpenWRT](https://github.com/tessel/openwrt-tessel) lives), it is powered by the 3.3V Mediatek bus. In order for the SAM to program the flash, it must hold the Mediatek in reset. Note that this is most useful for programming the board the first time...and other custom, low-level work, like what you would need to write in order to successfully save state so you could power down the Mediatek and bring it back up again elegantly. The protocol and architecture involved between the Mediatek and the coprocessor is explained more [in the T2 Firmware readme](https://github.com/tessel/t2-firmware).

### Port power management

Various pieces of the Tessel 2 have hardware support for individual power management.

#### Module port power
Tessel 2's architecture gives you individual control over the 3.3V (at least 250 mA) rails on each port, so you can turn modules off when they’re not in use to save power.

Tessel 2's firmware turns off power to the port if a program does not require the `tessel` module. However, if `tessel` is required in the code, both will be turned on by default. It is possible to turn off the other module port with an explicit call, detailed [here](../API/Hardware_API.html#power-management).

### USB ports
Though not currently broken out into the user layer, it is possible to control power to each USB port via SET_FEATURE(USB_PORT_FEAT_POWER) requests.

## Module port goodies

Each pin on the two 10-pin headers is unique and can be reconfigured to do almost anything from speaking alternate comms protocols to clock generation. For example, if you decide you don’t want SPI, feel free to give yourself another I2C or UART with minimal changes to the SAMD21’s firmware. Touching only JS, you can forgo the fancy comms in favor of just plain GPIO, which gives you access to as many as 16 of them.

Last but not least, all eight pins on Port B are also inputs to a 12-bit, 350[ksps](http://www.maximintegrated.com/en/glossary/definitions.mvp/term/ksps/gpk/573) ADC, with adjustable gain that can operate in differential mode.

More information on the module ports can be found in the [Atmel SAMD21 datasheet](http://www.atmel.com/Images/Atmel-42181-SAM-D21_Datasheet.pdf), such as this chart of I/O pin characteristics (p962):


![](https://cloud.githubusercontent.com/assets/454690/18562152/9215264c-7b50-11e6-9666-e00047d104d6.png)
![](https://cloud.githubusercontent.com/assets/454690/18562081/5343d92c-7b50-11e6-8473-9aba7ad127f3.png)
