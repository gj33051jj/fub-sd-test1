#Code Example
====
* Using Interrupts
* Use the Core Timer
* Use the on board button and LED
* Use the two built-in hardware UART (serial) ports

This code is a simple example of how to use Serial (USB serial), Serial0 (with RX on pin 8 and TX on pin 9) and Serial1 (with RX on pin 28 and TX on pin 29) for both input and output in a sketch. 

``` C
// UART test for Fubarino SD board
// Test out USB Serial and two hardware UARTs
// Use FTDI or other USB to serial cable with PC and
// terminal emulator for the two hardware UARTs

void setup() {
  Serial.begin(9600);     // USB port - baud doesn't matter
  Serial0.begin(115200);  // UART1 - pins 8 and 9
  Serial1.begin(115200);  // UART2 - pins 28 and 29
}

void loop() {
  Serial.println("This is USB serial ");
  Serial0.println("This is serial0 ");
  Serial1.println("This is serial1 ");
  if (Serial.available())
  {
    // Echo back what got sent + 1
    Serial.println(byte(Serial.read() + 1));
  }
  if (Serial0.available())
  {
    // Echo back what got sent + 1
    Serial0.println(byte(Serial0.read() + 1));
  }
  if (Serial1.available())
  {
    // Echo back what got sent + 1
    Serial1.println(byte(Serial1.read() + 1));
  }
  delay(200);
}
```