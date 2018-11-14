# clap-switch

/*
Created 2018
by  Roman Dirscherl
  
 You need:
 (ky- 030, ky- 037, lm393) Or any other soundmodul
 arduino microcontroller
 connector wires
 Led with resistor or a relay (ky- 019) or a transistor
 
 Wiring:
 Soundmodul: Aout to pin11; 5v to 5v; GND to GND
 Led: + to pin10; GND to GND
 */
 
 
int microphone = 11;// Mikrofon
int light = 10;// Lampe
int claps = 0;
long detectionSpanInitial = 0;
long detectionSpan = 0;
boolean lightState = false;
 
void setup() {
  pinMode(microphone, INPUT);
  pinMode(light, OUTPUT);
}
 
void loop() {
 
  int sensorState = digitalRead(light);
 
  if (sensorState == 0)
  {
    if (claps == 0)
    {
      detectionSpanInitial = detectionSpan = millis();
      claps++;
    }
    else if (claps > 0 && millis()-detectionSpan >= 50)
    {
      detectionSpan = millis();
      claps++;
    }
  }
 
  if (millis()-detectionSpanInitial >= 400)
  {
    if (claps == 2)
    {
      if (!lightState)
        {
          lightState = true;
          digitalWrite(light, HIGH);
        }
        else if (lightState)
        {
          lightState = false;
          digitalWrite(light, LOW);
        }
    }
    claps = 0;
  }
}
