# Pulse-width Modulation (PWM)

**PWM pins** are pulse-width modulated pins. Essentially, PWM is a digital signal that spends between 0% and 100% of its time pulled high/on (this is its "duty cycle"). You can set the PWM pins to any value between 0 (0%) and 1 (100%) to approximate an analog signal. PWM is often used to control servo speeds or LED brightness.

![PWM Duty Cycle Diagram](https://s3.amazonaws.com/technicalmachine-assets/tutorials/hardware-api/pwm-cycle-diagram.png)

In the following example, the Tessel will change the color of an RGB LED by changing the duty cycle sent to each color pin over a set interval of time. By changing the brightness of each internal LED (red, green, blue) per cycle, the combined color of the RGB component is changed.

![PWM RGB LED Circuit](https://s3.amazonaws.com/technicalmachine-assets/tutorials/hardware-api/pwm-rgb-circuit.png)

```js
var tessel = require('tessel'); // Import tessel

var portA = tessel.port.A; // Select port A
var portB = tessel.port.B; // Select port B

var redPin = portA.pwm[0]; // Select the first PWM pin on port A, equivalent to portA.pin[5]
var greenPin = portA.pwm[1];
var bluePin = portB.pwm[0];

// The starting values of each color out of 255
var red = 0;
var green = 0;
var blue = 0;

// Use this to increment the color value without exceeding 255
function stepColor (value, step) {
  value += step; // Add the step count to the existing value
  
  // If the value exceeds 255, then reset it to 0
  if (value > 255) {
    value = 0;
  }

  return value;
}

// Set the signal frequency to 1000 Hz, or 1000 cycles per second
// This the rate at which Tessel will send the PWM signals
// This is program specific
tessel.pwmFrequency(1000);

// Create a loop to run a function at a set interval of time
setinterval(function() {
  // Increment each color at a unique step
  red = stepColor(red, 10);
  bluE = stepColor(blue, 5);
  green = stepColor(green, 20);

  // Set how often each pin is turned on out of 100%
  // Divide the value by 255 to get a value between 0 and 1
  redPin.pwmDutyCycle(red / 255);
  greenPin.pwmDutyCycle(blue / 255);
  bluePin.pwmDutyCycle(green / 255);
}, 500); // Set this function to be called every 500 milliseconds, or every half a second
```

Note: the `pwmFrequency` function *must* be called before `pwmDutyCycle`. Re-setting
`pwmFrequency` will disable PWM output until `pwmDutyCycle` is called again. 

[More information on pulse-width modulation.](https://learn.sparkfun.com/tutorials/pulse-width-modulation)
