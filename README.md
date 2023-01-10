# Helping-machine-for-a-blind-human
When it rains the motor vibrates and if he push
One button, he can call a number we added to 
the code...and If there is an obstacle 0-30 cm 
Distance from the man the buzzer will sound...

#include <TinyGPS++.h>
#include <SoftwareSerial.h>

static const int RX=4,TX=3;
static const uint32_t GPSBaud = 9600;
SoftwareSerial mySerial(11,12);
TinyGPSPlus gps;
SoftwareSerial ss(RX,TX);
void setup(){ 
    mySerial.begin(9600);
    ss.begin(GPSBaud);
    pinMode(6,INPUT);
    pinMode(7,OUTPUT);
    pinMode(5,OUTPUT);
    pinMode(2,INPUT);
    pinMode(9,OUTPUT); 
    pinMode(10,OUTPUT);
    pinMode(8,INPUT);
   
} 

void loop(){
    
    
    
    
digitalWrite(5,LOW);
delayMicroseconds(2);
digitalWrite(5,HIGH);
delayMicroseconds(10);
digitalWrite(5,LOW);

long duration = pulseIn(2,HIGH);
long distance = duration * 0.034 /2 ;

bool y = digitalRead(8);
    
if ((distance <= 30) and (y==1)) {
   digitalWrite(7,HIGH);
   digitalWrite(9,HIGH);
   digitalWrite(10,LOW);
   delay(100);
        
   digitalWrite(7,HIGH);     
   digitalWrite(9,LOW);
   digitalWrite(10,LOW);
   delay(100); 
}

if ((distance <= 30) and (y==0)) {
   digitalWrite(7,HIGH);
   digitalWrite(9, LOW);
   digitalWrite(10,LOW);
}

if ((distance > 30) and (y==1)) {
   digitalWrite(7, LOW);
   digitalWrite(9, HIGH);
   digitalWrite(10,LOW);
   delay(100);
   
   digitalWrite(7,LOW);     
   digitalWrite(9,LOW);
   digitalWrite(10,LOW);
   delay(100); 
  
}
if ((distance > 30) and (y==0)) {
   digitalWrite(7, LOW);
   digitalWrite(9, LOW);
   digitalWrite(10,LOW);
}

    
if(Serial.available() > 0){
        
while (ss.available()>0){
            
gps.encode(ss.read());
            
if((digitalRead(6)==HIGH) and (gps.location.isUpdated())){
                
mySerial.println("ATD+94765371313;");       
delay(1000);         
mySerial.println("AT+CMGF=1");
delay(1000);
mySerial.println("AT+CMGS=\"+94765371313");   
delay(1000);
                
mySerial.print("Latitude ="); 
mySerial.println(gps.location.lat(),6);     
                
mySerial.print("Longitude ="); 
mySerial.println(gps.location.lng(),6);        
delay(1000);
}
}
}   
}
