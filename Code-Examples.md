#Code Example
====
* Using Interrupts
* Use the Core Timer

``` C
// CoreTimer demo1 : demonstrates a simple callback scheduled at
// a single frequency (20Hz). This example code is in the public domain.
// This example blinks the on-board LED at 20Hz from a CoreTimer
// callback function. In the callback function, you return an unsigned
// int that represents when, in the future, you want to get called again.

void setup() 
{
  pinMode(PIN_LED1, OUTPUT);
  attachCoreTimerService(MyCallback);
}

// We don't need to do anything in the main loop
void loop() 
{
}

// For the core timer callback, just toggle the output high and low
// and schedule us for another 50ms in the future. CORE_TICK_RATE
// is the number of core timer counts in 1 millisecond. So if we 
// want this callback to be called every 50ms, we just multiply 
// the CORE_TICK_RATE by 50, and add it to the current time.
// currentTime is the core timer clock value at the moment we get
// called.
uint32_t MyCallback(uint32_t currentTime) 
{
  digitalWrite(PIN_LED1, !digitalRead(PIN_LED1));
  return (currentTime + (CORE_TICK_RATE * 50));
}
```

* Use the on board button and LED

``` C
// This sketch demonstrates how to blink the on-board LED
// and read the on-board button. When you run the sketch,
// The LED will blink slowly until you hold down the PRG
// button. Then it will blink fast.

// Controls the LED blink speed, in ms
int LedSpeed = 1000;

void setup()
{
  // Set up our two pins of interest
  digitalWrite(PIN_LED1, LOW);
  pinMode(PIN_LED1, OUTPUT);
  pinMode(PIN_BTN1, INPUT);
}

void loop()
{
  digitalWrite(PIN_LED1, HIGH);
  delay(LedSpeed);
  digitalWrite(PIN_LED1, LOW);
  delay(LedSpeed);
  if (digitalRead(PIN_BTN1) == 1)
  {
    LedSpeed = 1000;
  }
  else
  {
    LedSpeed = 50;
  }
}
```

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