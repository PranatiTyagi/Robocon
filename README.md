#define D11 50
#define D12 51
#define D21 52
#define D22 53
#define P11 4
#define P12 5
#define P21 6
#define P22 7
#define B11 46
#define B12 47
#define B21 48
#define B22 49
#define jpin 2
#define ap1 A0
int setPoint=35;
float Kp=0.2;
void setup() {
 pinMode(D11,OUTPUT);
 pinMode(D12,OUTPUT);
 pinMode(D21,OUTPUT);
 pinMode(D22,OUTPUT);
 pinMode(P11,OUTPUT);
 pinMode(P12,OUTPUT);
 pinMode(P21,OUTPUT);
 pinMode(P22,OUTPUT);
 pinMode(B11,OUTPUT);   
 pinMode(B12,OUTPUT);
 pinMode(B21,OUTPUT);   
 pinMode(B22,OUTPUT);
 pinMode(jpin,INPUT);

pinMode(jpin, INPUT);
attachInterrupt(digitalPinToInterrupt(jpin), brk, RISING);

}

void loop() {
  digitalWrite(B11,0);
  digitalWrite(B22,0);
 int readVal=analogRead(A0);
 int position = ((float)readVal/921)*70;
 if(position>=25&&position<=45)
 forward();
 else if(position<25)
 left();
 else if(position>45)
 right();
}

int PID()
{
   int readVal=analogRead(A0);
   int position = ((float)readVal/921)*70;
   int error =  setPoint-position;   // Calculate the deviation from position to the set point
   int motorSpeed = Kp * error; // Applying formula of PID
   return motorSpeed;
}

void forward()
{
  analogWrite(P11,100-PID());
  analogWrite(P22,100+PID());
}
void left()
{
  analogWrite(P11,50);
  analogWrite(P22,80+PID());
}
void right()
{
  analogWrite(P22,50);
  analogWrite(P11,80+PID());
}
void brk()
{
  digitalWrite(B11,1);
  digitalWrite(B22,1);
}

