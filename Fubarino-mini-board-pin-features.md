#Fubarino Mini Board Features
====

##Serial Options
* USB serial init: Serial.begin()
* UART1: On board serial1 pins 17tx, 18rx: Serial0.begin()
* UART2: On board serial2 pins 26tx, 25rx: Serial1.begin()

Here is a short sketch showing all three serial ports in use:
	
    int inByte = 0;
    void setup()
    {
      Serial.begin(115200);
      Serial0.begin(115200);
      Serial1.begin(115200);
    }

    void loop()
    {
      Serial.print("USB\n");
      Serial0.print("0\n");
      Serial1.print("1\n");
      if (Serial.available() > 0) {
        inByte = Serial.read();
        Serial.print(inByte, DEC);
      }
      if (Serial0.available() > 0) {
        inByte = Serial0.read();
        Serial0.print(inByte, DEC);
      }
      if (Serial1.available() > 0) {
        inByte = Serial1.read();
        Serial1.print(inByte, DEC);
      }
      delay(250);
    }


##PRG Button
* PIN_BTN1 defined as pin 16
* PIN_BTN1 is labeled the PRG Button
* To enable: `pinMode(PIN_BTN1, INPUT);`

##On Board LED
* PIN_LED1 defined as pin 1 
* To enable: `pinMode(PIN_LED1, OUTPUT);`


##I2C
* Pins 9, 10
* Pins 26, 25 //Shared with UART2


##PPS Peripheral Pin Select

The PIC32MX250 part used on Fubarino Mini has a Peripheral Pin Select function for almost all of its I/O pins. When writing sketches for the Fubarino Mini, you must remember to connect an internal peripheral (like SPI or UART) to a particular set of I/O pins using the PPS functions (ppsInputSelect() and ppsOutputSelect()) before trying to use the peripheral. [See the example code on the Fubarino Mini Github site for more detailed information.](Fubarino-Mini-pps)


*Pins 0, 3-16, 17-32

##SPI: Still verifying the default
* The SPI pins are: 24, 25, 26, 27
* 24: SCK, 25: SDI(Serial Data In), 26: SDO (Serial Data Out), 27: SS
NOTE (From the ChipKit Wiki):

|Mini SPI| SD SPI Pin | SPI Label| Arduino Uno Pin|
|:--:|:---:|:----:|:---:|
|24|24|SCK, SCLK, CLK| 13| 
|26|25|SDI, MOSI| 11|
|25|26|SDO, MISO| 12|
|27|27|SS, CS| 10|

The SPI interface on AVR microcontrollers uses four signals labeled SS (slave select), MISO (master in/slave out), MOSI (master out/slave in) and SCK (serial clock). On AVR microcontrollers, MISO and MOSI switch direction depending on whether the SPI controller is enabled in master mode or slave mode.

##PWM Pins
* The default pins are 0,4,7,8,9

This sample sketch shows all five of the PWM outputs running:

    void setup()  {
    } 
    
    int x = 0;
    
    void loop()  { 
      analogWrite(0, x);
      analogWrite(4, x);
      analogWrite(7, x);
      analogWrite(8, x);
      analogWrite(9, x);
      delay(100);
      x = x + 8;
      if (x > 255) {
        x = 0;
      }
    }

Note that like most other digital type functions, these default pins can be re-mapped using PPS calls.