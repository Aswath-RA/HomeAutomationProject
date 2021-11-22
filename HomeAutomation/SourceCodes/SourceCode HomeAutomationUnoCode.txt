



//********************************************************************************************************
//Home Automation Project Contains two main components Esp32 and uno 
//In this file we will see about Uno coding used in this project,I hope you guys like the project :-)
// For any doubt contact @ :aswath.raviji@gmail.com
//$$$$$ Let Your Code Conquer The World $$$$$
//*********************************************************************************************************


#include<SoftwareSerial.h>
#include <Servo.h>               //HeaderFiles For servo and serial


Servo myservo;             
int initial_position = 90;   
int LDR1 = A0;          //connect The LDR1 on Pin A0 ---- solar
int LDR2 = A1;          //Connect The LDR2 on pin A1 ----- solar
int error = 5;          
int servopin=3; 
int sensorPin= A2;  //automatic on and off  -- A2
int sensorValue = 0;
int ledo = 7;       //automatic on and off  -- orange 12v 
int ledb = 8;      //automatic on and off  -- blue 12v 
int in = 2;        //physcial intrusion ldr 
int LEDr = 4;      // ledred
int LEDb = 5;      // ledblue
int BUZZ = 6;      //buzzer

void setup() {
  pinMode(in,INPUT);
  pinMode(ledo, OUTPUT);    //Setting pins as output and low
  pinMode(ledb, OUTPUT);
  digitalWrite(ledo,LOW);
  digitalWrite(ledb,LOW);
  pinMode(LEDr,OUTPUT);
  pinMode(LEDb,OUTPUT);
  pinMode(BUZZ,OUTPUT);
  myservo.attach(servopin);  
  pinMode(LDR1, INPUT);   
  pinMode(LDR2, INPUT);
  myservo.write(initial_position);   //Move servo at 90 degree
  delay(2000);
  Serial.begin(9600);
}

void loop() {
  // put your main code here, to run repeatedly:

 int R1 = analogRead(LDR1); // read  LDR 1
  int R2 = analogRead(LDR2); // read  LDR 2
  int diff1= abs(R1 - R2);   
  int diff2= abs(R2 - R1);
  Serial.println(R1);
  Serial.println(R2);
  if((diff1 <= error) || (diff2 <= error)) {    
  } else {    
    if(R1 > R2)
    {
      initial_position = --initial_position;  
    }
    if(R1 < R2) 
    {
      initial_position = ++initial_position; 
    }
  }
  myservo.write(initial_position); 
  delay(100);
sensorValue = analogRead(sensorPin);
 Serial.println(sensorValue);
 if(sensorValue < 100)
 {
   Serial.println("LED light on");
   digitalWrite(ledb,LOW);
   digitalWrite(ledo,HIGH);
 }
 else
 {
  digitalWrite(ledo,LOW);
  digitalWrite(ledb,HIGH);
 }
if (digitalRead(in)==HIGH){
  digitalWrite(LEDb,LOW);
  digitalWrite(LEDr,HIGH);
  digitalWrite(BUZZ,HIGH);
}
else
{
   digitalWrite(LEDr,LOW);
  digitalWrite(BUZZ,LOW); 
  digitalWrite(LEDb,HIGH);
}
delay(1000);
}
//*******************************************************************************************
