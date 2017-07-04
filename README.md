
/*
 Stepper Motor Control - one revolution

 This program drives a unipolar or bipolar stepper motor.
 The motor is attached to digital pins 8 - 11 of the Arduino.

 The motor should revolve one revolution in one direction, then
 one revolution in the other direction.


 Created 11 Mar. 2007
 Modified 30 Nov. 2009
 by Tom Igoe

 */

#include <Stepper.h>

const int stepsPerRevolution = 200;  // change this to fit the number of steps per revolution
// for your motor

// initialize the stepper library on pins 8 through 11:
Stepper myStepper(stepsPerRevolution, 8, 9, 10, 11);

void setup() {
  // set the speed at 60 rpm:
  myStepper.setSpeed(5);
  // initialize the serial port:
  Serial.begin(9600);
}

void getAngles(int *angles){
  int num = 0;
  if (Serial.available()){
    num = Serial.parseInt();
    //nums[i] = (int)chars[i] - 48;
    //Serial.println("hello");
    //Serial.println(nums[i]);
    //Serial.println(nums[i]);
    //Serial.println((int)nums[i]-48);
    //i++;
  }
  
  angles[0] = num/100;
  angles[1] = num%100;
}

void moveStepper(int a){
  if(a>0){
    while(a>0){
      a--;
      myStepper.step(1);
      //delay(100);
    }
  }

  if(a<0){
    while(a<0){
      a++;
      myStepper.step(-1);
      //delay(100);
    }
  }
  
}

int angles[2];

void loop() {
  // step one revolution  in one direction:

  if (Serial.available()) {
    getAngles(angles);
  }
  //Serial.println(angles[0]);
  //Serial.println(angles[1]);
  Serial.println("Clockwise");
  Serial.println(angles[0]);
  moveStepper(angles[0]);
  delay(1000);
  Serial.println("Anticlockwise");
  Serial.println(angles[1]);
  moveStepper(-1*angles[1]);
  delay(1000);
  //myStepper.step(30);
  //Serial.println(angle1);
  //myStepper.step(angle1);
  //delay(500);

  // step one revolution in the other direction:
  //Serial.println("counterclockwise");
  //myStepper.step(25);
  //delay(500);
}
