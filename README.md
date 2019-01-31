# hanim
//Help me solve this. i want to connect Arduino with blynk and the serial monitor keep showing the authorize token instead of the sensor's value. i use Ultrasonic sensor.

#include <Servo.h>
#define trigPin 6
#define echoPin 7
#define BLYNK_PRINT DebugSerial
Servo servo;

#include <SoftwareSerial.h>
SoftwareSerial DebugSerial(2, 3); // RX, TX

#define PIN_UPTIME V5
#define PIN_UPTIME V1

#include <BlynkSimpleStream.h>

// You should get Auth Token in the Blynk App.
// Go to the Project Settings (nut icon).
char auth[] = "9f62605f4fde495f830fa351e58b2661";

BlynkTimer timer;

// This function sends Arduino's up time every second to Virtual Pin (5).
// In the app, Widget's reading frequency should be set to PUSH. This means
// that you define how often to send data to Blynk App.
void myTimerEvent()
{
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
  Blynk.virtualWrite(V5, millis() / 1000);
    Blynk.virtualWrite(V1, millis() / 1000);

}

void setup()

{
  DebugSerial.begin(9600);

Serial.begin (9600);
  Blynk.begin(Serial,  auth);
  Serial1.begin(9600);

pinMode(trigPin, OUTPUT);

pinMode(echoPin, INPUT);

servo.attach(8);

timer.setInterval(1000L, myTimerEvent);


}

void loop()

{
long duration, distance;

Blynk.virtualWrite (V5,distance);
Blynk.virtualWrite (V1,distance);


digitalWrite(trigPin, LOW);

delayMicroseconds(2);

digitalWrite(trigPin, HIGH);

delayMicroseconds(10);

digitalWrite(trigPin, LOW);

duration = pulseIn(echoPin, HIGH);

distance = (duration/2) / 29.1;

if (distance > 20)

{

//Serial.println("the distance is less than 5");

servo.write(100);

}

else

{

servo.write(0);

}

if (distance > 60 || distance <= 0)

{

//Serial.println("The distance is more than 60");

}

else

{

Serial.print(distance);

Serial.println(" cm");

}
delay(1000);
  Blynk.run();
  timer.run(); // Initiates BlynkTimer

}
