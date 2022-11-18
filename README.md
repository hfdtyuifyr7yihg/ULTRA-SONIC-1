/*
    * Control Your PC with Ultrasonic Sensor HC-SR04 and Arduino
    * by Hanie Kiani
    *  https://electropeak.com/learn
 */
#include <Keyboard.h>
long duration;
int distance,distLeft,distRight;

const int trigger1 = 2; //Trigger pin of 1st Sesnor
const int echo1 = 3; //Echo pin of 1st Sesnor
const int trigger2 = 4; //Trigger pin of 2nd Sesnor
const int echo2 = 5;//Echo pin of 2nd Sesnor


void setup() {
Serial.begin(9600); 
  
pinMode(trigger1, OUTPUT); 
pinMode(echo1, INPUT); 
pinMode(trigger2, OUTPUT); 
pinMode(echo2, INPUT); 
}



void loop() { 
distance=calculateDistance(trigger1,echo1);
distLeft =distance; 

distance=calculateDistance(trigger2,echo2);
distRight =distance;
 
//Pause Modes -Hold
if ((distLeft >20 && distRight>20) && (distLeft <30 && distRight<30)) //Detect both hands
{Serial.println("Play/Pause"); 
                        Keyboard.press(KEY_TAB);
                        delay(100);
                        Keyboard.releaseAll();
  delay (500);}

distance=calculateDistance(trigger1,echo1);
distLeft =distance;

distance=calculateDistance(trigger2,echo2);
distRight =distance;

 

//Control Modes
//Lock Left - Control Mode
if (distLeft>=9 && distLeft<=14)
{
  delay(100); //Hand Hold Time
 distance=calculateDistance(trigger1,echo1);
  distLeft =distance;
  if (distLeft>=0 && distLeft<=15)
  {
    Serial.println("Left Hand detected");
    while(distLeft<=20)
    {
     distance=calculateDistance(trigger1,echo1);
      distLeft =distance;
      if (distLeft<5) //Hand pushed in 
      {Serial.println ("Volume Up");
                         Keyboard.press(KEY_UP_ARROW); //up  key
                         delay(100);
                         Keyboard.releaseAll();
        delay (300);}
      if (distLeft>17) //Hand pulled out
      {Serial.println ("Volume Down");
       Keyboard.press(KEY_DOWN_ARROW); //down  key
                         delay(100);
                         Keyboard.releaseAll();
       delay (300);}
    }
  }
}

//Lock Right - Control Mode
if (distRight>=9 && distRight<=14)
{
  delay(100); //Hand Hold Time
 distance=calculateDistance(trigger2,echo2);
  distRight =distance;
  if (distRight>=0 && distRight<=20)
  {
    Serial.println("Right Hnaad Detected");
    while(distRight<=20)
    {
      distance=calculateDistance(trigger2,echo2);
      distRight =distance;
      if (distRight<5) //Right hand pushed in
      {Serial.println ("Rewind"); 
       Keyboard.press(KEY_LEFT_ARROW); //left key
                         delay(100);
                         Keyboard.releaseAll();
       delay (300);
      }
      if (distRight>17) //Right hand pulled out
      {Serial.println ("Forward"); 
       Keyboard.press(KEY_RIGHT_ARROW); //right  key
                         delay(100);
                         Keyboard.releaseAll();
       delay (300);}
  }
}
}

delay(200);
}
 int calculateDistance(int trig, int echo){ 
  
  digitalWrite(trig, LOW); 
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trig, HIGH); 
  delayMicroseconds(10);
  digitalWrite(trig, LOW);
  duration = pulseIn(echo, HIGH); // Reads the echoPin, returns the sound wave travel time in microseconds
  distance= duration*0.034/2;
distance = 50;
 return distance;
}
