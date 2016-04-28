# Tessel 2 Hardware API

* [Ports and pins](#ports-and-pins)
  * [Modules](#modules)
  * [Pin mapping](#pin-mapping)
  * [Digital pins](#digital-pins)
  * [Analog pins](#analog-pins)
  * [PWM pins](#pwm-pins)
  * [SPI](#spi)
  * [I2C](#i2c)
  * [UART/Serial](#uart-serial)
* [Button and LEDs](#button-and-leds)
* [USB ports](#usb-ports)

When you `require('tessel')` within a script which is executed on Tessel 2, this loads a library which interfaces with the Tessel 2 hardware, including pins, ports, and LEDs, just like Tessel 1 ([Tessel 1 hardware documentation](https://github.com/tessel/t1-docs/blob/master/hardware-api.md)). The code for Tessel 2's hardware object can be found [here](https://github.com/tessel/t2-firmware/blob/master/node/tessel.js).

## Ports and pins

Tessel has two ports, A and B. They are referred to as `tessel.port.B`. `tessel.port['B']` is also an acceptable reference style.

Tessel's ports can be used as module ports as in Tessel 1 (e.g. `accelerometer.use(tessel.port.B)`), or used as flexible GPIO pins (e.g. `myPin = tessel.port.A.pin[0]`).

### Modules

Tessel 2's module ports can be used with [Tessel modules](//tessel.io/modules) much as in [Tessel 1](http://start.tessel.io/modules).

Here is an example of using the Tessel Climate module on Tessel's port B:

```js
var tessel = require('tessel');
var climatelib = require('climate-si7020').use(tessel.port.B);
```

### Pin mapping

The module ports are not just for modules! They can also be used as flexible, simply addressable General purpose input-output (GPIO) pins. GPIO pins provide access for digital and analog signal lines. Available pins are exposed through the .pin, .digital, .analog, and .pwm arrays.

The pin capabilities for ports A and B are as follows:

| Port | Pin | Digital I/O | SCL | SDA | SCK | MISO | MOSI | TX | RX | Analog In | Analog Out | Interrupt | PWM |
|------|-----|-------------|-----|-----|-----|------|------|----|----|-----------|------------|-----------|-----|
| A    | 0   | ✓           | ✓   |     |     |      |      |    |    |           |            |           |     |
| A    | 1   | ✓           |     | ✓   |     |      |      |    |    |           |            |           |     |
| A    | 2   | ✓           |     |     | ✓   |      |      |    |    |           |            | ✓         |     |
| A    | 3   | ✓           |     |     |     | ✓    |      |    |    |           |            |           |     |
| A    | 4   | ✓           |     |     |     |      | ✓    |    |    | ✓         |            |           |     |
| A    | 5   | ✓           |     |     |     |      |      | ✓  |    |           |            | ✓         | ✓   |
| A    | 6   | ✓           |     |     |     |      |      |    | ✓  |           |            | ✓         | ✓   |
| A    | 7   | ✓           |     |     |     |      |      |    |    | ✓         |            | ✓         |     |
| B    | 0   | ✓           | ✓   |     |     |      |      |    |    | ✓         |            |           |     |
| B    | 1   | ✓           |     | ✓   |     |      |      |    |    | ✓         |            |           |     |
| B    | 2   | ✓           |     |     | ✓   |      |      |    |    | ✓         |            | ✓         |     |
| B    | 3   | ✓           |     |     |     | ✓    |      |    |    | ✓         |            |           |     |
| B    | 4   | ✓           |     |     |     |      | ✓    |    |    | ✓         |            |           |     |
| B    | 5   | ✓           |     |     |     |      |      | ✓  |    | ✓         |            | ✓         | ✓   |
| B    | 6   | ✓           |     |     |     |      |      |    | ✓  | ✓         |            | ✓         | ✓   |
| B    | 7   | ✓           |     |     |     |      |      |    |    | ✓         | ✓          | ✓         |     |

If you're newer to hardware and these functions look like alphabet soup to you, take a look at our [communication protocols documentation](https://tessel.io/docs/communicationProtocols) to get an idea of how these pins should be used.

By default, all of the pins are pulled high if not specifically set.

### Digital pins

A digital pin (any pin other than 3.3V and GND on Tessel 2) is either high (on/3.3V) or low (off/0V). On both of ports A and B, pins 0 and 1 are pulled high to 3.3V by default.

Here is an example usage of a digital pin on Tessel:

```js
var tessel = require('tessel'); // import tessel
var pin = tessel.port.A.pin[2]; // select pin 2 on port A
pin.output(1);  // turn pin high (on)
pin.read(function(error, value) {
  // print the pin value to the console
  console.log(value);
  pin.output(0);  // turn pin low (off)
});
```

### Analog pins

An analog pin is a pin whose value can vary in the range between 0V and 3.3V. Pins 4 and 7 on port A and all pins on port B can read analog values (though pins 0 and 1 are pulled to 3.3V by default and are thus not recommended for this purpose). Pin 7 on port B can write an analog value.

Here is an example usage of an analog pin on Tessel:

```js
var tessel = require('tessel'); // import tessel
var pin = tessel.port.B.pin[7]; // select pin 7 on port B
pin.analogWrite(0.6);  // turn pin to 60% of high
pin.analogRead(function(error, value) {
  // print the pin value to the console
  console.log(value);
});
```

### PWM pins

**PWM pins** are pulse-width modulated pins ([wiki link](http://en.wikipedia.org/wiki/Pulse-width_modulation)). Essentially, PWM is a digital signal that spends between 0% and 100% of its time pulled high/on (this is its "duty cycle"). You can set the PWM pins to any value between 0 and 1 to approximate an analog signal. PWM is often used to control servo speeds or LED brightness.

Tessel has four PWM pins: pins 5 and 6 on each module port.

Here is an example of setting a PWM pin:
```js
var tessel = require('tessel'); // Import tessel
var port = tessel.port.A; // Select one of the two ports
var myPin = port.pwm[0]; // Equivalent to port.digital[0]
// Tell the Tessel that the frequency of its pwm pins is 50 Hz (application specific)
tessel.pwmFrequency(980);
// Set duty cycle
myPin.pwmDutyCycle(0.6); // set the pin to be on 60% of the time
```

Note: the `pwmFrequency` function *must* be called before `pwmDutyCycle`. Re-setting
`pwmFrequency` will disable PWM output until `pwmDutyCycle` is called again. `pwmFrequency` is capable of frequencies from 1Hz to 5kHz.

### I2C

An I2C channel uses the SCL and SDA pins (0 and 1 on Tessel 2). If you are unfamiliar with the I2C protocol, please see the [communication protocols tutorial](https://tessel.io/docs/communicationProtocols#i2c).

Here is an example using Tessel's I2C protocol:

```js
var port = tessel.port.A;
var replicaAddress = 0xDE;
var i2c = new port.I2C(replicaAddress)
i2c.transfer(new Buffer([0xde, 0xad, 0xbe, 0xef]), function (err, rx) {
  console.log('buffer returned by I2C replica ('+replicaAddress.toString(16)+'):', rx);
})
```

### SPI

A SPI channel uses the SCK, MISO, and MOSI pins (2, 3, and 4 on Tessel 2). If you are unfamiliar with the SPI protocol, please see the [communication protocols tutorial](https://tessel.io/docs/communicationProtocols#spi).

Here is an example using Tessel's SPI protocol:

```js
var port = tessel.port.A;
var spi = new port.SPI({
  clockSpeed: 4*1000*1000, // 4MHz
  cpol: 1, // polarity
  cpha: 0, // clock phase
});

spi.transfer(new Buffer([0xde, 0xad, 0xbe, 0xef]), function (err, rx) {
  console.log('buffer returned by SPI replica:', rx);
});
```

### UART/Serial

A UART (serial) channel uses the TX and RX pins (5 and 6 on Tessel 2). If you are unfamiliar with the UART protocol, please see the [communication protocols tutorial](https://tessel.io/docs/communicationProtocols#uart).

Here is an example using Tessel's UART protocol:

```js
var port = tessel.port.A;
var uart = new port.UART({
  baudrate: 115200
});

uart.write('ahoy hoy\n')
uart.on('data', function (data) {
  console.log('received:', data);
})

// UART objects are streams!
// pipe all incoming data to stdout:
uart.pipe(process.stdout);
```

## Button and LEDs
There are 4 LEDs available on the Tessel 2, you may see them labeled on the board as `ERR`, `WLAN`, `LED0`, and `LED1`. They are available through the `tessel.led` object.

```js
// an array of available LEDs
var leds = tessel.led;

// ERR - Red
var red = leds[0];

// WLAN - Amber
var amber = leds[1];

// LED0 - Green
var green = leds[2];

// LED1 - Blue
var blue = leds[3];
```

Each LED has a few methods for controlling it.


```js
// Green LED
var led = tessel.led[2];

/*
* Property: isOn
* Returns: boolean (true or false) if led is on
*
* Checks the led to see if it is on or not.
*/
if (led.isOn) {
  console.log('The green LED is currently on.');
}

/*
* Method: on
* Returns: the led object (this makes it a chainable function)
*
* Turns the led ON.
*/
led.on();

/*
* Method: off
* Returns: the led object (this makes it a chainable function)
*
* Turns the led OFF.
*/
led.off();

/*
* Method: toggle
* Arguments:
* - callback: function to call once the led's value has been set and is passed an error object if one occured
*
* Toggles the current state of the led and calls the callback function once that is done.
*/
led.toggle(function (err) {
  if (err) {
    console.log('There was an error with toggling the led.', err);
  } else {
    console.log('The green led is now on OR off!');
  }
});
```

Tessel 2's button is not yet exposed in the API – but you can change that! See [#15](https://github.com/tessel/t2-firmware/issues/107) for a description of what needs to be done.

## USB Ports

USB modules do not need to be accessed through the Tessel object. See [node-audiovideo](https://github.com/tessel/node-audiovideo) for an example USB module.
