#include <LiquidCrystal.h>
#include <Keypad.h>
#include <Servo.h>

Servo servoMotor;
const int servoAngle0 = 0;
const int servoAngle1 = 95;

const byte ROWS = 4; 
const byte COLS = 4; 

// Tableau de touches
char keys[ROWS][COLS] = {
    {'7','8','9','R'},
    {'4','5','6','C'},
    {'1','2','3','D'},
    {'A','0','V','T'}
};

#define Password_Length 5
char Data[Password_Length + 1] = {0}; 
byte data_count = 0;
char customKey;

bool ProgramStarted = false; 

// Données pré-stockées
const char COMPOSANT_R1000[] = "R1000";
const char COMPOSANTS_R0220[] = "R0220";
const char COMPOSANT_C0002[] = "C0002";
const char COMPOSANT_D1004[] = "D1004";
const char COMPOSANT_T3600[] = "T3600";


const int LED_7 = 4;
const int LED_15 = 3;
const int LED_9 = 2;
const int LED_30 = 1;
const int LED_42 = 0;

// Broches
const byte rowPins[ROWS] = {9, 8, 7, 6}; 
const byte colPins[COLS] = {13, 12, 11, 10}; 

LiquidCrystal lcd(A0, A1, A2, A3, A4, A5);
Keypad customKeypad = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS );

// Fonction pour vérifier
void ScrollLeft() {
  for (int positionCounter = 0; positionCounter < 5; positionCounter++) {
    lcd.scrollDisplayLeft();
    delay(150);}}

void ScrollRight() {
  for (int positionCounter = 0; positionCounter < 5; positionCounter++) {
    lcd.scrollDisplayRight();
    delay(150);}}    


void checkComposant(const char* Composant, int ledPin) {
  if (strcmp(Composant, Data) == 0) {
    digitalWrite(ledPin, HIGH);
    servoMotor.write(servoAngle1);
    lcd.setCursor(1, 0);
    lcd.print("Compartiment Ouvert");
    ScrollLeft();
    ScrollRight();
    lcd.setCursor(1, 1);
    lcd.print(Composant);
    delay(2000);
    digitalWrite(ledPin, LOW);
    servoMotor.write(servoAngle0);
    data_count = 0;
    memset(Data, 0, Password_Length + 1);
    lcd.clear();
    
  }}

   void failedComposant(const char* Composant) {
  if (strcmp(Composant, Data) != 0) {
      lcd.setCursor(1, 0);
      lcd.print("Compartiment introuvable");
      ScrollLeft();
      ScrollRight();
      delay(500);
      data_count = 0;
      memset(Data, 0, Password_Length + 1);
      lcd.clear();
    
  }

}

void setup() {
  lcd.begin(16, 2);
  lcd.display();
  
  servoMotor.attach(5);
  servoMotor.write(servoAngle0);
  pinMode(LED_7, OUTPUT);
  pinMode(LED_15, OUTPUT);
  pinMode(LED_9, OUTPUT);
  pinMode(LED_30, OUTPUT);
  pinMode(LED_42, OUTPUT);  
  
}

void loop() {

    if (!ProgramStarted) {
    char StartKey = customKeypad.getKey();
    if (StartKey == 'A') {
      ProgramStarted = true;
      lcd.setCursor(5,0);
      lcd.print("Hello!");
      lcd.setCursor(2,1);
      lcd.print("CMC EAR201 G1");
      delay(1000);
      lcd.clear();
    }
    return;
  }

  customKey = customKeypad.getKey();

  lcd.setCursor(0, 0);
  lcd.print("Entrez composants");

  if (customKey != 0) {
    
    Data[data_count] = customKey;
    
    lcd.setCursor(data_count, 1);
    lcd.print(Data[data_count]);

    data_count++;
    if (data_count == Password_Length) {
      lcd.clear();

      // Vérification des Composants
      checkComposant(COMPOSANT_R1000, LED_7);
      checkComposant(COMPOSANTS_R0220, LED_15);
      checkComposant(COMPOSANT_C0002, LED_9);
      checkComposant(COMPOSANT_D1004, LED_30);
      checkComposant(COMPOSANT_T3600, LED_42);
      

      failedComposant(99999);


    }
  }

  
}
