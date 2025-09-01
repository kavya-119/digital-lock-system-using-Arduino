# digital-lock-system-using-Arduino
const int buttons[4] = {2, 3, 4, 5};   
const int greenLED = 12, redLED = 13;

// Passcode = 1-3-2-4
int passcode[4] = {1, 3, 2, 4};
int input[4], idx = 0;

void setup() {
  for (int i = 0; i < 4; i++)
  pinMode(buttons[i], INPUT_PULLUP);
  pinMode(greenLED, OUTPUT);
  pinMode(redLED, OUTPUT);
  Serial.begin(9600);
}

void loop() {
for (int i = 0; i < 4; i++) {
if (digitalRead(buttons[i]) == LOW) {   
delay(250);                           
input[idx++] = i + 1;                 
Serial.print("Pressed: "); 
  Serial.println(i + 1);
  
if (idx == 4) {                       
 if (checkPass()) {
 Serial.println("Access Granted");
blink(greenLED);
} else {
Serial.println("Access Denied");
 blink(redLED);
}
idx = 0;  
}
}
}
}

bool checkPass() {
  for (int i = 0; i < 4; i++) {
    if (input[i] != passcode[i]) 
      return false;
  }
  return true;
}

void blink(int led) {
  for (int i = 0; i < 3; i++) {
    digitalWrite(led, HIGH); delay(300);
    digitalWrite(led, LOW);  delay(300);
  }
}
