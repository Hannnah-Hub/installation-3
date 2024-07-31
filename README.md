# installation-3
## 第一节课 7.31
### 距离远 转动舵机
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
  int angle = map(distance, 20, 200, 0, 180);
  angle = constrain(angle, 0, 180); // 限制角度范围在0到180度之间
  myServo.write(angle);

  // 延迟一段时间再进行下一次测量
  delay(100);
}
```
