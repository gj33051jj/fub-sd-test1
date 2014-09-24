# Firmata
## Firmata
[Firmata Website](http://www.firmata.org)

## Board Details
## Fubarino SD 
Fubarino SD board added to boards.h
```
// pic32 board FubarinoSD
#elif defined(_BOARD_FUBARINO_SD_)
#define TOTAL_ANALOG_PINS 15
#define TOTAL_PINS 44 // All pins can be digital
#define MAX_SERVOS TOTAL_PINS //All pins can be servo with SoftPWMservo
#define VERSION_BLINK_PIN PIN_LED1
#define IS_PIN_DIGITAL(p) ((p) >= 0 && (p) <= TOTAL_PINS)
#define IS_PIN_ANALOG(p) ((p) >= 30 && (p) <= 44)
#define IS_PIN_PWM(p) digitalPinHasPWM(p)
#define IS_PIN_SERVO(p) ((p) >= 0 && (p) < MAX_SERVOS)
#define IS_PIN_I2C(p) ((p) == 1 || (p) == 2)
#define PIN_TO_DIGITAL(p) (p)
#define PIN_TO_ANALOG(p) (p)
#define PIN_TO_PWM(p) PIN_TO_DIGITAL(p)
#define PIN_TO_SERVO(p) (p)
```
## Fubarino Mini 
Fubarino Mini added to boards.h
```
// pic32 board FubarinoMini
#elif defined(_BOARD_FUBARINO_MINI_)
#define TOTAL_ANALOG_PINS NUM_ANALOG_PINS
#define TOTAL_PINS NUM_DIGITAL_PINS // All pins can be digital
#define MAX_SERVOS TOTAL_PINS //All pins can be servo with SoftPWMservo
#define VERSION_BLINK_PIN PIN_LED1
#define IS_PIN_DIGITAL(p) ((p) >= 0 && (p) <= TOTAL_PINS)
#define IS_PIN_ANALOG(p) ((p) == 0 || (p) == 20 || ((p) >= 3 && (p) <= 13))
#define IS_PIN_PWM(p) digitalPinHasPWM(p)
#define IS_PIN_SERVO(p) ((p) >= 0 && (p) < MAX_SERVOS)
#define IS_PIN_I2C(p) ((p) == 25 || (p) == 26)
#define PIN_TO_DIGITAL(p) (p)
#define PIN_TO_ANALOG(p) (p)
#define PIN_TO_PWM(p) PIN_TO_DIGITAL(p)
#define PIN_TO_SERVO(p) (p)
```

## Fubarino SDZ
Coming Soon