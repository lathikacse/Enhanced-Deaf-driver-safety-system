# Enhanced-Deaf-driver-safety-system
//#include <ESP8266WiFi.h>
//#include <WiFiClient.h> 
//#include <ESP8266WebServer.h>
//#include <ESP8266HTTPClient.h>
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27, 16, 2);
#include <Wire.h>
const int trigPin = 5;
const int echoPin = 6;
#define SOUND_VELOCITY 0.034
#define CM_TO_INCH 0.393701
//const char* ssid = "SMART-G";
//const char* password = "10112019"; 
long duration;
float distanceCm;
float distanceInch;
int i=0;
int j=0;
void setup() 
{
 pinMode(3,OUTPUT);
digitalWrite(3,LOW); 
lcd.init();
lcd.backlight();
lcd.clear();
lcd.setCursor(0,0);
lcd.print("  DEAF DRIVING    ");
lcd.setCursor(0,1);
lcd.print("    SYSTEM");
delay(3000);
Serial.begin(9600); 
pinMode(trigPin, OUTPUT); 
pinMode(echoPin, INPUT); 
//WiFi.mode(WIFI_OFF);     

//lcd.clear();
//lcd.setCursor(0,0);
//lcd.print("UN:SMART-G");
//lcd.setCursor(0,1);
//lcd.print("PSW:10112019");
//WiFi.mode(WIFI_STA);  
//WiFi.begin(ssid, password);     
//Serial.println(""); 
//Serial.print("Connecting");
//while (WiFi.status() != WL_CONNECTED) 
//  {
//    delay(100);
//    Serial.print("*");
//  } 
//  Serial.println("");
//  Serial.print("Connected to ");
//  Serial.println(ssid);
//  Serial.print("IP address: ");
//  Serial.println(WiFi.localIP()); 
}

void loop() {
    int MOI =  (analogRead(A0));
    //Serial.println(MOI);
    if(MOI>=585)
    {
      j++;
      }
    
  // Clears the trigPin
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration = pulseIn(echoPin, HIGH);
  
  // Calculate the distance
  distanceCm = duration * SOUND_VELOCITY/2;
  
  // Convert to inches
  distanceInch = distanceCm * CM_TO_INCH;
  
//  // Prints the distance on the Serial Monitor
//  Serial.print("Distance (cm): ");
//  Serial.println(distanceCm);
//  Serial.print("Distance (inch): ");
//  Serial.println(distanceInch);
  
 

  if(distanceCm>10)
  {i=0;
digitalWrite(3,LOW);
lcd.setCursor(0,0);
lcd.print("NO VEHICLE***     ");
lcd.setCursor(0,1);
lcd.print("GO------>          ");
//digitalWrite(3,LOW); 
//delay(1000);
    
    }


   if(distanceCm<=10)
  {
digitalWrite(3,HIGH); 
lcd.setCursor(0,0);
lcd.print("VEHICLE DETECT    ");
Serial.print("VEHICLE DETECT");
lcd.setCursor(0,1);

if(j>=10)
{
lcd.print("Emergency       ");
Serial.print("Emergency");
j=0;
delay(2000);
}
else
{
  lcd.print("****");
  delay(1000);
  }
i++;
//if(i==1)
//{
////http://sms.creativepoint.in/api/push.json?apikey=6555c521622c1&route=transsms&sender=FSSMSS&mobileno=9597078051&text=Dear customer your msg is amt 30 Sent By FSMSG FSSMSS
//HTTPClient http;  
//String postData;  
//postData = "apikey=6555c521622c1&route=transsms&sender=FSSMSS&mobileno=9488184056&text=Dear customer your msg is VEHICLE DETECT GO SLOW Sent By FSMSG FSSMSS";  
//http.begin("http://sms.creativepoint.in/api/push.json?");            
//http.addHeader("Content-Type", "application/x-www-form-urlencoded");
//int httpCode = http.POST(postData);   
//String payload = http.getString();
//Serial.println(payload); 
//http.end();    
//delay(1000);
//}

}   
}
