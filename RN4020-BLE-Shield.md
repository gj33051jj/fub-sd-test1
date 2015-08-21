BLE Shield for chipKIT


Feature | shield | Fubarino SD pin | Fubarino Mini pin
------- | ----- | ------- | -----
rx | 0 | 8 | 18 |
tx  | 1 | 9 | 17 | 
wake_sw | 7 | 10 | 11
wake_hw | 8 | 11 | 12
cmd_mldp //command vs data? | 4 | 12 | 13
more pins | //optional


Code Example
---

```
#define WAKE_SW 10
#define WAKE_HW 11
#define CMD_MLPD 12

String inputString ="";
char incoming = 0;

void setup ()
{
  //wake_sw
  pinMode( WAKE_SW,OUTPUT);
  digitalWrite( WAKE_SW,HIGH);
  //wake_hw
  pinMode(WAKE_HW,OUTPUT);
  digitalWrite(WAKE_HW,HIGH);
  //cmd_mldp
  pinMode(CMD_MLPD,OUTPUT);
  digitalWrite(CMD_MLPD,HIGH);

  delay(800);

  Serial.begin(115200);
  Serial0.begin(115200);


  while (1)
  {
    if (Serial.available() > 0) 
    {
      char cc = Serial.read();
      if (cc == 'h' || cc=='?') {
        Serial.println("Press 's' to start.");
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
  Serial.print("setup: ");

}

void loop ()
{
  Serial.print("REC: ");
  if(Serial0.available()>0)
  {
    while(Serial0.available()>0)
    {
      incoming = Serial0.read();
      inputString += String(incoming);
      delay(6);
    }
    Serial.print("Received: " + inputString+"\n");
    //Serial0.print("Received: " + inputString+"\n");
    delay(100);
    inputString ="";
  }
  delay(200);
  Serial0.print("D\n");
  delay(800);
  Serial0.print("+\n");
  delay(800);
  Serial0.print("V\n");
  delay(1000);
  
}


```

