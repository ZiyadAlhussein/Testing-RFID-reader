# Testing-RFID-reader

## Introduction
A wireless system made up of two parts, readers and tags, is referred to as radio frequency identification (RFID). A reader is a device equipped with one or more antennas that transmit radio waves and receive signals from RFID tags. Tags can be passive or active. They convey their identity and other information to adjacent readers using radio waves. </br>

![image](https://user-images.githubusercontent.com/108147030/223827086-21d9375c-eb2b-4052-a3ce-20478f182b64.png)

## Circuit Diageam and Implementation
**i) Components:** </br>
              1- Arduino UNO board </br>
              2- RFID reader (RFID-RC522) </br>
              3- RFID card/tags </br>
              4- Breadboard </br>
              5- LED lights </br>
              6- Wires </br>
 
**ii) Software:** Arduino IDE 2.0.4 (https://www.arduino.cc/en/software) </br>

**iii) Circuit Diagram** </br>
![image](https://user-images.githubusercontent.com/108147030/223833813-21a3ff67-bde8-4f31-92e2-70413d25c4d9.png) </br>

![WhatsApp Image 2023-03-09 at 00 35 48](https://user-images.githubusercontent.com/108147030/223857101-316792e0-0791-43a5-9a79-3197deb3da92.jpg)



## Code-for-RFID-Reader-and-Testing

**i) Verifying Arduino UNO on arduino IDE and selecting the Port:** </br>
![image](https://user-images.githubusercontent.com/108147030/223836056-624fdbbd-01c6-4279-ab47-3e75c6ea9b72.png) </br>
![image](https://user-images.githubusercontent.com/108147030/223836451-807161e6-7ffd-4bc7-8883-fb8b088e8518.png) </br>

**ii) Installing RFID library:**
![image](https://user-images.githubusercontent.com/108147030/223841133-9c940ed7-a96b-405f-b802-fa1c11e4467b.png) </br>
![image](https://user-images.githubusercontent.com/108147030/223843148-29629096-9bb4-4443-8c04-c343e85902fd.png) </br>



**iii) Code for RFID Reader:**

**1- Checking ID of your tag:** </br>
  - Run the following code: File -> Examples -> MFRC522 -> DumpInfo </br>
  - After uploading the code go to Serial Monitor to see your ID card/tag number </br>
  ![image](https://user-images.githubusercontent.com/108147030/223847558-140fabee-383d-4f46-99b3-b4640816b043.png)

```ruby 
#include <SPI.h>
#include <MFRC522.h>

#define RST_PIN         9          // Configurable, see typical pin layout above
#define SS_PIN          10         // Configurable, see typical pin layout above

MFRC522 mfrc522(SS_PIN, RST_PIN);  // Create MFRC522 instance

void setup() {
	Serial.begin(9600);		// Initialize serial communications with the PC
	while (!Serial);		// Do nothing if no serial port is opened (added for Arduinos based on ATMEGA32U4)
	SPI.begin();			// Init SPI bus
	mfrc522.PCD_Init();		// Init MFRC522
	delay(4);				// Optional delay. Some board do need more time after init to be ready, see Readme
	mfrc522.PCD_DumpVersionToSerial();	// Show details of PCD - MFRC522 Card Reader details
	Serial.println(F("Scan PICC to see UID, SAK, type, and data blocks..."));
}

void loop() {
	// Reset the loop if no new card present on the sensor/reader. This saves the entire process when idle.
	if ( ! mfrc522.PICC_IsNewCardPresent()) {
		return;
	}

	// Select one of the cards
	if ( ! mfrc522.PICC_ReadCardSerial()) {
		return;
	}

	// Dump debug info about the card; PICC_HaltA() is automatically called
	mfrc522.PICC_DumpToSerial(&(mfrc522.uid));
}
```
**2- Testing RFID for 4 different tags:**

- We used the avalible library for RFID while adding a simple IF else statment to write our code shown below: </br>
- After assigning a student for each RFID tag/card we can check the serial monitor to see the results after uploading: </br> 
![image](https://user-images.githubusercontent.com/108147030/223856757-8598fe74-e3d3-45f9-8c0b-cc2e6070e2d2.png)

```ruby 
#include <SPI.h>
#include <MFRC522.h>

 
#define SS_PIN 10
#define RST_PIN 9

MFRC522 mfrc522(SS_PIN, RST_PIN);   // Create MFRC522 instance.
 
void setup() 
{
  Serial.begin(9600);   // Initiate a serial communication
  SPI.begin();      // Initiate  SPI bus
  mfrc522.PCD_Init();   // Initiate MFRC522
  Serial.println("Put your card to the reader...");
  Serial.println();
}

void loop() 
{
  // Look for new cards
  if ( ! mfrc522.PICC_IsNewCardPresent()) 
  {
    return;
  }
  // Select one of the cards
  if ( ! mfrc522.PICC_ReadCardSerial()) 
  {
    return;
  }
  //Show UID on serial monitor
  Serial.print("UID tag :");
  String content= "";
  byte letter;
  for (byte i = 0; i < mfrc522.uid.size; i++) 
  {
     Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
     Serial.print(mfrc522.uid.uidByte[i], HEX);
     content.concat(String(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " "));
     content.concat(String(mfrc522.uid.uidByte[i], HEX));
  }
  Serial.println();
  Serial.print("Message : ");
  content.toUpperCase();
  if (content.substring(1) == "D1 3A 7E 1C") //change here the UID of the card/cards that you want to give access
  {
    Serial.println("Ziyad Alhussein Present");
      delay(500);
    Serial.println();
  }
 
 else if (content.substring(1) == "EA E4 44 17")  {
    Serial.println(" Mukhtar Alhajji Present ");
      delay(500);
  }
  else if (content.substring(1) == "41 D7 FA 1B")  {
    Serial.println(" Majed Alnashri Present ");
      delay(500);
  }
  else if (content.substring(1) == "04 55 D3 3D")  {
    Serial.println(" Rinad Choukair  Present ");
      delay(500);
  }
}
```
