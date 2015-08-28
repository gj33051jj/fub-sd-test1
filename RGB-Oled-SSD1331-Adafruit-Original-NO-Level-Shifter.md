# RGB Oled SSD1331 Adafruit Original NO Level Shifter

Product From Adafruit:
[http://www.adafruit.com/products/684](http://www.adafruit.com/products/684)

Original Tutorial From Adafruit:
[https://learn.adafruit.com/096-mini-color-oled/wiring](https://learn.adafruit.com/096-mini-color-oled/wiring)

The code depends on the GLFX Library from Adafruit which can be found in the Arduino library manager or directly here:
https://github.com/adafruit/Adafruit-GFX-Library

This modified Adafruit-SSD1331-OLED-Driver-Library-for-Arduino
 library allows for PIC32 hardware SPI support:
https://github.com/ricklon/Adafruit-SSD1331-OLED-Driver-Library-for-Arduino

Mapping for the Fubarino Mini then Fubarino SD

SSD1331 | Fubarino Mini | Fubarino SD | Arduino Uno
----- | ----- | ----- | -----
GND | GND | GND | GND
3.3v | 3.3v  | 3.3v | 3.3v //issue Uno needs level shifter
SD CS | | | Skip
OLED CS | 30 | 27 | 10 //SS/CS
OLED Reset | 9 | 9 | 9 //digital
OLED D/C | 8 | 8 | 8 //digital
OLED SCLK | 4 | 24 | 13 //SCK
OLED Data | 29 | 25 | 11 //SDO/Mosi
SD Detect | | | Skip
