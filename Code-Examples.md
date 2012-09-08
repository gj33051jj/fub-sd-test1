#Code Example
====
* Using Interrupts
* Use the Core Timer
* Use the on board button and LED
* Use the two built-in hardware UART (serial) ports

This code is a simple example of how to use Serial0 (with RX on pin 8 and TX on pin 9) and Serial1 (with RX on pin 28 and TX on pin 29) for both input and output in a sketch. Note that these are NOT the same as the USB serial port, which is referred to as just "Serial" (not Serial0 or Serial1, etc.)

void setup() {
  Serial0.begin(115200);
  Serial1.begin(115200);
}

void loop() {
  Serial0.println("This is serial0 ");
  Serial1.println("This is serial1 ");
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