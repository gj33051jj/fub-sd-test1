PPS General Info
===
The PIC32MX1xx and PIC32MX2xx family of parts have a peripheral called the Peripheral Pin Select (PPS) that allows you to change which pins certain functions inside the chip map to. This applies to inputs and outputs. Every pin can be a GPIO (General Purpose Input or Output) - i.e. a digital output or digital input. Most pins also have alternative functions that you can turn on - either fixed functions like I2C that can not be re-mapped to different pins, or functions like UART or PWM that can be re-mapped using the PPS.

The Fubarino Mini, along with the Digilent DP32, both use MCUs that have PPS support. It is very important to understand that for some peripherals, you MUST map them using PPS to pins before you can use them. They are not, by default, set up to be mapped on boot. Over time, this may change, as the core library code for MPIDE is updated to have default pins on boot for every peripheral. Currently only the UARTs are automatically mapped. All other PPS-enabled-peripherals must be mapped before they can be used.

The MPIDE core library code contains some functions and defines that you can use to make PPS mapping very simple - at least much simpler than trying to set up all of the registers yourself! The main function you will use is 

```
mapPps(pin, function);
```

The <pin> argument to this function is an "Arduino Pin Number". This is the pin number that you normally supply to functions like digitalWrite(), etc. 

The <function> argument is one of a list of constants that specify which actual peripherals function you are mapping to that pin. For example, PPS_OUT_OC1 for mapping the Output Compare 1 (for PMW output for example).

The mapPps() function is used to map both inputs and outputs to the pins. There are some important restrictions to understand about how PPS works. 

1) There are up to eight I/O pins that any particular <function> can legally be mapped to. So you only get a small sub-set of the various I/O pins to choose from, and for each group of peripherals they are different.

2) Although in hardware, a single peripheral output function can be mapped to multiple (up to 8) actual output pins at once, the current MPIDE mapPps() code does not support this.

3) Although in hardware, a single input pin can be mapped to multiple (up to 8) actual peripheral input functions at once, the current MPIDE mapPps() code does not support this.

What 2) and 3) above mean is that mapPps() creates a 1:1 link between an Arduino Pin Number and a peripheral function inside the MCU. Each Arduino Pin Number can only be mapped to a single function, which is either an input or an output. Normally this restriction will not cause any problems. Only in very rare cases would a many-to-one mapping be required. 

The key to the whole deal is the proper chapter in the [datasheet (section 11.3)](http://ww1.microchip.com/downloads/en/DeviceDoc/61168E.pdf) put together with the ppsFunctionType enum in p32_defs.h.

On boot, all pins are set to be PPS_OUT_GPIO except for UART pins. To make a different peripheral come out of a particular pin, you have to call

```
mapPps(pin, function);
```
For example, to have OC1 come out of pin 14, you need to call

```
mapPps(14, PPS_OUT_OC1);
```

What combinations are valid? That's where (for now) you need to cross-reference the datasheet. Only certain functions can be mapped to certain pins - it's not an any-to-any type thing. So you have to look at the schematic of the Mini, find out the port name of the pin you're interested in, then look in the datasheet (table 11-1 for inputs, table 11-2 for outputs) to see if the function you want to use can be mapped to that pin. Then, look up what Arduino pin that port corresponds to, and put that into the mapPps() function. 

On boot, the only things we map are the UARTs - the SPI pins, output compares, input captures, etc. are all left alone. So users will need to do mapPps() before they can use any of that.


Example PPS Sketch
===
```C++
#include <Wire.h>
//Fubarino Mini.

#define LED 1
#define SCK2 4
#define SDI2 18
#define SDO2 20


void setup() {                
  pinMode(LED, OUTPUT);
  Serial.begin(115200);

  //TODO: write a verbose "wait for keypress routine"
  for (int i = 7; i>=0; i--)
  {
    Serial.println(i);   
    delay(1000);
  }
  Serial.println("*boom*");
  
//  ANSELA = 0x0000; //all digital
//  ANSELB = 0x0000; //all digital
//  ANSELC = 0x0000; //all digital
  pinMode(SCK2, OUTPUT);
  pinMode(SDI2, INPUT);
  pinMode(SDO2, OUTPUT);

  init_pps();

  delay(100);
  init_spi();
}

void init_pps()
{
  if (mapPps(18, PPS_IN_SDI2))
  {
    Serial.println("success mapping SDI2 to 18");
  } 
  else
  {
    Serial.println("failure mapping SDI2 to 18");
  }

  if (mapPps(20, PPS_OUT_SDO2))
  {
    Serial.println("success mapping SDO2 to 20");
  } 
  else
  {
    Serial.println("failure mapping SDO2 to 20");
  }   
  SDI2R = 0x02;
  Serial.print("SDI2R=");
  Serial.println(SDI2R, BIN);
  Serial.print("RPC3R=");
  Serial.println(RPC3R, BIN);
}

int write_spi(int input)
{
  while (!SPI2STATbits.SPITBE) {}; //wait until the transmit buffer is empty
  SPI2BUF = input;
  while (!SPI2STATbits.SPIRBF) {}; //wait until the receive buffer is full
  return SPI2BUF;
}

void init_spi()
{
  unsigned int rData;
  //IEC0CLR=0x03800000; // disable all interrupts
  
  SPI2CON = 0; // Stops and resets the SPI2;
  SPI2CON2 = 0; // clear out any erroneous settings.
  SPI2BRG = 0;
  rData = SPI2BUF; // clears the receive buffer
  SPI2STATCLR = 0x40; // clear the Overflow
  SPI2BRG = 0x4D; // (to generate 256 kbps sample rate, PBCLK @ 40 MHz)
  SPI2CONbits.MSTEN = 1;
  Serial.print("SPI2STAT: "); Serial.println(SPI2STAT, BIN);
  SPI2CONbits.ON = 1;
  int loopback = write_spi(100);
  Serial.println(loopback, DEC);
  Serial.print("SPI2STAT: "); Serial.println(SPI2STAT, BIN);
  Serial.println(loopback, DEC);
}

void loop() {
  int loopback = write_spi(0x55);
}