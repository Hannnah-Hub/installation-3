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
