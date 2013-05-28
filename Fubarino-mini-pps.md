PPS General Info
===
The key to the whole deal is the proper chapter in the [datasheet (section 11.3)](http://ww1.microchip.com/downloads/en/DeviceDoc/61168E.pdf) put together with the ppsFunctionType enum in p32_defs.h.

On boot, all pins are set to be PPS_OUT_GPIO. To make a different peripheral come out of a particular pin, you have to call

```
mapPps(pin, function);
```
For example, to have OC1 come out of pin 14, you need to call

```
mapPps(14, PPS_OUT_OC1);
```

What combinations are valid? That's where (for now) you need to cross-reference the datasheet. Only certain functions can be mapped to certain pins - it's not an any-to-any type thing. So you have to look at the schematic of the Mini, find out the port name of the pin you're interested in, then look in the datasheet (table 11-1 for inputs, table 11-2 for outputs) to see if the function you want to use can be mapped to that pin. Then, look up what Arduino pin that port corresponds to, and put that into the mapPps() function. 

On boot, the only things we map are the UARTs - the SPI pins, output compares, input captures, etc. all alone. So users will need to do mapPps() before they can use any of that.


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