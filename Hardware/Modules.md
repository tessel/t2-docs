# Hardware documentation for Tessel modules

## Scope

This document provides hardware overviews for each Tessel module:

*  [Accelerometer](#accelerometer)
*  [Ambient](#ambient)
*  [Climate](#climate)
*  [GPS](#gps)
*  [Infrared](#infrared)
*  [Relay](#relay)
*  [RFID](#rfid)
*  [Servo](#servo)

## Accelerometer

Measures acceleration in the x, y, and z directions.

### Quick overview:

Parameter                       | Value
--------------------------------|------
Tessel part #                   | TM-01-XX
Latest version                  | TM-01-02
Key components                  | [MMA8452Q](http://www.freescale.com/files/sensors/doc/data_sheet/MMA8452Q.pdf)
Current consumption (rated max) | 165 microamps
Current consumption (average)   | 165 microamps
Communication protocol          | I2C, GPIO

### Notes
*  I2C address can be modified by shorting J3 – see [MMA8452Q datasheet](http://www.nxp.com/files/sensors/doc/data_sheet/MMA8451Q.pdf) 5.11.1 Table 10.
*  +x = towards the Tessel, +y = G3 --> GND, +z = up

### Links to Hardware Information:

*  [Code: firmware, docs, examples](https://github.com/tessel/accel-mma84)
*  [Schematic(PDF)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Accelerometer/TM-01-02.pdf)
*  [Schematic (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Accelerometer/TM-01-02.dch)
*  [Layout (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Accelerometer/TM-01-02.dip)
*  [Layout support file for silkscreen](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Accelerometer/TM-01-02.dipb2)

## Ambient

Measures ambient light and sound. Sound gain adjustable via R6.

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-08-XX
Latest version | TM-08-03
Key components | [PT15-21C/TR8](http://www.everlight.com/file/ProductFile/PT15-21C-TR8.pdf) photodiode, [SPU0410HR5H-PB](http://ke-www2.knowles.com/search/prods_pdf/SPU0410HR5H.pdf) microphone
Current consumption (rated max) | 10 mA
Current consumption (average) | 10 mA
Communication protocol | SPI, GPIO

### Notes

*  Light signal path includes low pass filters with *f<sub>c</sub>* = 1.6 Hz
*  Sound signal path measures loudness via a half wave rectifier

### Links to Hardware Information:

*  [Code: firmware, docs, examples](https://github.com/tessel/ambient-attx4)
*  [Schematic(PDF)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Ambient/TM-08-03.pdf)
*  [Schematic (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Ambient/TM-08-03.dch)
*  [Layout (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Ambient/TM-08-03.dip)
*  [Layout support file for silkscreen](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Ambient/TM-08-03.dipb2)

## Climate

Measure temperature and humidity

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-02-XX
Latest version | TM-02-02
Key components | [SI7020-A20](http://www.silabs.com/Support%20Documents/TechnicalDocs/Si7020-A20.pdf) sensor
Current consumption (rated max) | 565 microamps
Current consumption (average) | 320 microamps
Communication protocol | I2C, GPIO

### Notes

Temperature of the climate module can be affected by proximity to the Tessel processor.

### Links to Hardware Information:

*  [Code: firmware, docs, examples](https://github.com/tessel/climate-si7005)
*  [Schematic(PDF)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Climate/TM-02-02.pdf)
*  [Schematic (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Climate/TM-02-02.dch)
*  [Layout (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Climate/TM-02-02.dip)
*  [Layout support file for silkscreen](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Climate/TM-02-02.dipb1)

## GPS

Global positioning system: position reference, movement speed & heading, time reference

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-09-XX
Latest version | TM-09-02
Key components | A2235-H GPS module ([user manual](http://www.maestro-wireless.com/download-dp40), [product page](http://www.maestro-wireless.com/a2235-h))
Current consumption (rated max) | 42 mA
Current consumption (average) | 36 mA
Communication protocol | UART, GPIO

### Notes

*  The A2235-H is based on CSR SiRFstarIV chipset, which supports a novel GPS fix mode and a variety of low power states.
*  The A2235-H supports two different data modes: NMEA and SiRF binary mode (also called OSP mode). Tessel GPS's default APIs use the module in NMEA mode.
*  A-GPS is supported with client generated extended ephemeris up to 3 days.

### Links to Hardware Information:

*  [Code: firmware, docs, examples](https://github.com/tessel/gps-a2235h)
*  [Schematic(PDF)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/GPS/TM-09-02.pdf)
*  [Schematic (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/GPS/TM-09-02.dch)
*  [Layout (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/GPS/TM-09-02.dip)
*  [Layout support file for silkscreen](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/GPS/TM-09-02.dipb0)

## Infrared

Communicate with and command household electronics using IR light

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-11-XX
Latest version | TM-11-03
Key components | [TSOP38238](http://www.vishay.com/docs/82491/tsop382.pdf) 38 kHz IR demodulator, [SFH-4646-Z](http://www.osram-os.com/Graphics/XPic6/00116152_0.pdf) IR LED
Current consumption (rated max) | 95 mA
Current consumption (average) | 5 mA
Communication protocol | UART, GPIO

### Notes

*  Receiving IR takes very little power. Transmission, on the other hand, uses a good deal of power.
*  Receiver frequency is fixed at 38 kHz, although pin-compatible parts which operate at other frequencies presumably exist. Check your appliance's manual and/or the internet to make sure your hardware is compatible.

### Links to Hardware Information:

*  [Code: firmware, docs, examples](https://github.com/tessel/ir-attx4)
*  [Schematic(PDF)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/IR/TM-11-03.pdf)
*  [Schematic (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/IR/TM-11-03.dch)
*  [Layout (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/IR/TM-11-03.dip)
*  [Layout support file for silkscreen](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/IR/TM-11-03.dipb2)

## Relay

Switch large, high voltage loads

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-04-XX
Latest version | TM-04-04
Key components | IM02DGR ([DigiKey page](http://www.digikey.com/product-detail/en/IM02DGR/PB1171CT-ND/1828461))
Current consumption (rated max) | 90 mA
Current consumption (average) | 10 mA
Communication protocol | GPIO

### Notes

*  Relays are normally open (load disconnected) and will open should the module/Tessel lose power
*  Current draw increases when the relays close (*I<sub>draw</sub>* = 10mA + 40mA * [number of relays on])
*  Rated to 240 V, 5 A (but please be careful)
*  Insert and remove wires by pressing down on the connectors with a ballpoint pen (or similar).

### Links to Hardware Information:

*  [Code: firmware, docs, examples](https://github.com/tessel/relay-mono)
*  [Schematic(PDF)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Relay/TM-04-04.pdf)
*  [Schematic (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Relay/TM-04-04.dch)
*  [Layout (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Relay/TM-04-04.dip)
*  [Layout support file for silkscreen](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Relay/TM-04-04.dipb2)

## RFID

Read and write from/to 13.56 MHz RFID cards and tags

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-10-XX
Latest version | TM-10-03
Key components | PN532 ([datasheet](http://www.adafruit.com/datasheets/pn532longds.pdf), [user manual](http://www.nxp.com/documents/user_manual/141520.pdf), [MiFARE tag IC/protocol docs](http://www.nxp.com/documents/data_sheet/MF1S503x.pdf)) 13.56 MHz RFID transceiver
Current consumption (rated max) | 150 mA
Current consumption (average) | 60 mA
Communication protocol | I2C, GPIO

### Notes

*  Module can be switched to communicate over SPI with appropriate resistor pop option swaps
*  Current is highest when transmitting and much lower most of the time
*  A MiFare Classic card ships with the module
*  Hardware-compatible with [ISO-14443 RFID tags](http://nfc-tools.org/index.php?title=ISO14443A), including but not limited to: MiFare (Classic 1k, Classic 4k, Ultralight), DesFire, and FeliCa. Further information in the link.
*  Compatible cards (consumer): Charlie, Clipper

### Links to Hardware Information:

*  [Code: firmware, docs, examples](https://github.com/tessel/rfid-pn532)
*  [Schematic(PDF)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/RFID/TM-10-03.pdf)
*  [Schematic (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/RFID/TM-10-03.dch)
*  [Layout (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/RFID/TM-10-03.dip)
*  [Layout support file for silkscreen](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/RFID/TM-10-03.dipb0)

## Servo

Control up to 16 servos

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-03-XX
Latest version | TM-03-02
Key components | [PCA9685](http://www.adafruit.com/datasheets/PCA9685.pdf) LED driver
Current consumption (rated max) | 50 mA
Current consumption (average) | 10 mA
Communication protocol | I2C, GPIO

### Notes

*  An I2C LED PWM driver makes for a great servo driver too!
*  16 channels
*  Tessel is a 3.3V device, so *V<sub>out</sub>* on the PWM channels is 3.3 V (works for standard hobby servos)
*  5.5 mm barrel jack (center positive) for powering servos
*  Can be used to control anything that needs PWM (speed controllers, gate drivers-to-FETs-big LEDs, etc.)

### Links to Hardware Information:

*  [Code: firmware, docs, examples](https://github.com/tessel/servo-pca9685)
*  [Schematic(PDF)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Servo/TM-03-03.pdf)
*  [Schematic (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Servo/TM-03-03.dch)
*  [Layout (DipTrace)](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Servo/TM-03-03.dip)
*  [Layout support file for silkscreen](http://design-files.tessel.io.s3.amazonaws.com/2014.06.06/Modules/Servo/TM-03-03.dipb0)
