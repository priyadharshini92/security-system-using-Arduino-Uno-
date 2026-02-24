//# security-system-using-Arduino-Uno-
//security system using an ultrasonic sensor (HC-SR04) and a buzzer. This system will detect if an object comes within a set distance and trigger an alarm
// Components: Arduino UNO, HC-SR04 Ultrasonic Sensor, Buzzer

// Pin definitions
const int trigPin = 6;   // Trigger pin of HC-SR04
const int echoPin = 5;   // Echo pin of HC-SR04
const int buzzerPin = 9; // Buzzer pin

// Distance threshold in centimeters
const int dangerDistance = 20; // Alarm if object is closer than 20 cm

void setup() {
  Serial.begin(9600);           // Start serial communication
  pinMode(trigPin, OUTPUT);     // Set trigPin as output
  pinMode(echoPin, INPUT);      // Set echoPin as input
  pinMode(buzzerPin, OUTPUT);   // Set buzzerPin as output
}

void loop() {
  long duration;
  int distance;

  // Send ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Read echo time
  duration = pulseIn(echoPin, HIGH, 30000UL); // Timeout after 30ms to avoid lock
  if (duration == 0) {
    Serial.println("No object detected or out of range");
    noTone(buzzerPin);
    delay(200);
    return;
  }

  // Calculate distance in cm
  distance = duration * 0.034 / 2;

  // Display distance
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Security check
  if (distance > 0 && distance <= dangerDistance) {
    tone(buzzerPin, 1000); // Play alarm tone
  } else {
    noTone(buzzerPin);     // Stop alarm
  }

  delay(200); // Small delay for stability
}
