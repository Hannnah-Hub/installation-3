```C++
// 定义引脚
#define IN1 2
#define IN2 3
#define IN3 4
#define IN4 5

// 步进电机步进序列
int steps[8][4] = {
  {1, 0, 0, 0},
  {1, 1, 0, 0},
  {0, 1, 0, 0},
  {0, 1, 1, 0},
  {0, 0, 1, 0},
  {0, 0, 1, 1},
  {0, 0, 0, 1},
  {1, 0, 0, 1}
};

void setup() {
  // 初始化串口通讯
  Serial.begin(9600);
  
  // 设置引脚模式
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
}

void loop() {
  // 顺时针旋转一圈（4096步）
  for (int i = 0; i < 4096; i++) { 
    for (int j = 0; j < 8; j++) {
      digitalWrite(IN1, steps[j][0]);
      digitalWrite(IN2, steps[j][1]);
      digitalWrite(IN3, steps[j][2]);
      digitalWrite(IN4, steps[j][3]);

      // 打印调试信息（可选）
      // Serial.print("Step: ");
      // Serial.print(j);
      // Serial.print(" IN1: ");
      // Serial.print(steps[j][0]);
      // Serial.print(" IN2: ");
      // Serial.print(steps[j][1]);
      // Serial.print(" IN3: ");
      // Serial.print(steps[j][2]);
      // Serial.print(" IN4: ");
      // Serial.println(steps[j][3]);
      
      delayMicroseconds(488); // 使用微秒延时控制速度
    }
  }

  delay(1000); // 等待一秒

  // 逆时针旋转一圈（4096步）
  for (int i = 0; i < 4096; i++) { 
    for (int j = 7; j >= 0; j--) {
      digitalWrite(IN1, steps[j][0]);
      digitalWrite(IN2, steps[j][1]);
      digitalWrite(IN3, steps[j][2]);
      digitalWrite(IN4, steps[j][3]);

      delayMicroseconds(488); // 使用微秒延时控制速度
    }
  }

  delay(1000); // 等待一秒
}

```
