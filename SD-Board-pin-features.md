#Board Features
====

##Serial Options
* USB serial init: Serial.begin()
* On board serial1 pins 8rx, 9tx: Serial0.begin()
* On board serial2 pins 28rx, 29tx: Serial1.begin()

##PRG Button
* PIN_BTN1 defined to access the PRG Button
* To enable: `pinMode(PIN_BTN1, INPUT);`

##On Board LED
* PIN_LED1 defined as pin 21 
* To enable: `pinMode(PIN_LED1, OUTPUT);`

##PWM Pins
* The Pins are 4,7,8,9,10

##SPI
* The SPI pins are: 24, 25, 26, 27
* 24: SCK, 25: SDI(Serial Data In), 26: SDO (Serial Data Out), 27: SS
NOTE (From the ChipKit Wiki):

| SPI Pin | SPI Label|
|:---:|:----:|
|24|SCK, SCLK, CLK|
|25|SDI, MOSI|
|26|SDO, MISO|
|27|SS, CS|


The SPI interface on AVR microcontrollers uses four signals labeled SS (slave select), MISO (master in/slave out), MOSI (master out/slave in) and SCK (serial clock). On AVR microcontrollers, MISO and MOSI switch direction depending on whether the SPI controller is enabled in master mode or slave mode.




