# I2C

I2C is a simple-to-use communication protocol which can have more than one master, with the upper bus speed defined, and only two wires with pull-up resistors are needed to connect almost _unlimited_ number of I2C devices.

Each slave device has a unique address. Transfer from and to master device is serial and it is split into 8-bit packets. All these simple requirements make it very simple to implement I2C interface even with cheap microcontrollers that have no special I2C hardware controller. You only need 2 free I/O pins and few simple I2C routines to send and receive commands.

Let us get the accelerometer values using the I2C protocol. We will be using the [The Tessel Accelerometer](https://www.seeedstudio.com/Tessel-Accelerometer-Module-p-2223.html)

[Refer to the datasheet for the accelerometer module to follow along](http://www.nxp.com/docs/en/data-sheet/MMA8452Q.pdf)

![Fritzing Diagram](http://i.imgur.com/zK4U4S3.png)

```js
var tessel = require('tessel');

// Connect to device
var port = tessel.port.A; // Use the SCL/SDA pins of Port A

// This address of the Slave has been taken from https://github.com/tessel/accel-mma84/blob/master/index.js#L15
// It can also be found in Table 10 - I2C device address sequence on page number 17 of the datasheet. 
var slaveAddress = 0x1D; // Specific to device

var i2c = new port.I2C(slaveAddress); // Initialize I2C communication

// Details of I2C read and I2C transfer
var numBytesToRead = 1; // Read back this number of bytes

// Read data over I2C using i2c.read
i2c.read(numBytesToRead, function (error, dataReceived) {
  // Print data received (buffer of hex values)
  console.log('Buffer returned by I2C slave device ('+slaveAddress.toString(16)+'):', dataReceived);

});

// Read/Receive data over I2C using i2c.transfer
// 0x0D is the WHO_AM_I Register which sends back an acknowledgement to the master for starting the communication
// Information about 0x0D can also be found in Table 11 - Register address map on page number 19 of the datasheet.

i2c.transfer(new Buffer([0x0D]), numBytesToRead, function (error, dataReceived) {
    // Print data received (buffer of hex values)
    // The returned buffer from the I2C slave device should be [0x2A] as specified in the register address map on page number 19 of the MMA84 datasheet
  console.log('Buffer returned by I2C slave device ('+slaveAddress.toString(16)+'):', dataReceived);

});

/*

  In the i2c.transfer function, the dataReceived consists of all the 6 bytes of data (which is 48 bits of data) : 2 bytes each for the x, y, and z values.
  This can be seen on page 20 and 21 of the accelerometer datasheet.
  Each of the x, y, and z acceleration values have two registers associated with them for storing the 12 bit long sample.
  The first 8 bits are stored in their respective OUT_MSB registers. These are the Most Significant first 8 bits.
  The next 4 bits are stored in their respective OUT_LSB registers. The remaining 4 bits are occupied
  by 0s. These lower 4 bits (bits 0, 1, 2, and 3 of the OUT_LSB registers) are redundant bits which are not required. The OUT_LSB and OUT_MSB store the 2's complement form of the coordinates.

  The organization of the registers can be seen in the datasheet inside section 6.1 (Data Registers), page number - 21
  The register address for OUT_X_MSB is 0x01. This can be found at Page 19 of https://www.nxp.com/docs/en/data-sheet/MMA8452Q.pdf

*/
  i2c.transfer(new Buffer([0x01]), 6, function (error, dataReceived) {

      // This is an exception. If an error is generated, it will throw the error
      if (error) throw error;

      //This array is going to store the final x,y,z values
      var out=[];

      // The for loop is iterated 3 times, in order to extract the x,y, and z acceleration values.
      for (var i=0;i<3;i++){
      
      /*
    
       We have 16 bits to put together - the most significant 8 bits and the least significant 8 bits so that we can interpret them as one number. This combined value is placed in gCount variable using the OR operation.
      The gCount variable is a bitwise OR operation between two binary numbers:
        1. The 8 bits in OUT_MSB for a given coordinate are shifted left by 8 bits in order to make space for the remaining 8 bits
        of the OUT_LSB.
        2. The OUT_LSB of the respective coordinate.
        
     */
        var gCount=(dataReceived[i*2] << 8) | dataReceived[(i*2)+1];
        
      // The gCount number of the respective coordinate is right shifted by 4 to get rid of the unused lower 0 bits which are 4 in number yielding the 12 bits that the datasheet specifies each coordinate has on page number 21.  
        gCount=gCount >> 4;

      /*

      The x,y, and z accelerometer values are stored in their respective OUT_MSB and OUT_LSB registers as negative values i.e. their 2's       complement form. 
      Therefore in order to obtain the correct values, check whether the most significant bit of the OUT_MSB is 1 or 0 i.e whether the         coordinate value is negative or positive. 
      If it is negative (checked using 0x7F, which is the maximum possible number that can be made from 7 bits), the if condition             changes it to a 2's complement form, thus making it positive and adding a "-" sign in front of it.
      127 is checking whether we have a 0 or a 1 at the first position - basically its sign.

      */
        if (dataReceived[i*2] > 0x7F) {
            gCount = -(1 + 0xFFF - gCount); // Transform into negative 2's complement
          }
          
      // Lastly, normalize the coordinate to get a value between 0 and 1, dividing gCount by 2^10.
          out[i] = gCount / ((1<<12)/(2*2));
      }
      console.log('The x, y, z values are :',out);

  });
```

### Sample Ouput

![output](https://i.imgur.com/Dg462Jf.jpg)

[Datasheet for accelerometer module](http://www.nxp.com/docs/en/data-sheet/MMA8452Q.pdf)

[More Informatiom on I2C Communication](https://learn.sparkfun.com/tutorials/i2c)
