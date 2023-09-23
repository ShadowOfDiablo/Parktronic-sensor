#include <Adafruit_NeoPixel.h>
#include <NewPing.h>
#include <Olimex_LED_Matrix_L.h>
#include <Olimex_Buzzer.h>
#include <math.h>

#define LED_LATCH 11
#define LED_DATA 16
#define LED_CLOCK 15
// led matrix pins
#define US_TX 8
#define US_RX 4
#define MAX_DISTANCE   200
#define PIN A4

Olimex_LED_Matrix_L Matrix(LED_LATCH,LED_DATA,LED_CLOCK);
Adafruit_NeoPixel strip = Adafruit_NeoPixel(1, PIN, NEO_GRB + NEO_KHZ800);
Olimex_Buzzer Buzz(6);

NewPing Sonar(US_TX, US_RX, MAX_DISTANCE);
int R=0, G=0, B=0;
int i,rows,Data;

void SetPixel (int Red, int Green, int Blue)
{
  strip.setPixelColor(0, strip.Color(Red, Green, Blue));
  strip.show();
  delay(3);
}
void HSL_to_RGB (int H, float S)
{
  // Lightness set const to 50%
  int R, G, B;
  float C, X; 
  C = S;
  X = C * (1 - abs(((float)H / 60.0) / 2 - 1) );

  if ((H>=0) && (H<=60))
  {
    R = C * 255;
    G = X * 255;
  }

  if ((H>=60) && (H<120))
  {
    R = X * 255;
    G = C * 255;
  }

  if ((H>=120) && (H<180))
  {
    G = C * 255;
    B = X * 255;
  }

  if ((H>=180) && (H<240))
  {
    G = X * 255;
    B = C * 255;
  }

  if ((H>=240) && (H<300))
  {
    B = C * 255;
    R = X * 255;
  }

  if ((H>=300) && (H<360))
  {
    B = X * 255;
    R = C * 255;
  }

  SetPixel (R, G, B);
}
void setup()
{
  Serial.begin(115200);
  strip.begin();
  strip.show(); // Initialize all pixels to 'off'
  for (int i=0; i<255; i++)
    SetPixel (R, G, B);
  delay(100);
}

int Sample;
int Button = 1;
int Buzz_ON = 0;
int Duration;
void loop()
{
  
  static int Set_LED = 1;
  Sample = Sonar.ping_cm ();
  Serial.print(Sample);
  Serial.println(" cm");
  Matrix.Clear();
  rows =(int) (Sample  / 8);

    for(i = 0; i < rows;i++){
      Matrix.Line(0,i,7,i);;
    }
  if(Sample > 100){
    Buzz.Mute();
    SetPixel(0,100,0);
    Serial.print("Green\n");
     Matrix.UpdateMatrix();
  }
  if(Sample < 100 && Sample > 30){
    Buzz.Mute();
    SetPixel(100,100,0);
    Serial.print("Yellow\n");
     Matrix.UpdateMatrix();
  }
  if(Sample < 30 && Sample > 10){
    Buzz.Mute();
    SetPixel(100,0,0);
    Serial.print("Red\n");
     Matrix.UpdateMatrix();
  }
  if(Sample < 10){
    Buzz.Mute();
    Buzz.Note (200, 1);
    Serial.print("Buzzer\n");
     Matrix.UpdateMatrix();
  }
  
  if (Buzz_ON)
  {
    if (Sample < 50)
    {
      Duration = 200 - (Sample * 3);
      Buzz.Note (880, Duration);
    }
    else
    {
      Duration = 50;
      Buzz.Note (440, Duration);
    }
  }
  delay(200 - Duration);
}
