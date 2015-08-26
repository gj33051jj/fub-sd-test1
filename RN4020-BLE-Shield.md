BLE Shield for chipKIT

What is the shield?
----
It is a break out board for the RN4020 BLE device from Microchip. It has several features like mosfets on board which are broken out and can be used from the shield.

RN4070 Commands and Details
----
[RN4020 Data Sheet](http://ww1.microchip.com/downloads/en/DeviceDoc/70005191B.pdf)

Access the device from Android
----
[MLDP Terminal bluetooth viewer](ww1.microchip.com/downloads/en/DeviceDoc/MLDPTerminal8.apk  )


Pin Mappings for Fubarino Boards
----

Feature | shield/uc32 pin | Fubarino SD pin | Fubarino Mini pin
------- | ----- | ------- | -----
rx | 0 | 8 | 18 |
tx  | 1 | 9 | 17 | 
cmd_mldp //command vs data? | 4 | 12 | 13
wake_sw | 7 | 10 | 11
wake_hw | 8 | 11 | 12
MOSFET gate pin | 34 |||
MOSFET gate pin | 35 |||
MOSFET gate pin | 36 |||
MOSFET gate pin | 37 |||
MOSFET gate pin | 38 |||
rx | 39 |||
tx | 40 |||
A0 ||||
A1 ||||
A2 ||||



Code Example
---

```
/*
  chipKIT BLE shield Simlpe serial and echo

*/

#define WAKE_SW 10
#define WAKE_HW 11
#define CMD_MLPD 12
#define MAX_INPUT 200


String inputString = "";
char incomingByte = 0;
String commandString = "";
char cmdByte = 0;

void setup() {
  //wake_sw
  pinMode( WAKE_SW, OUTPUT);
  digitalWrite( WAKE_SW, HIGH);
  //wake_hw
  pinMode(WAKE_HW, OUTPUT);
  digitalWrite(WAKE_HW, HIGH);
  //cmd_mldp
  pinMode(CMD_MLPD, OUTPUT);
  digitalWrite(CMD_MLPD, HIGH);

  delay(800);

  Serial.begin(115200);
  Serial0.begin(115200);


  while (1)
  {
    if (Serial.available() > 0)
    {
      char cc = Serial.read();
      if (cc == 'h' || cc == '?' || cc == '\n' || cc == '\r') {
        Serial.println("Press 's' to start.");
        Serial.println("Sample Commands:");
        Serial.println("'D Dump'");
        Serial.println("'SN,<string> set device name'");
        Serial.println("'LS List Server Services'");
        Serial.println("'LC List Client Services'");
        Serial.println("'M Last Signal Strength'");
        Serial.println("'R,1 Reboot'");
        Serial.println("'V Version'");
        Serial.println("'H Help'");
      }
      if (cc == 's') {
        break;
      }
    }
  }
  Serial.println("start");


  delay(200);
  Serial0.print("+\n");
  delay(500);
  Serial0.print("SF,1\n");
  delay(500);
  Serial0.print("SS,30000000\n");
  delay(500);
  Serial0.print("SR,32000800\n");
  delay(500);
  Serial0.print("SN,HEYBUDDY\n");
  delay(500);
  Serial0.print("SB,4\n");
  delay(500);
  Serial0.print("R,1\n");
  delay(2000);
}

void loop() {
  if (Serial.available() > 0) {
    cmdByte = Serial.read();
    commandString += String(cmdByte);

    if (cmdByte == '\n' || cmdByte == '\r')
    {
      Serial.print("cmd: ");
      Serial.println(commandString);
      Serial0.print(commandString);
      commandString = "";
    }
  }
  //read from ble device blocking or non blocking
  if (Serial0.available() > 0)
  {

    incomingByte = Serial0.read();
    inputString += String(incomingByte);

    if (incomingByte == '\n' || incomingByte == '\r')
    {
      //Serial.print("resp: ");
      Serial.print(inputString);
      inputString = "";
    }
  }

}
```

