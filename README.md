# installation-3
## 第一节课 7.31
### 光敏电阻 开关led灯
```C++
int sensorPin = A0;    // AO引脚连接到模拟输入A0
int ledPin = 13;       // LED连接到数字输出引脚13
int threshold = 500;   // 设定的电压阈值

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
