BLE Shield for chipKIT


Feature  | Fubarino SD pin | Fubarino Mini pin
------- | ------- | -----
rx  | 8 | 18 |
tx  | 9 | 17 | 
wake_sw | 11 | 11
wake_hw | 12 | 12
cmd_mldp //command vs data? | 13 | 13
more pins | //optional


Code Example
---

```
String inputString ="";
char incoming = 0;

void setup ()
{
  pinMode(4,OUTPUT);
  digitalWrite(4,HIGH);
  pinMode(7,OUTPUT);
  digitalWrite(7,HIGH);
  pinMode(8,OUTPUT);
  digitalWrite(8,HIGH);
  delay(800);
  Serial.begin(115200);
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
    Serial.print("Received: " + inputString+"\n");
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

