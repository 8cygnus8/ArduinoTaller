# ArduinoTaller
//Codigo para Taller en C ucaramacara Comocs

const int sPIR = 10;

int mov = 0; 

int trig_pin = 8;
int echo_pin = 9;
long echotime; 
float distance; 
int state='g';
int led= 11;

void setup() {
  Serial.begin(9600);
  pinMode(sPIR,INPUT);
  Serial.println("PIR Motion | Sensor de Movimiento IR");
  Serial.println(" ");
   Serial.print("*D"+String(echotime)+"*");

   pinMode(trig_pin, OUTPUT); 
  pinMode(echo_pin, INPUT);
  digitalWrite(trig_pin, LOW); //Start with trigger LOW
  pinMode(led,OUTPUT);

}

void loop() {
  if(Serial.available() > 0){ 
    state = Serial.read(); 
  }
  if(state=='a') {
    
  digitalWrite(trig_pin, LOW);
  delayMicroseconds(2);
  digitalWrite(trig_pin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trig_pin, LOW);

  //get the result
  echotime= pulseIn(echo_pin, HIGH);
  
  distance= 0.0001*((float)echotime*340.0)/2.0;
  Serial.print("*D"+String(distance)+"*");

  }
  else if(state=='g') {
    
  }
  
  mov = digitalRead(sPIR);
  delay(50);
  
  if(state=='b'){
    digitalWrite(led,HIGH);
    
  }
  else{
    digitalWrite(led,LOW);
    
  }
  if (mov == HIGH) {
  Serial.print("*P"+String(mov)+"*");
    Serial.print(mov); Serial.print(" : "); Serial.println("Movimiento detectado"); 
     mov = 0;
    
  }
  
  else {
    Serial.print(mov); Serial.print(" : "); Serial.println("No se detecta movimiento");
    Serial.print("*P"+String(mov)+"*");
    
    }

}
