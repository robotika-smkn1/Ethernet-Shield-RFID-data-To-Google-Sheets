



# :pushpin: Ethernet-Shield-RFID-data-To-Google-Sheets



<p align="center">
  <img src="https://i.postimg.cc/tRZw0xQ4/logo-removebg-preview.png" alt="robotika smkn1 kotabekasi logo"/ style="height:350px;" "width: 350px;">
</p>


[![Version](https://img.shields.io/badge/VENOM-1.0.17-brightgreen.svg?maxAge=259200)]()
[![Stage](https://img.shields.io/badge/Release-Stable-brightgreen.svg)]()
![licence](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)
[![Coverity Scan Build Status](https://scan.coverity.com/projects/aircrack-ng/badge.svg)](##Link##)

  


## :inbox_tray: Media Social Instagram

To keep this collection up-to-date need contributors who can add more Program Arduino scripts
||
|--------------|
|:mailbox_with_mail: [artha_sa_](https://www.instagram.com/artha_sa_/)
|:mailbox_with_mail: [dicky_asqaelani26](https://www.instagram.com/dicky_asqaelani26/)
|:mailbox_with_mail: [rahmatnurraya](https://www.instagram.com/rahmatnurraya990/)
|:mailbox_with_mail: [applepi_fthur](https://www.instagram.com/applepi_fthur/)
|:mailbox_with_mail: [robotika-smkn1](https://www.instagram.com/robotika.smkn1kotabekasi/)


# :moneybag: [Donate](https://saweria.co/arthasyarif)

# :briefcase: Barang & Bahan
- Arduino Uno
- Rfid Card
- Ethernet Shield
- Kabel Lan RJ45
- Kabel Male & Female


# :mag: Ilustrasi Arduino

<p align="center">
  <img src="https://content.instructables.com/FUO/ON31/JVATKS9E/FUOON31JVATKS9E.jpg" style="height:205px;" "width:205px;"/>
</p>
<p align="center">
  <img src="https://i.postimg.cc/rs8fMZTb/Whats-App-Image-2023-02-04-at-10-12-26.jpg" style="height:205px;" "width:205px;"/>
  <img src="https://i.postimg.cc/FRqKQ80F/Whats-App-Image-2023-02-04-at-10-12-25.jpg" style="height:205px;" "width:205px;"/>
</p>

# :speech_balloon: Notice
<p align="center">
  <img src="https://i.postimg.cc/cCLfDrf8/notice.jpg" style="height:205px;" "width:205px;"/>
  <img src="https://i.postimg.cc/9QZwYGsn/nama.jpg" style="height:205px;" "width:205px;"/>
</p>

<h2>Login Ke Pushing Box</h2>

```bash

?allowed_members=$allowed_members$&Member_ID=$Member_ID$

&allowed_members=1&Member_ID=081212923

```



# :clipboard: Code Google Sheet

```bash
function doGet(e) { 
  Logger.log( JSON.stringify(e) ); 

  var result = 'Ok';

  if (e.parameter == undefined) {
    result = 'No Parameters';
  }
  else {
    var id = '<Your Spreadsheet ID>'; // Spreadsheet ID
    var sheet = SpreadsheetApp.openById(id).getActiveSheet();
    var newRow = sheet.getLastRow() + 1;
    var rfidData = [];
    rfidData[0] = new Date();
    for (var param in e.parameter) {
      Logger.log('In for loop, param='+param);
      var value = stripQuotes(e.parameter[param]);
      switch (param) {
        case 'allowed_members': //Parameter
          rfidData[1] = value; //Value in column B
          break;
        case 'Member_ID':
          rfidData[2] = value;
          break;
        default:
          result = "Wrong parameter";
      }
    }
    Logger.log(JSON.stringify(rfidData));

    // Write new row below
    var newRange = sheet.getRange(newRow, 1, 1, rfidData.length);
    newRange.setValues([rfidData]);
  }

  // Return result of operation
  return ContentService.createTextOutput(result);
}

/**
* Remove leading and trailing single or double quotes
*/
function stripQuotes( value ) {
  return value.replace(/^["']|['"]$/g, "");
}


```

# :clipboard: Source Code

```bash

/*
 * 
 * All the resources for this project: smkn1kotabekasi.sch.id
 * Modified by Robotika smkn1 kotabekasi
 * 
 * Created by Robotika smkn1 kotabekasi
 * 
 */

/* Arduino Code which sends data to google spreadsheet */

#include<SPI.h>
#include<MFRC522.h>
#include <Ethernet.h>
#define SS_PIN 4 //FOR RFID SS PIN BECASUSE WE ARE USING BOTH ETHERNET SHIELD AND RS-522
#define RST_PIN 9
#define No_Of_Card 3
byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };
char server[] = "api.pushingbox.com";   //YOUR SERVER
IPAddress ip(192, 168, 137, 177);
EthernetClient client;     
MFRC522 rfid(SS_PIN,RST_PIN);
MFRC522::MIFARE_Key key; 
byte id[No_Of_Card][4]={
  {692,31,40,42},             //RFID NO-1
 {153,178,60,64},             //RFID NO-2
  {141,764,60,94}              //RFID NO-3
};
byte id_temp[3][3];
byte i;
int j=0;


// the setup function runs once when you press reset or power the board
void setup(){
    Serial.begin(9600);
    SPI.begin();
    rfid.PCD_Init();
    for(byte i=0;i<6;i++)
    {
      key.keyByte[i]=0xFF;
    }
    if (Ethernet.begin(mac) == 0) {
      Serial.println("Failed to configure Ethernet using DHCP");
      Ethernet.begin(mac, ip);
    }
    delay(1000);
    Serial.println("connecting...");
 }

// the loop function runs over and over again forever
void loop(){
    int m=0;
    if(!rfid.PICC_IsNewCardPresent())
    return;
    if(!rfid.PICC_ReadCardSerial())
    return;
    for(i=0;i<4;i++)
    {
     id_temp[0][i]=rfid.uid.uidByte[i]; 
               delay(50);
    }
    
     for(i=0;i<No_Of_Card;i++)
    {
            if(id[i][0]==id_temp[0][0])
            {
              if(id[i][1]==id_temp[0][1])
              {
                if(id[i][2]==id_temp[0][2])
                {
                  if(id[i][3]==id_temp[0][3])
                  {
                    Serial.print("your card no :");
                    for(int s=0;s<4;s++)
                    {
                      Serial.print(rfid.uid.uidByte[s]);
                      Serial.print(" ");
                     
                    }
                    Serial.println("\nValid Person");
                    Sending_To_spreadsheet();
                    j=0;
                              
                              rfid.PICC_HaltA(); rfid.PCD_StopCrypto1();   return; 
                  }
                }
              }
            }
     else
     {j++;
      if(j==No_Of_Card)
      {
        Serial.println("Not a valid Person");
        Sending_To_spreadsheet();
        j=0;
      }
     }
    }
    
       // Halt PICC
    rfid.PICC_HaltA();
  
    // Stop encryption on PCD
    rfid.PCD_StopCrypto1();
 }

 void Sending_To_spreadsheet()   //CONNECTING WITH MYSQL
 {
   if (client.connect(server, 80)) {
    Serial.println("connected");
    // Make a HTTP request:
    client.print("GET /pushingbox?devid=vE28B66ED46C9137&allowed_members=");     //YOUR URL CUSTOM
    if(j!=No_Of_Card)
    {
      client.print('1');
//      Serial.print('1');
    }
    else
    {
      client.print('0');
    }
    
    client.print("&Member_ID=");
    for(int s=0;s<4;s++)
                  {
                    client.print(rfid.uid.uidByte[s]);
                                  
                  }
    client.print(" ");      //SPACE BEFORE HTTP/1.1
    client.print("HTTP/1.1");
    client.println();
    client.println("Host: api.pushingbox.com");
    client.println("Connection: close");
    client.println();
  } else {
    // if you didn't get a connection to the server:
    Serial.println("connection failed");
  }
 }

 

```

