# Pull- Pins

Suppose a pin is configured as an input. If nothing is connected to the pin and the program tries to read the state of the pin, it would be in a 'floating' state i.e an unknown state. To prevent this, a pull-up or a pull-down state is defined. They are often used in the case of Buttons and Switches.

Pins 2-7 on both the Ports are available for interrupts.

Take a look at the following circuit. The code example given below turns on the Blue LED of the Tessel module when the pushbutton is pressed and turns off the Blue LED when the pushbutton is released.

![Pin-Pull using Push buttons](http://i.imgur.com/OYJZ8Dp.jpg)

```js
var tessel= require('tessel'); // Import Tessel

var pin = tessel.port.A.pin[2]; // Select pin 2 on port A

var pullType = "pullup"; // Set the mode of `pin.pull to pullup`

var led = tessel.led[3]; // Blue LED of Tessel

pin.pull(pullType,(error, buffer) => { // Pin 2 pulled high

  if (error){
    throw error;
  }
});

setInterval({
  pin.read(function(error, number){

      if (error) {
        throw error;
      }

      // Pin 2 reads high when the pushbutton is not pressed since it is pulled up
      console.log(number);
      if (number == 1){
        led.off();
      }

      // Pin 2 reads low when the pushbutton is pressed since its connection with ground gets complete
      else{
        led.on();
      }

  });

}, 500);
```
[More Information on Pull-up pins and resistors](https://learn.sparkfun.com/tutorials/pull-up-resistors)
