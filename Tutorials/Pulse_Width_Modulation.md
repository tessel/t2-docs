# Pulse-width Modulation (PWM)

**PWM pins** are pulse-width modulated pins. Essentially, PWM is a digital signal that spends between 0% and 100% of its time pulled high/on (this is its "duty cycle"). You can set the PWM pins to any value between 0 (0%) and 1 (100%) to approximate an analog signal. PWM is often used to control servo speeds or LED brightness.

![PWM Duty Cycle Diagram](https://s3.amazonaws.com/technicalmachine-assets/tutorials/hardware-api/pwm-cycle-diagram.png)

In the following example, the Tessel will change the color of an RGB LED by changing the duty cycle sent to each color pin over a set interval of time. By changing the brightness of each internal LED (red, green, blue) per cycle, the combined color of the RGB component is changed.

![PWM RGB LED Circuit](https://s3.amazonaws.com/technicalmachine-assets/tutorials/hardware-api/pwm-rgb-circuit.png)



```js
const tessel = require('tessel');

// Unpack and name pwm pins from port A
const [ red, green ] = tessel.port.A.pwm;
// Unpack and name pwm pin from port B
const [ blue ] = tessel.port.B.pwm;

// The starting values of each color out of 255
red.value = 0;
green.value = 0;
blue.value = 0;

// Use this to increment the color value without exceeding 255
function stepColor(led, step) {
  led.value += step; // Add the step count to the existing value

  // If the value exceeds 255, then reset it to 0
  if (led.value > 255) {
    led.value = 0;
  }

  // return a fractional value
  return led.value / 255;
}

// Set the signal frequency to 1000 Hz, or 1000 cycles per second
// This the rate at which Tessel will send PWM signals
// This is program specific
tessel.pwmFrequency(1000);

// Create an interval to run a function which sets the color of all three LEDs
setInterval(() => {
  // Increment each LED color at a unique step
  red.pwmDutyCycle(stepColor(red, 10));
  green.pwmDutyCycle(stepColor(green, 5));
  blue.pwmDutyCycle(stepColor(blue, 20));
}, 500); // Set this function to be called every 500 milliseconds, or every half a second
```

Note: the `pwmFrequency` function *must* be called before `pwmDutyCycle`. Re-setting
`pwmFrequency` will disable PWM output until `pwmDutyCycle` is called again. 

[More information on pulse-width modulation.](https://learn.sparkfun.com/tutorials/pulse-width-modulation)

--------------------------------------------------------------

If you're Tessel 2 is up-to-date, it will be be running Node.js 6.10.3 or newer. You can check that by running `t2 version` and comparing the output to the following: 

```
$ t2 version
INFO Looking for your Tessel...
INFO Connected to maria.
INFO Tessel Environment Versions:
INFO t2-cli: 0.1.5
INFO t2-firmware: 0.1.0
INFO Node.js: 6.10.3
```

If your output doesn't match, you should run `t2 update` to get the latest OS and firmware. Once that's done, you're ready to run the program in this tutorial.


