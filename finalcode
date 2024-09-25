#include <Vex.h>

Gyro g;
Vex Robot;

int triggerPin = 11;
int echoPin = 10;
Ultrasonic us(triggerPin, echoPin);

Adafruit_DCMotor *motorA = Robot.setMotor(4);
Adafruit_DCMotor *motorB = Robot.setMotor(1);

double initial;
double adjust;
int iteration;

void setup() {
  Serial.begin(9600);
  Robot.begin();
  us.begin();
  g.begin();
}
void moveForward(double initial, double time) {
  iteration = 0;
  while (us.checkUltrasonic() > 5) {
    iteration += 1;
    if (iteration >= 35) {
      break;
    }
    Robot.moveTank(motorA, motorB, -50, 50, 0.25);
    adjustAngle(initial, 3, 0);
  }
  Robot.moveTank(motorA, motorB, 20, -20, time);
  adjustAngle(initial, 1, 0);
}

void adjustAngle(double initial, int triggerAngle, int turnAngle) {
  adjust = g.getZAngle() - turnAngle;
  while (abs(initial - adjust) > triggerAngle) {
    if ((initial - adjust) > triggerAngle) {
      while ((initial - adjust) > 1) {
        Robot.moveTank(motorA, motorB, -40, -40, 0.01);
        adjust = g.getZAngle() - turnAngle;
      }
    } else if ((initial - adjust) < -triggerAngle) {
      while ((initial - adjust) < -1) {
        Robot.moveTank(motorA, motorB, 40, 40, 0.01);
        adjust = g.getZAngle() - turnAngle;
      }
    }
  }
}

void loop() {
  // First move
  initial = g.getZAngle();
  moveForward(initial, 1.8);

  // First turn
  Robot.moveTank(motorA, motorB, 50, 50, 1.5);
  delay(500);
  adjustAngle(initial, 1, -90);

  // Second move
  initial = g.getZAngle();
  for (int i = 1; i <= 22; ++i) {
    Robot.moveTank(motorA, motorB, 50, -50, 0.25);
    delay(300);
    adjustAngle(initial, 3, 0);
  }
  adjustAngle(initial, 1, 0);

  // Placing rock
  delay(3000);

  // Third move
  moveForward(initial, 0.6);

  // Second turn
  g.begin();
  initial = g.getZAngle();
  Robot.moveTank(motorA, motorB, 50, 50, 1.5);
  delay(500);
  adjustAngle(initial, 1, -90);

  // Fourth move
  initial = g.getZAngle();
  moveForward(initial, 2.3);

  // Third turn
  initial = g.getZAngle();
  Robot.moveTank(motorA, motorB, -50, -50, 0.75);
  delay(500);
  adjustAngle(initial, 1, 45);

  // Climbing second peak
  Robot.moveTank(motorA, motorB, 55, - 55, 8.0);

  // Fourth turn
  delay(500);
  initial = g.getZAngle();
  Robot.moveTank(motorA, motorB, 50, 50, 1.5);
  delay(500);
  adjustAngle(initial, 1, -90);

  // Climbing third peak
  Robot.moveTank(motorA, motorB, -55, 55, 4.0);
  Robot.end();
}

