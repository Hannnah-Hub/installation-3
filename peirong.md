peiro
```C++

#include <Adafruit_NeoPixel.h>

#define TRIG_PIN 9
#define ECHO_PIN 8
#define LED_PIN 6
#define NUM_LEDS 24 
#define DISTANCE_THRESHOLD 50

Adafruit_NeoPixel strip = Adafruit_NeoPixel(NUM_LEDS, LED_PIN, NEO_GRB + NEO_KHZ800);

void setup() {
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  strip.begin();
  strip.show();  
  Serial.begin(9600);  
}

void loop() {
  long duration, distance;
  
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  duration = pulseIn(ECHO_PIN, HIGH);
  distance = duration * 0.034 / 2;

  Serial.println(distance);
  

  if (distance < DISTANCE_THRESHOLD) {

    for (int i = 0; i < NUM_LEDS; i++) {
      strip.setPixelColor(i, strip.Color(255, 0, 0));  
    }
    strip.show();
  } else {
    strip.clear();
    strip.show();
  }
  delay(100);  
}
```
led灯环和灯带的连接
```C++
#include <Adafruit_NeoPixel.h>

#define TRIG_PIN 9
#define ECHO_PIN 10
#define LED_STRIP_PIN 6
#define RING_PIN_BASE 7
#define STRIP_LENGTH 30
#define RING_LED_COUNT 12
#define RING_COUNT 5

Adafruit_NeoPixel ledStrip = Adafruit_NeoPixel(STRIP_LENGTH, LED_STRIP_PIN, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel rings[RING_COUNT];

void setup() {
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);

  ledStrip.begin();
  ledStrip.show();
  initialStripOn();

  for (int i = 0; i < RING_COUNT; i++) {
    rings[i] = Adafruit_NeoPixel(RING_LED_COUNT, RING_PIN_BASE + i, NEO_GRB + NEO_KHZ800);
    rings[i].begin();
    rings[i].show();
  }

  Serial.begin(9600);
}

void loop() {
  float distance = measureDistance();

  Serial.print("距离: ");
  Serial.print(distance);
  Serial.println(" cm");

  if (distance < 50) {
    allOff(ledStrip);
    sequentialRingOn();
  } else {
    initialStripOn();
    allRingsOff();
  }

  delay(500);
}

float measureDistance() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  long duration = pulseIn(ECHO_PIN, HIGH);
  float distance = duration * 0.034 / 2;
  return distance;
}

void initialStripOn() {
  for (int i = 0; i < STRIP_LENGTH; i++) {
    ledStrip.setPixelColor(i, ledStrip.Color(255, 0, 0));
  }
  ledStrip.show();
}

void sequentialRingOn() {
  for (int i = 0; i < RING_COUNT; i++) {
    for (int j = 0; j < RING_LED_COUNT; j++) {
      rings[i].setPixelColor(j, rings[i].Color(0, 255, 0));
    }
    rings[i].show();
    delay(300);
  }
}

void allOff(Adafruit_NeoPixel &strip) {
  for (int i = 0; i < strip.numPixels(); i++) {
    strip.setPixelColor(i, strip.Color(0, 0, 0));
  }
  strip.show();
}

void allRingsOff() {
  for (int i = 0; i < RING_COUNT; i++) {
    allOff(rings[i]);
  }
}
```
