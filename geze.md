2个距离传感器 距离都小于20cm的时候 打印1  
先试 步进电机代码 和 超声波传感器代码  
再合到一起  
下面是超声波传感器代码
```C++
const int trigPin1 = 9;  // 第一个传感器的 Trig 引脚
const int echoPin1 = 8;  // 第一个传感器的 Echo 引脚
const int trigPin2 = 7;  // 第二个传感器的 Trig 引脚
const int echoPin2 = 6;  // 第二个传感器的 Echo 引脚

long duration1, distance1;  
long duration2, distance2;  

void setup() {
  Serial.begin(9600);
  
  pinMode(trigPin1, OUTPUT);
  pinMode(echoPin1, INPUT);
  pinMode(trigPin2, OUTPUT);
  pinMode(echoPin2, INPUT);
}

void loop() {
  digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin1, LOW);
  duration1 = pulseIn(echoPin1, HIGH);
  distance1 = duration1 * 0.034 / 2;

  digitalWrite(trigPin2, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin2, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin2, LOW);
  duration2 = pulseIn(echoPin2, HIGH);
  distance2 = duration2 * 0.034 / 2;


  Serial.print("Distance 1: ");
  Serial.print(distance1);
  Serial.print(" cm, Distance 2: ");
  Serial.print(distance2);
  Serial.println(" cm");

  if (distance1 < 20 && distance2 < 20) {
    Serial.println(1);
  }

  delay(1000);
}
```
步进电机代码 4096步
```C++
#include <Stepper.h>

// 定义步进电机的步数和引脚
const int stepsPerRevolution = 4096;  // 4096 步对应电机一圈
const int motorPin1 = 8;  // 电机输入引脚 1
const int motorPin2 = 9;  // 电机输入引脚 2
const int motorPin3 = 10; // 电机输入引脚 3
const int motorPin4 = 11; // 电机输入引脚 4

// 创建步进电机对象
Stepper myStepper(stepsPerRevolution, motorPin1, motorPin3, motorPin2, motorPin4);

void setup() {
  // 设置电机速度（步/分钟）
  myStepper.setSpeed(10);  // 调整速度，10 是比较慢的转速
}

void loop() {
  // 持续正转 4096 步（电机一圈）
  myStepper.step(stepsPerRevolution);

  // 如果需要反转，可以使用负数
  // myStepper.step(-stepsPerRevolution);

  // 如果只想持续转动不需要停止，可以去掉这个延时
  ```
