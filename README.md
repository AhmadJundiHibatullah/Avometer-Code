# Avometer-Code
// Amperemeter
#include <LiquidCrystal.h>
// sensor amperemeter
int sensor1 = A2;
int sensor2 = A3;
float R1 = 220;
float R2 = 220; 

//Resistansi
int sensor0 = A0;
float Vcc = 5;
float R3 = 1000;

#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup()
{
lcd.init();
Serial.begin(9600);
lcd.begin(16, 2);
lcd.backlight();
lcd.print("Ukur Amp dan Res");
delay(500);
pinMode(sensor0, INPUT);
pinMode(sensor1, INPUT);
pinMode(sensor2, INPUT);
}

void loop()
{
//mengukur arus
float adc1 = analogRead(sensor1);
float adc2 = analogRead(sensor2);
float Vadc1 = (adc1-adc2)*5/1023; 
float I_ukur = Vadc1 /(R1 + R2)*1000;

//mengukur resistansi
float adc0 = analogRead(sensor0);
float Vadc0 = 5 * adc0 / 1024; // tegangan dalam satuan volt
float buffer = (Vcc / Vadc0) - 1;
float R_uji = R3 / buffer;

// menampilkan arus dalam serial
Serial.print("I ukur : ");
Serial.print(I_ukur);
Serial.println(" mA ");
delay(1000);

// menampilkan resistansi dalam serial
Serial.print("R uji : ");
Serial.print(R_uji);
Serial.println("Ohm");
delay(1000);

// menampilkan arus dalam LCD
lcd.clear();
lcd.setCursor(0, 0);
lcd.print("Iukur= ");
lcd.print(I_ukur);
lcd.print("mA");

// menampilkan resistansi dalam LCD
lcd.setCursor(0, 1);
lcd.print("Ruji= ");
lcd.print(R_uji);
lcd.print("Ohm");
  
}
