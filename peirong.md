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
  distance = (duration / 2) / 29.1;
  Serial.println(distance);
  

  if (distance < DISTANCE_THRESHOLD) {

    for (int i = 0; i < NUM_LEDS; i++) {
      strip.setPixelColor(i, strip.Color(255, 0, 0));  // 红色
    }
    strip.show();
  } else {
    strip.clear();
    strip.show();
  }
  delay(100);  
}
```
