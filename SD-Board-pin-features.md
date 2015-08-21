##Serial Options
* USB serial init: Serial.begin()
* On board serial1 pins 8rx, 9tx: Serial0.begin()
* On board serial2 pins 28rx, 29tx: Serial1.begin()

##PRG Button
* PIN_BTN1 defined to access the PRG Button as pin 23
* To enable: `pinMode(PIN_BTN1, INPUT);`

##On Board LED
* PIN_LED1 defined as pin 21 
* To enable: `pinMode(PIN_LED1, OUTPUT);`

##PWM Pins
* The Pins are 4,7,8,9,10

##I2C
Pins 1 & 2 are the default I2C interface for the TwoWire (Wire.h) library.

* SDA1: 1
* SCL1: 2
* SDA2: 28
* SCL2: 29

The pins are 1,2 and 29,28

##SPI
* The SPI pins are: 24, 25, 26, 27
* 24: SCK, 25: SDI(Serial Data In), 26: SDO (Serial Data Out), 27: SS
NOTE (From the ChipKit Wiki):

SPI default 0

| SPI Pin | SPI Label| Arduino Uno Pin|
|:---:|:----:|:---:|
|24|SCK, SCLK, CLK| 13| 
|25|SDI, MISO| 11|
|26|SDO, MOSI| 12|
|27|SS, CS| 10|

SPI1 default 1

//on board mx795 SPI3

| SPI Pin | SPI Label| 
|:---:|:----:|:---:|
|7|SCK, SCLK, CLK|  
|8|SDI, MISO| 
|9|SDO, MOSI| 
|1|SS, CS|

SPI2 //todo
SPI3 //todo  


The SPI interface on AVR microcontrollers uses four signals labeled SS (slave select), MISO (master in/slave out), MOSI (master out/slave in) and SCK (serial clock). On AVR microcontrollers, MISO and MOSI switch direction depending on whether the SPI controller is enabled in master mode or slave mode.



