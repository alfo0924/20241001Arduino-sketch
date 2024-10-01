bool ledState = false;  // LED的狀態
bool lastButtonState = true;  // 上一次按鈕的狀態

void setup() {
  Serial.begin(9600);
  pinMode(16, INPUT_PULLUP);
  pinMode(4, OUTPUT);
  digitalWrite(4, LOW);
}

void loop() {
  bool p16 = digitalRead(16);
  Serial.println(p16);

  // 檢測按鈕是否被按下（從高電平變為低電平）
  if (lastButtonState == true && p16 == false) {
    ledState = !ledState;  // 切換LED狀態
    digitalWrite(4, ledState ? HIGH : LOW);
  }

  lastButtonState = p16;
  delay(50);  // 短暫延遲以防止彈跳
}
