```C++
// 定义LED连接的引脚
const int ledPin = 9; // 你可以将LED连接到PWM支持的引脚，如9, 10, 11等

void setup() {
  // 设置LED引脚为输出模式
  pinMode(ledPin, OUTPUT);
}

void loop() {
  // 逐步增加亮度
  for (int brightness = 0; brightness <= 255; brightness++) {
    analogWrite(ledPin, brightness); // 使用PWM设置LED的亮度
    delay(10); // 控制亮度增加的速度，数值越大，亮起越慢
  }
  
  // LED保持全亮状态一段时间
  delay(1000); // LED全亮保持1秒
  
  // 逐步降低亮度
  for (int brightness = 255; brightness >= 0; brightness--) {
    analogWrite(ledPin, brightness); // 使用PWM设置LED的亮度
    delay(10); // 控制亮度减少的速度
  }
  
  // LED保持全灭状态一段时间
  delay(1000); // LED全灭保持1秒
}
```
超声波&led
```C++
// 定义超声波传感器的引脚
const int trigPin = 9;  // 连接到Trig引脚
const int echoPin = 10; // 连接到Echo引脚

// 定义LED连接的引脚
const int ledPin = 13;  // 连接到LED的引脚

void setup() {
  // 设置引脚模式
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(ledPin, OUTPUT);
  
  // 初始化串口通讯，用于调试（可选）
  Serial.begin(9600);
}

void loop() {
  // 发送超声波脉冲
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // 接收回声脉冲
  long duration = pulseIn(echoPin, HIGH);
  
  // 计算距离（厘米）
  long distance = duration * 0.034 / 2;
  
  // 打印距离到串口监视器（用于调试，可选）
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");
  
  // 如果距离小于20厘米，则点亮LED，否则熄灭LED
  if (distance < 20) {
    digitalWrite(ledPin, HIGH); // LED亮
  } else {
    digitalWrite(ledPin, LOW); // LED灭
  }
  
  // 延时50毫秒，以防止连续测量干扰
  delay(50);
}
```
