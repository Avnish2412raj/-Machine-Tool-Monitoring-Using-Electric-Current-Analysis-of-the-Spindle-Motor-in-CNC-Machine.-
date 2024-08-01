#include <DHT.h>
#include <DHT_U.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// Hall sensor pins
const int hallSensorPin_ch2 = A0;  // A Connect the signal pin of the Hall effect sensor to digital pin 2
const int hallSensorPin_ch1 = A1;  //D

// DHT11 sensor pins
#define DHTPIN 3     // what pin we're connected to

// Variables
int sensorValue_ch1 =0 ;
int sensorValue_ch2 =0 ;



// Uncomment whatever type you're using!
   #define DHTTYPE DHT11   // DHT 11 
//#define DHTTYPE DHT22   // DHT 22  (AM2302)
//#define DHTTYPE DHT21   // DHT 21 (AM2301)

// Initialize DHT sensor for normal 16mhz Arduino
DHT dht(DHTPIN, DHTTYPE);
LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  Serial.begin(115200);       // Initialize serial communication
  dht.begin();
  pinMode(hallSensorPin_ch2, INPUT); // Set the Hall effect sensor pin as input
  pinMode(hallSensorPin_ch1, INPUT); 
  Serial.println("INIT");

  lcd.init();
  
  // Turn on the backlight
  lcd.backlight();
  
  // Print a message to the LCD
  


}

void loop()
 {
    // Wait a few seconds between measurements.
  delay(2000);

  // ____________________Hall sensor____________________ 
   sensorValue_ch1 = analogRead(hallSensorPin_ch1); // Read the digital value from the sensor
   sensorValue_ch2 = analogRead(hallSensorPin_ch2);
  Serial.print("sensorValue_ch1 = ");
  Serial.println(sensorValue_ch1);
  Serial.print("sensorValue_ch2 = ");
  Serial.println(sensorValue_ch2);
  // if (sensorValue > 0) 
  // {
  //   Serial.println("Magnetic field detected!"); // Output a message if magnetic field is detected
  // } else {
  //   Serial.println("No magnetic field."); // Output a message if no magnetic field is detected
  // }
// ____________________Temperature sensor____________________ 

  float h = dht.readHumidity();
  // Read temperature as Celsius
  float t = dht.readTemperature();
  // Read temperature as Fahrenheit
  float f = dht.readTemperature(true);
  
  // Check if any reads failed and exit early (to try again).
  if (isnan(h) || isnan(t) || isnan(f)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }


  Serial.print("Humidity: "); 
  Serial.print(h);
  Serial.print(" %\t");
  Serial.print("Temperature: "); 
  Serial.print(t);
  Serial.print(" *C ");
  Serial.print(f);
  Serial.print(" *F\t");
  Serial.println("");


  lcd.setCursor(0, 0);

  lcd.print("Channel 1 :");
  lcd.print(sensorValue_ch1);
  delay(4000);
  lcd.clear();

  lcd.print("Channel 2 :");
  lcd.print(sensorValue_ch2);
  delay(4000);
  lcd.clear();
  lcd.print("Humidity : ");
  lcd.print(h);
  delay(4000);
  lcd.clear();
  lcd.print("Temperature:");
  lcd.print(t);
  delay(4000);
  lcd.clear();

}
