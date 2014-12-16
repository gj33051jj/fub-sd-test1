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

##SPI
* The SPI2 pins are: 4 (SCK2), 27 (SDI2), 29 (SDO2), 30 (SS2)
* The SPI1 pins are: 3 (SCK1), 19 (SDI1), 18 (SDI1), 17 (SS1) 

Note that SPI2 is the 'default' SPI port, and is what is used with the Arduino SPI.h library.

|Mini SPI| SD SPI Pin | SPI Label| Arduino Uno Pin|
|:--:|:---:|:----:|:---:|
|4|24|SCK, SCLK, CLK| 13| 
|29|25|SDO, MOSI| 11|
|27|26|SDI, MISO| 12|
|30|27|SS, CS| 10|

Here is a simple sketch showing a simple SPI loopback test for the default SPI port:

    // SPI Loopback test - connect MISO and MOSI
    #include <SPI.h>

    const int slaveSelectPin = 30;
    char x;

    void setup() {
      Serial.begin(115200);
      delay(5000);
      pinMode (slaveSelectPin, OUTPUT);
      SPI.begin(); 
    }

    void loop() {
      digitalWrite(slaveSelectPin,LOW);
      Serial.println(SPI.transfer(x), DEC);
      digitalWrite(slaveSelectPin,HIGH); 
      delay(250);
      x++;
    }

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

##Interrupts

* The Fubarino Mini has hardware interrupts on pins 24 (non-PPS), 3 (PPS), 0 (PPS), 6 (PPS) and 4 (PPS)
* Pin 24 is INT0, Pin 3 is INT1, Pin 0 is INT2, Pin 6 is INT3 and Pin 4 is INT5

Here is a sketch showing how to use these interrupts. 

	// Move a wire from pin 12 to each of the hardware interrupt pins to show that
	// they print out the proper message.
	#define pinSrc 12 // used as external interrupt stimulator

	void int0Handle();
	void int1Handle();
	void int2Handle();
	void int3Handle();
	void int4Handle();

	int x;

	void setup() 
	{
	  
	  Serial.begin(9600);              // Turn on UART to pi
	  digitalWrite(PIN_LED1, LOW);      // Start of with LED off
	  pinMode(PIN_LED1, OUTPUT);        // Make LED pin an output
	  pinMode(PIN_INT0, INPUT);         // Interrupt pin must be an input
	  pinMode(PIN_INT1, INPUT);         // Interrupt pin must be an input
	  pinMode(PIN_INT2, INPUT);         // Interrupt pin must be an input
	  pinMode(PIN_INT3, INPUT);         // Interrupt pin must be an input
	  pinMode(PIN_INT4, INPUT);         // Interrupt pin must be an input
	  digitalWrite(pinSrc, LOW);        // Simulator pin must start low
	  pinMode(pinSrc, OUTPUT);          // And stim pin must be output too
	  attachInterrupt(0, int0Handle, RISING); // Register interrupt function on Int0
	  attachInterrupt(1, int1Handle, RISING); // Register interrupt function on Int1
	  attachInterrupt(2, int2Handle, RISING); // Register interrupt function on Int2
	  attachInterrupt(3, int3Handle, RISING); // Register interrupt function on Int3
	  attachInterrupt(4, int4Handle, RISING); // Register interrupt function on Int4
	}

	void loop() 
	{
	   digitalWrite(pinSrc, HIGH);      // Set stim pin high (triggers interrupt)
	   delay(1000);                      // wait half a second
	   Serial.println("Main loop tick");
	   digitalWrite(pinSrc, LOW);       // Set stim pin low
	   delay(1000);                      // wait another half second
	}

	// This function gets called when an external interrupt occurs
	void int0Handle() 
	{
	  digitalWrite(PIN_LED1, !digitalRead(PIN_LED1));
	  Serial.println("int0Handler() triggered");
	}

	void int1Handle() 
	{
	  digitalWrite(PIN_LED1, !digitalRead(PIN_LED1));
	  Serial.println("int1Handler() triggered");
	}

	void int2Handle() 
	{
	  digitalWrite(PIN_LED1, !digitalRead(PIN_LED1));
	  Serial.println("int2Handler() triggered");
	}

	void int3Handle() 
	{
	  digitalWrite(PIN_LED1, !digitalRead(PIN_LED1));
	  Serial.println("int3Handler() triggered");
	}

	void int4Handle() 
	{
	  digitalWrite(PIN_LED1, !digitalRead(PIN_LED1));
	  Serial.println("int4Handler() triggered");
	}
