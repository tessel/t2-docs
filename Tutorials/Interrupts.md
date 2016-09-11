# Interrupt Pins

Interrupts allow us to register events based on state changes in a pin. Pins
2, 5, 6, 7 on both Ports are available for interrupts.

Take a look at the following circuit. While the switch is open, pin2 will be
low. When the button is pressed we'll complete the circuit and the voltage at
pin2 will rise to 3.3V.

![Interrupt Circuit](https://s3.amazonaws.com/technicalmachine-assets/tutorials/hardware-api/interrupt-circuit.png)

Let's turn on the green LED when the button is pressed. Here's one way of doing
it.

```js
var tessel = require('tessel'); // Import tessel

// Initialize our pins
var green = tessel.led[2];
var pin2 = tessel.port.A.pin[2];

// Turn off the green LED
green.off();

// Every 10 milliseconds check to see if the button was pressed
setInterval(function() {
  pin2.read(function(error, value) {

    // button was pressed
    if (value === 1) {
      green.on();
    }
  });
}, 10);

```

This is pretty inefficient. We're spending a lot of time (every 10 millseconds)
manually checking to see if the button was pressed. Let's just tell the
processor to run some code when the voltage rises.

```js
var tessel = require('tessel'); // Import tessel

// Initialize our pins
var green = tessel.led[2];
var pin2 = tessel.port.A.pin[2];

// Turn off the green LED
green.off();

// Register an event. When the voltage on pin2 rises, turn on the green LED.
pin2.on('rise', function() {
  green.on();
});

```

We tell the processor to invoke our callback function when pin2 rises (button
has been pressed). We don't have to continuously poll to see if the voltage
has risen. We can just register our listener and move on.

[More information on interrupts.](https://www.sparkfun.com/tutorials/326)
