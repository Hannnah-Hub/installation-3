
```C++
#include <Servo.h>

// 定义超声波传感器引脚
const int trigPin1 = 9;
const int echoPin1 = 8;
const int trigPin2 = 7;
const int echoPin2 = 6;

long duration1, distance1;
long duration2, distance2;

// 定义舵机引脚和实例
Servo servo1;
Servo servo2;
Servo servo3;

// 定义舵机3是否已经执行过操作的标志
bool servo3Executed = false;

void setup() {
  Serial.begin(9600);
  
  // 初始化超声波传感器引脚
  pinMode(trigPin1, OUTPUT);
  pinMode(echoPin1, INPUT);
  pinMode(trigPin2, OUTPUT);
  pinMode(echoPin2, INPUT);

  // 初始化舵机
  servo1.attach(3); // 舵机1连接到3号引脚
  servo2.attach(5); // 舵机2连接到5号引脚
  servo3.attach(6); // 舵机3连接到6号引脚

  // 设置初始角度
  servo1.write(0);
  servo2.write(0);
  servo3.write(0);
}

void loop() {
  // 读取超声波传感器1的距离
  digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin1, LOW);
  duration1 = pulseIn(echoPin1, HIGH);
  distance1 = duration1 * 0.034 / 2;

  // 读取超声波传感器2的距离
  digitalWrite(trigPin2, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin2, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin2, LOW);
  duration2 = pulseIn(echoPin2, HIGH);
  distance2 = duration2 * 0.034 / 2;

  // 打印距离值，方便调试
  Serial.print("Distance 1: ");
  Serial.print(distance1);
  Serial.print(" cm, Distance 2: ");
  Serial.print(distance2);
  Serial.println(" cm");

  if (distance1 < 20 && distance2 < 20) {
    // 当两个传感器距离都小于20cm时，舵机1从0到180度循环，舵机2从0到45度来回运动
    servo3Executed = false; // 重置舵机3的状态，使其可以再次执行

    Serial.println("Both sensors < 20cm: Moving Servo 1 and Servo 2");

    // 舵机1运动
    for (int pos = 0; pos <= 180; pos += 1) {
      servo1.write(pos); // 舵机1移动
      delay(15);
    }
    for (int pos = 180; pos >= 0; pos -= 1) {
      servo1.write(pos); // 舵机1返回
      delay(15);
    }

    // 舵机2只移动一次
    servo2.write(45);
    delay(500);
    servo2.write(0);
  } 
  else if ((distance1 >= 20 || distance2 >= 20) && !servo3Executed) {
    // 当任意一个传感器距离大于20cm时，舵机3只执行一次
    Serial.println("Either sensor >= 20cm: Moving Servo 3");

    servo3.write(45);
    delay(500);
    servo3.write(0);
    servo3Executed = true; // 标记舵机3已经执行过动作
  }

  delay(1000); // 延时1秒，避免过于频繁触发
}


```
led灯逐渐亮起的逻辑
```C++
#include <Adafruit_NeoPixel.h>

#define LED_PIN    6  // LED环连接的引脚
#define NUM_LEDS   12  // 每组LED灯的数量
#define NUM_GROUPS 6   // 总共有6组LED灯
#define BRIGHTNESS 255 // LED亮度

Adafruit_NeoPixel leds = Adafruit_NeoPixel(NUM_LEDS * NUM_GROUPS, LED_PIN, NEO_GRB + NEO_KHZ800);

unsigned long startTime;
int currentGroup = 0;

void setup() {
  leds.begin();
  leds.setBrightness(BRIGHTNESS);
  leds.show(); // 初始化所有灯为关闭状态
  startTime = millis(); // 记录程序开始时间
}

void loop() {
  unsigned long currentTime = millis();

  // 在代码运行后的15秒开始亮灯
  if (currentTime - startTime >= 15000) {
    // 检查是否是每10秒的时间间隔来点亮下一组灯
    if (currentTime - startTime >= 15000 + currentGroup * 10000) {
      lightUpGroup(currentGroup);
      currentGroup++; // 点亮下一组灯
    }
  }
}

// 点亮一组灯
void lightUpGroup(int group) {
  int startLed = group * NUM_LEDS;
  int endLed = startLed + NUM_LEDS;

  // 将该组的LED灯设置为白色
  for (int i = startLed; i < endLed; i++) {
    leds.setPixelColor(i, leds.Color(255, 255, 255)); // 设置为白色
  }

  leds.show(); // 更新显示
}
```
组合在一起的代码
```C++
#include <Adafruit_NeoPixel.h>
#include <Servo.h>

// 超声波传感器引脚
const int trigPin1 = 9;
const int echoPin1 = 8;
const int trigPin2 = 7;
const int echoPin2 = 6;

long duration1, distance1;
long duration2, distance2;

// 舵机引脚和实例
Servo servo1;
Servo servo2;
Servo servo3;

// 舵机3是否执行的标志
bool servo3Executed = false;

// LED设置
#define LED_PIN    4  // 将LED连接到4号引脚，避免和舵机冲突
#define NUM_LEDS   12  // 每组LED灯的数量
#define NUM_GROUPS 6   // 总共有6组LED灯
#define MAX_BRIGHTNESS 255 // 最大LED亮度

Adafruit_NeoPixel leds = Adafruit_NeoPixel(NUM_LEDS * NUM_GROUPS, LED_PIN, NEO_GRB + NEO_KHZ800);

unsigned long startTime;
int currentGroup = 0;

void setup() {
  // 初始化舵机
  Serial.begin(9600);
  pinMode(trigPin1, OUTPUT);
  pinMode(echoPin1, INPUT);
  pinMode(trigPin2, OUTPUT);
  pinMode(echoPin2, INPUT);

  servo1.attach(3);  // 舵机1连接到3号引脚
  servo2.attach(5);  // 舵机2连接到5号引脚
  servo3.attach(6);  // 舵机3连接到6号引脚
  servo1.write(0);
  servo2.write(0);
  servo3.write(0);

  // 初始化LED
  leds.begin();
  leds.setBrightness(MAX_BRIGHTNESS);
  leds.show(); // 初始化所有灯为关闭状态
  startTime = millis(); // 记录程序开始时间
}

void loop() {
  unsigned long currentTime = millis();

  // 超声波传感器读取和舵机控制逻辑
  readUltrasonicAndControlServos();

  // LED控制逻辑
  if (currentTime - startTime >= 15000) { // 15秒后开始亮灯
    if (currentTime - startTime >= 15000 + currentGroup * 10000) {
      lightUpGroup(currentGroup);
      currentGroup++; // 点亮下一组灯
    }
  }
  
  delay(100); // 避免循环执行过快
}

void readUltrasonicAndControlServos() {
  // 读取超声波传感器1的距离
  digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin1, LOW);
  duration1 = pulseIn(echoPin1, HIGH);
  distance1 = duration1 * 0.034 / 2;

  // 读取超声波传感器2的距离
  digitalWrite(trigPin2, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin2, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin2, LOW);
  duration2 = pulseIn(echoPin2, HIGH);
  distance2 = duration2 * 0.034 / 2;

  // 打印距离值
  Serial.print("Distance 1: ");
  Serial.print(distance1);
  Serial.print(" cm, Distance 2: ");
  Serial.print(distance2);
  Serial.println(" cm");

  // 控制舵机逻辑
  if (distance1 < 20 && distance2 < 20) {
    servo3Executed = false; // 重置舵机3的状态

    // 舵机1运动
    for (int pos = 0; pos <= 180; pos += 1) {
      servo1.write(pos);
      delay(15);
    }
    for (int pos = 180; pos >= 0; pos -= 1) {
      servo1.write(pos);
      delay(15);
    }

    // 舵机2运动
    servo2.write(45);
    delay(500);
    servo2.write(0);

  } else if ((distance1 >= 20 || distance2 >= 20) && !servo3Executed) {
    // 舵机3只执行一次
    servo3.write(45);
    delay(500);
    servo3.write(0);
    servo3Executed = true;
  }
}

// 点亮一组灯并逐渐亮起
void lightUpGroup(int group) {
  int startLed = group * NUM_LEDS;
  int endLed = startLed + NUM_LEDS;

  // 从亮度0逐渐增加到最大亮度
  for (int brightness = 0; brightness <= MAX_BRIGHTNESS; brightness += 5) { // 每次增加5个亮度单位
    for (int i = startLed; i < endLed; i++) {
      int scaledBrightness = map(brightness, 0, MAX_BRIGHTNESS, 0, 255); // 将亮度映射到颜色值
      leds.setPixelColor(i, leds.Color(scaledBrightness, scaledBrightness, scaledBrightness)); // 根据亮度设置颜色
    }
    leds.show(); // 更新显示
    delay(50); // 每步延时50毫秒，控制亮度变化速度
  }
}
```
