 Water-Dispenser-using-RFID
Get water using RFID
I USED SERVO,ARDUINO,WIRES,BUZZER and RFID....
Please visit my channel on youtube "BER NA" or Search my name" BERNADETTE BACUDO"


#include "SPI.h"
#include "MFRC522.h"
#include <Servo.h>

#define SS_PIN 10
#define RST_PIN 9
#define SP_PIN 8
#define SP_PIN 2

MFRC522 rfid(SS_PIN, RST_PIN);

MFRC522::MIFARE_Key key;

Servo servo;  
int SPEAKER = 2;

void setup() {
  Serial.begin(9600);
  SPI.begin();
  servo.attach(3);
  rfid.PCD_Init();
  pinMode(SPEAKER,OUTPUT);
  servo.write(0);
}

void loop() {
  if (!rfid.PICC_IsNewCardPresent() || !rfid.PICC_ReadCardSerial())
    return;

  // Serial.print(F("PICC type: "));
  MFRC522::PICC_Type piccType = rfid.PICC_GetType(rfid.uid.sak);
  // Serial.println(rfid.PICC_GetTypeName(piccType));

  // Check is the PICC of Classic MIFARE type
  if (piccType != MFRC522::PICC_TYPE_MIFARE_MINI &&
    piccType != MFRC522::PICC_TYPE_MIFARE_1K &&
    piccType != MFRC522::PICC_TYPE_MIFARE_4K) {
    Serial.println(F("Your tag is not of type MIFARE Classic."));
    return;
  }

  String strID = "";
  for (byte i = 0; i < 4; i++) {
    strID +=
    (rfid.uid.uidByte[i] < 0x10 ? "0" : "") +
    String(rfid.uid.uidByte[i], HEX) +
    (i!=3 ? ":" : "");
  }
  strID.toUpperCase();

  Serial.print("Your Card Number Is: ");
  Serial.println(strID);
 
    servo.write(0);
  if ((strID == "B3:F2:91:2E") || (strID ==  "70:26:BF:A3") || (strID == "D6:2F:61:AC"))
    {
     Serial.println("Your cup should ready...");
     tone(SPEAKER,200);
     delay(100);
     noTone(SPEAKER);
     delay(100);
     tone(SPEAKER,300);
     delay(250);
     noTone(SPEAKER);
     
     servo.write(150);
     delay(11000);
     tone(SPEAKER,900);
     delay(500);
     noTone(SPEAKER);
     delay(500);
     tone(SPEAKER,900);
     delay(500);
     noTone(SPEAKER);
     delay(500);
     tone(SPEAKER,900);
     delay(500);
     
     servo.write(0);
     tone(SPEAKER,100);
     delay(500);
     noTone(SPEAKER);
     }     
     else
     {
      tone(SPEAKER,500);
      delay(1000);
      noTone(SPEAKER);
      Serial.println("Sorry you're not allow to get water here.");
      } 
   
  rfid.PICC_HaltA();
  rfid.PCD_StopCrypto1();
}



