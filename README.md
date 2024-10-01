
## sketch_oct1a程式碼解析

這段程式碼主要用於讀取並監控一個數位輸入腳位的狀態。

### setup() 函式

```cpp
void setup() {
  Serial.begin(9600);
  pinMode(16,INPUT_PULLUP);
}
```

在`setup()`函式中，程式執行以下步驟：

1. **初始化串口通訊**：
   - `Serial.begin(9600);` 啟動串口通訊，設定鮑率為9600。
   - 這允許Arduino透過USB連接與電腦進行通訊。

2. **設定腳位模式**：
   - `pinMode(16,INPUT_PULLUP);` 將數位腳位16設定為輸入模式，並啟用內部上拉電阻。
   - 使用上拉電阻模式可以確保腳位在未連接時維持在高電平狀態。

### loop() 函式

```cpp
void loop() {
  bool p16 = digitalRead(16);
  Serial.println(p16);
  delay(500);
}
```

在`loop()`函式中，程式會重複執行以下操作：

1. **讀取腳位狀態**：
   - `bool p16 = digitalRead(16);` 讀取腳位16的當前狀態。
   - 如果腳位為高電平（未按下），`p16`的值為`true`（1）。
   - 如果腳位為低電平（按下），`p16`的值為`false`（0）。

2. **輸出狀態**：
   - `Serial.println(p16);` 將讀取到的狀態透過串口輸出。
   - 這會在串口監視器中顯示0或1。

3. **延遲**：
   - `delay(500);` 暫停程式執行500毫秒（0.5秒）。
   - 這個延遲控制了讀取和輸出的頻率。







##  sketch_oct1b程式碼解析

這段程式碼實現了一個簡單的按鈕控制LED的功能。

### setup() 函式

```cpp
void setup() {
  Serial.begin(9600);
  pinMode(16, INPUT_PULLUP);
  pinMode(4, OUTPUT);
  digitalWrite(4, LOW);
}
```

在`setup()`函式中，程式執行以下步驟：

1. **初始化串口通訊**：
   - `Serial.begin(9600);` 啟動串口通訊，設定鮑率為9600。

2. **設定輸入腳位**：
   - `pinMode(16, INPUT_PULLUP);` 將數位腳位16設定為輸入模式，並啟用內部上拉電阻。
   - 這個腳位將用於連接按鈕。

3. **設定輸出腳位**：
   - `pinMode(4, OUTPUT);` 將數位腳位4設定為輸出模式。
   - 這個腳位將用於控制LED。

4. **初始化LED狀態**：
   - `digitalWrite(4, LOW);` 將腳位4設為低電平，確保LED初始狀態為關閉。

### loop() 函式

```cpp
void loop() {
  bool p16 = digitalRead(16);
  Serial.println(p16);
  if(p16 == 0) {
    digitalWrite(4, HIGH);
  }
  else {
    digitalWrite(4, LOW);
  }
  delay(500);
}
```

在`loop()`函式中，程式會重複執行以下操作：

1. **讀取按鈕狀態**：
   - `bool p16 = digitalRead(16);` 讀取腳位16（按鈕）的當前狀態。
   - 由於使用了上拉電阻，按鈕未按下時讀取值為1，按下時為0。

2. **輸出按鈕狀態**：
   - `Serial.println(p16);` 將讀取到的按鈕狀態透過串口輸出。

3. **控制LED**：
   - 使用if-else語句根據按鈕狀態控制LED：
     - 如果`p16 == 0`（按鈕被按下），則`digitalWrite(4, HIGH);`點亮LED。
     - 否則，`digitalWrite(4, LOW);`關閉LED。

4. **延遲**：
   - `delay(500);` 暫停程式執行500毫秒（0.5秒）。






## sketch_oct1c 按鈕控制 LED 程式碼解析

### 全域變數

```cpp
bool ledState = false;  // LED的狀態
bool lastButtonState = true;  // 上一次按鈕的狀態
```

- `ledState`：追蹤 LED 的當前狀態（開或關）。
- `lastButtonState`：記錄上一次讀取的按鈕狀態，用於檢測狀態變化。

### setup() 函式

```cpp
void setup() {
  Serial.begin(9600);
  pinMode(16, INPUT_PULLUP);
  pinMode(4, OUTPUT);
  digitalWrite(4, LOW);
}
```

1. 初始化串口通訊，設定鮑率為 9600。
2. 設定腳位 16 為輸入上拉模式（用於按鈕）。
3. 設定腳位 4 為輸出模式（用於 LED）。
4. 初始化 LED 為關閉狀態。

### loop() 函式

```cpp
void loop() {
  bool p16 = digitalRead(16);
  Serial.println(p16);

  if (lastButtonState == true && p16 == false) {
    ledState = !ledState;
    digitalWrite(4, ledState ? HIGH : LOW);
  }

  lastButtonState = p16;
  delay(50);
}
```

1. **讀取按鈕狀態**：
   - `bool p16 = digitalRead(16);` 讀取腳位 16 的當前狀態。
   - `Serial.println(p16);` 將按鈕狀態輸出到串口。

2. **檢測按鈕按下**：
   - `if (lastButtonState == true && p16 == false)` 檢查按鈕是否從未按下狀態變為按下狀態。

3. **切換 LED 狀態**：
   - `ledState = !ledState;` 反轉 LED 狀態。
   - `digitalWrite(4, ledState ? HIGH : LOW);` 根據 `ledState` 設定 LED 的開或關。

4. **更新按鈕狀態**：
   - `lastButtonState = p16;` 儲存當前按鈕狀態為下一次循環的上一狀態。

5. **延遲**：
   - `delay(50);` 短暫延遲以防止按鈕彈跳。

**  程式功能總結 **

1. **按鈕控制**：每次按下按鈕時，LED 狀態會切換（開→關或關→開）。
2. **狀態記憶**：使用 `ledState` 變數記住 LED 的當前狀態，即使在多次 `loop()` 執行之間也能保持。
3. **邊緣檢測**：通過比較當前和上一次的按鈕狀態，只在按鈕剛被按下時執行動作，避免持續按住按鈕時重複觸發。
4. **防彈跳**：使用短暫延遲來減少按鈕彈跳造成的誤觸。
5. **狀態監控**：持續通過串口輸出按鈕的當前狀態，方便調試和監控。

這個程式展示了如何實現一個簡單但有效的按鈕控制 LED 的功能，同時考慮了實際應用中可能遇到的問題，如按鈕彈跳和狀態保持。
