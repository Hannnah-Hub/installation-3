# installation-3
## 第一节课 7.31
### 代码
```C++
int ledPin = 9; // 选择PWM引脚9

void setup() {
  pinMode(ledPin, OUTPUT); // 设置引脚为输出
}

void loop() {
  for (int brightness = 0; brightness <= 255; brightness++) { 
    analogWrite(ledPin, brightness); // 设置PWM值，控制亮度
    delay(10); // 每次变化后等待10毫秒
  }

  for (int brightness = 255; brightness >= 0; brightness--) {
    analogWrite(ledPin, brightness); 
    delay(10); 
  }
}
```
当光暗时，led灯亮起
```C++
int sensorPin = A0;    // AO引脚连接到模拟输入A0
int ledPin = 13;       // LED连接到数字输出引脚13
int threshold = 150;   // 设定的电压阈值

void setup() {
  pinMode(ledPin, OUTPUT);  // 设置LED引脚为输出模式
  Serial.begin(9600);       // 初始化串口通信，波特率设为9600
}


void loop() {
  int sensorValue = analogRead(sensorPin);  // 读取传感器的AO值
  Serial.println(sensorValue); // 打印传感器值到串口监视器
  if (sensorValue < threshold) {            // 如果电压值低于阈值
    digitalWrite(ledPin, HIGH);             // 点亮LED
  } else {
    digitalWrite(ledPin, LOW);              // 熄灭LED
  }  
  delay(500); // 每隔500毫秒读取一次
}
```
当距离改变，舵机角度改变
```C++
#include <Servo.h>

// 定义超声波传感器的引脚
const int trigPin = 9;
const int echoPin = 10;

// 创建舵机对象
Servo myServo;

void setup() {
  // 初始化串口监视器
  Serial.begin(9600);

  // 设置超声波传感器的引脚模式
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  // 将舵机连接到数字引脚3，因为3~是一个PWM接口 帮助舵机平滑的转动
  myServo.attach(3);
  myServo.write(0); // 设置初始位置为0度
}

void loop() {
  // 发送超声波信号
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // 读取回声信号持续时间
  long duration = pulseIn(echoPin, HIGH);

  // 计算距离 (单位: 厘米)
  int distance = duration * 0.034 / 2;

  // 打印距离到串口监视器
  Serial.print("距离: ");
  Serial.print(distance);
  Serial.println(" cm");

  // 根据距离调整舵机的角度
  int angle = map(distance, 20, 200, 0, 180);
  angle = constrain(angle, 0, 180); // 限制角度范围在0到180度之间
  myServo.write(angle);

  // 延迟一段时间再进行下一次测量
  delay(100);
}
```

当距离大于50cm时，舵机旋转180度
```C++
#include <Servo.h>

// 定义超声波传感器的引脚
const int trigPin = 9;
const int echoPin = 10;

// 创建舵机对象
Servo myServo;

void setup() {
  // 初始化串口监视器
  Serial.begin(9600);

  // 设置超声波传感器的引脚模式
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  // 将舵机连接到数字引脚3
  myServo.attach(3);
  myServo.write(0); // 设置初始位置为0度
}

void loop() {
  // 发送超声波信号
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // 读取回声信号持续时间
  long duration = pulseIn(echoPin, HIGH);

  // 计算距离 (单位: 厘米)
  int distance = duration * 0.034 / 2;

  // 打印距离到串口监视器
  Serial.print("距离: ");
  Serial.print(distance);
  Serial.println(" cm");

  // 根据距离调整舵机的角度
  int angle;
  if (distance > 50) {
    angle = 180;
  } else {
    angle = 0;
  }
  
  myServo.write(angle);

  // 延迟一段时间再进行下一次测量
  delay(100);
}
```
