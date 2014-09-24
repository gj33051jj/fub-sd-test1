Arduino 1.x library issues and fixes (These and more are now in the chipKIT master branch)
====

pgmspace.h: http://www.arduino.cc/en/Reference/PROGMEM
=====
###Example 1:
```
#include <avr/pgmspace.h>
```
source:
https://github.com/adafruit/Adafruit_Sensor/blob/master/Adafruit_Sensor.cpp

###FIX 
https://github.com/adafruit/TFTLCD-Library/blob/master/Adafruit_TFTLCD.cpp
```
#if defined(__PIC32MX__)
    #define PROGMEM
    #define pgm_read_byte(addr) (*(const unsigned char *)(addr))
    #define pgm_read_word(addr) (*(const unsigned short *)(addr))
#endif
#if defined(__SAM3X8E__)
#include <include/pio.h>
    #define PROGMEM
    #define pgm_read_byte(addr) (*(const unsigned char *)(addr))
    #define pgm_read_word(addr) (*(const unsigned short *)(addr))
#endif
#if define(__AVR__)
#include <avr/pgmspace.h>
#endif
```


F() Store string in Flash: 
======
###Example:
```
lcd.println(F("Const char in flash"));.
````
###Fix: http://chipkit.net/forum/viewtopic.php?f=7&t=1496
```
#if defined(__PIC32MX__)
  #if defined F
    #undef F
  #endif
  #define F(X) (X)
#endif
```

SoftwareSerial required (Super frustrating):  
====
###Example 1:
source: https://github.com/adafruit/Adafruit-GPS-Library/blob/master/Adafruit_GPS.h
###Example:
```
#if ARDUINO >= 100
 #include <SoftwareSerial.h>
#else
 #include <NewSoftSerial.h>
#endif
```

###Example 2:
Source: https://github.com/adafruit/Adafruit-Thermal-Printer-Library/blob/master/Adafruit_Thermal.h
```
#if ARDUINO >= 100
 #include "Arduino.h"
 #include "SoftwareSerial.h"
#else
 #include "WProgram.h"
 #include "WConstants.h"
 #include "NewSoftSerial.h"
#endif
```
Issue: We don't have SoftwareSerial in chipKIT

###Fix, current:
Replace by allowing the user to pass the SerialObject from sketch into library. Put conditions around SoftwareSerial.

###Fix, long term:
Add support for SoftwareSerial even though we have more hardware serial available, and better off using the Hardware serial for projects. Also, 0023 we don't have NewSoftSerial either. Maybe include a version of that too.

