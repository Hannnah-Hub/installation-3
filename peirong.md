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
}这个代码内容改一下 接口6是灯带 需要原本是呼吸灯的状态 距离小于50cm时熄灭 灯环接口是7 需要在距离小于50cm时 亮度从0开始亮起 两个颜色都是白色 然后超声波距离传感器用这个逻辑：// 定义引脚
const int trigPin = 9;  // Trig引脚连接Arduino的9号引脚
const int echoPin = 10; // Echo引脚连接Arduino的10号引脚

// 定义时间变量
long duration;
int distance;

void setup() {
  // 设置Trig引脚为输出
  pinMode(trigPin, OUTPUT);
  
  // 设置Echo引脚为输入
  pinMode(echoPin, INPUT);
  
  // 初始化串口通信
  Serial.begin(9600);
}

void loop() {
  // 确保Trig引脚为低电平
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  
  // 发送超声波脉冲，Trig引脚高电平持续10微秒
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // 读取Echo引脚返回的高电平时间
  duration = pulseIn(echoPin, HIGH);
  
  // 根据时间计算距离，声速约为343米/秒，换算为每微秒0.0343厘米
  distance = duration * 0.0343 / 2;
  
  // 将距离值打印到串口监视器
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");
  
  // 延时200毫秒再读一次
  delay(200);
}
