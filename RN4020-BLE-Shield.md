BLE Shield for chipKIT


Feature | shield | Fubarino SD pin | Fubarino Mini pin
------- | ----- | ------- | -----
rx | 0 | 8 | 18 |
tx  | 1 | 9 | 17 | 
wake_sw | 7 | 11 | 11
wake_hw | 8 | 12 | 12
cmd_mldp //command vs data? | 4 | 13 | 13
more pins | //optional


Code Example
---

```
#define WAKE_SW 11
#define WAKE_HW 12
#define CMD_MLPD 13

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
  Serial0.begin(115200);
  Serial1.begin(115200);
  delay(200);
  Serial1.print("+\n");
  delay(500);
  Serial1.print("SF,1\n");
  delay(500);
  Serial1.print("SS,30000000\n");
  delay(500);
  Serial1.print("SR,32000800\n"); 
  delay(500);
  Serial1.print("SB,4\n");
  delay(500);
  Serial1.print("R,1\n");
  delay(2000);
}

void loop ()
{
  if(Serial1.available()>0)
  {
    while(Serial1.available()>0)
    {
      incoming = Serial1.read();
      inputString += String(incoming);
      delay(6);
    }
    Serial0.print("Received: " + inputString+"\n");
    Serial1.print("Received: " + inputString+"\n");
    delay(100);
    inputString ="";
  }
  delay(200);
  Serial1.print("D\n");
  delay(800);
  Serial1.print("+\n");
  delay(800);
  Serial1.print("V\n");
  delay(1000);
}



```

