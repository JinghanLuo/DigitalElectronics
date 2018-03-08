## Midterm Project

### The Eye (In progress)

The project is an illuminating eye (in the final version) that tracks and follows people's movement around it. It is also capable of telling the time, triggered by touching. </br>
At this stage there are two PIR sensors installed and the servo would rotate to the direction in which movement is detected. A piezo is triggered by a capacitive sensorm but right now it only makes a constant sound. </br>

#### Initial sketch </br>
<html>
  <img src="https://github.com/JinghanLuo/DigitalElectronics/blob/master/MidtermProject/images/sketch0208.jpg" />
</html>

#### Process

The first step of building this project was getting the servo react to the input of the PIR sensors. I started with some demo codes for the servo and PIR sensors on their own and tried to combine them.</br>
Here are some of the links of the references: </br>
Servo: https://www.allaboutcircuits.com/projects/servo-motor-control-with-an-arduino/ </br>
PIR: http://www.instructables.com/id/PIR-Motion-Detector-With-Arduino-simple-and-Easy-D/ </br>

One problem is that sometimes both sensors are detecting motion and the servo struggled between the two opposite directions that it's told to turn to. To better keep track of the motion detection, I added two led lights that would indicate each sensor's state respectively. Then I adjusted the delay time in the code for the servo and found a point where the pace of the servo is proper. </br>
<html>
  <img src="https://github.com/JinghanLuo/DigitalElectronics/blob/master/MidtermProject/images/process.jpg" />
</html>

Then I started installing the other part of the device. I got a capacitive sensor from Sparkfun and soldered some headers to it. Then I found some basic codes for capacitive sensors and piezo speakers and combined them, so that the activity on the capacitive sensor would trigger the speaker to make sound. </br>

<html>
  <img src="https://github.com/JinghanLuo/DigitalElectronics/blob/master/MidtermProject/images/process2.jpg" />
</html>

#### Code
Here's the full code of this version. </br>
  
    #include "Servo.h"
    Servo servo;        //initialize a servo object for the connected servo

    int angle = 90;
    const int inputPinA = 2;               // choose the input pin (for PIR sensor)
    const int inputPinB = 3;
    int valA = 0;                    // variable for reading the pin status
    int valB = 0;
    const int ledA = 9;
    const int ledB = 10;
    const int touchPin = 5;
    const int buzzer = 7;
    int touchState = 0;

    void setup() {
      pinMode(inputPinA, INPUT);     // declare sensor as input
      pinMode(inputPinB, INPUT);
      pinMode(ledA, OUTPUT);
      pinMode(ledB, OUTPUT);
      pinMode(touchPin, INPUT);
      pinMode(buzzer, OUTPUT);
      servo.attach(12);      // attach the signal pin of servo to pin9 of arduino
      Serial.begin(9600);
    }

    void loop() {
      valA = digitalRead(inputPinA);  // read input value
      valB = digitalRead(inputPinB);
      if (valA == HIGH) {
        digitalWrite(ledA, HIGH);
        Serial.println("A");
        for (angle; angle > 0; angle -= 10) {
          servo.write(angle);
          delay(50);
        }
      }
      else {
        digitalWrite(ledA, LOW);
      }
      if (valB == HIGH) {
        digitalWrite(ledB, HIGH);
        Serial.println("B");
        for (angle; angle < 180; angle += 5) {
          servo.write(angle);
          delay(50);
        }
      }
      else {
        digitalWrite(ledB, LOW);
      }

      touchState = digitalRead(touchPin);
      if (touchState == HIGH) {
        tone(buzzer, 600, 500);
      }
    }
    
#### Video 
Here's the link to the video showing how it works: </br>
https://drive.google.com/open?id=1QqiZkhrF3LSzVfZM7uT4oeBFitAybKEA

#### Schematic </br>
<html>
    <img src="https://github.com/JinghanLuo/DigitalElectronics/blob/master/MidtermProject/images/schematic.jpg" />
</html>

#### Next steps / goals for the final project
- Figure out a way for multitasking without using delay, so that it allows the sensors to keep working while the speaker makes sound in a rhythm. </br>
- Connect the arduino with a time module to embed time information. </br>
- Translate time into Morse code and convey it with the speaker (and light). </br>
- Make masks for the PIR sensors to limit its range and sensitivity. Something like tubes around the lenses, leaving only the front area. </br>
- Add more PIR sensors to allow multiple directions of detection. </br>
- Attach a LED bulb to the top of the servo and connect its action to the input from the capacitive sensor. Maybe sync it with the speaker. </br>
- Put everything together steadily. Make a base for the device and attach the eye to the LED bulb. </br>
