void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  pinMode(16,INPUT_PULLUP);
  //PULLUP（上拉）是指在Arduino中使用內部上拉電阻的一種輸入模式
  pinMode(4,OUTPUT);
  digitalWrite(4,LOW);

}

void loop() {
  // put your main code here, to run repeatedly:
bool p16 = digitalRead(16);
//bool p16 = digitalRead(16); 讀取腳位16的當前狀態。
Serial.println(p16);
if(p16 == 0){
digitalWrite(4,HIGH);
}
else{
digitalWrite(4,LOW);
}
delay(500);
}
