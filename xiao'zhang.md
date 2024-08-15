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
  // 设置引脚模式
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
}

void loop() {
  // 顺时针旋转
  for (int i = 0; i < 512; i++) { // 512步为例
    for (int j = 0; j < 8; j++) {
      digitalWrite(IN1, steps[j][0]);
      digitalWrite(IN2, steps[j][1]);
      digitalWrite(IN3, steps[j][2]);
      digitalWrite(IN4, steps[j][3]);
      delay(10); // 调整延迟以控制转速
    }
  }

  delay(1000); // 等待一秒

  // 逆时针旋转
  for (int i = 0; i < 512; i++) { // 512步为例
    for (int j = 7; j >= 0; j--) {
      digitalWrite(IN1, steps[j][0]);
      digitalWrite(IN2, steps[j][1]);
      digitalWrite(IN3, steps[j][2]);
      digitalWrite(IN4, steps[j][3]);
      delay(10); // 调整延迟以控制转速
    }
  }

  delay(1000); // 等待一秒
}
```