# -ecek-Otomat-
LED'lerle taklit edilmiş içecek otomatı prototipi 
kaynak kod

#include <SPI.h> 

#include <MFRC522.h>
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>
#define SS_PIN 10 
#define RST_PIN 9 
#define KIRMIZI_LED_PIN 7
#define SARI_LED_PIN 6
#define MAVI_LED_PIN 2


LiquidCrystal_I2C lcd(0x27,16,2);
MFRC522 mfrc522(SS_PIN, RST_PIN); 

int upButton = 5;
int downButton = 3;
int selectButton = 4;
int menu = 1;
int buton_durumu = 0;
 

int code[] = {210,244,183,28,141}; 
int codeRead = 0;
String uidString;

void setup() {

    pinMode(4, INPUT);
    pinMode(7, OUTPUT); 
    pinMode(2, OUTPUT);
    pinMode(6, OUTPUT);
   SPI.begin();       
   mfrc522.PCD_Init(); 
  
   pinMode(8,OUTPUT);
  lcd.init();
  lcd.backlight();
  pinMode(upButton, INPUT_PULLUP);
  pinMode(downButton, INPUT_PULLUP);
  pinMode(selectButton, INPUT_PULLUP);
  updateMenu();


  lcd.setCursor(0,0);
  lcd.print("KARTI OKUTUNUZ");
  lcd.setCursor(0,1);
  lcd.print("                            ");

}

void loop() {



 
if (!digitalRead(downButton)){
    menu++;
    updateMenu();
    delay(100);
    while (!digitalRead(downButton));
  }
  if (!digitalRead(upButton)){
    menu--;
    updateMenu();
    delay(100);
    while(!digitalRead(upButton));
  }
  if (!digitalRead(selectButton)){
    executeAction();
    updateMenu();
     delay(100);
    digitalWrite(8,HIGH);
    delay(500);
    digitalWrite(8,LOW);
    delay(100);
    while (!digitalRead(selectButton));
  }
    
  

if ( mfrc522.PICC_IsNewCardPresent())

    {

        if ( mfrc522.PICC_ReadCardSerial())

        {

           lcd.clear();
           lcd.setCursor(0,0);
           lcd.print("KART OKUNDU");
                          

           for (byte i = 0; i < mfrc522.uid.size; i++) {



           }



            int i = 0;

            boolean match = true;

            while(i<mfrc522.uid.size)

            {

    

               if(!(int(mfrc522.uid.uidByte[i]) == int(code[i])))

               {

                  match = false;

               }

              i++;

            }



            delay(3000);

           lcd.clear();

           lcd.setCursor(0,0);

           if(match)

           {



           lcd.setCursor(0,0);
           lcd.print("SECIM YAPINIZ");
           lcd.setCursor(0,1);
           lcd.print("Kola-Fanta-Gazoz");
              

           

    

           }else{

          

              lcd.print("GECERSIZ KART"); 
              delay(100000000);

           }

            

             mfrc522.PICC_HaltA();



             delay(3000); 

             reset_state();
        
         
        }
       

}

}

void updateMenu() {
  switch (menu) {
    case 0:
    lcd.clear();
      menu = 1;
      break;
    case 1:
      lcd.clear();
      lcd.print(">KOLA");
      lcd.setCursor(0, 1);
      lcd.print(" FANTA");
      break;
    case 2:
      lcd.clear();
      lcd.print(" KOLA");
      lcd.setCursor(0, 1);
      lcd.print(">FANTA");
      break;
    case 3:
      lcd.clear();
      lcd.print(">GAZOZ");
      break;

  }
}
void action1() {
  lcd.clear();
  lcd.print(">HAZIRLANIYOR...");
  delay(5000);  
  lcd.print(">HAZIR !");
}
void action2() {
  lcd.clear();
  lcd.print(">HAZIRLANIYOR...");
  delay(5000); 
  lcd.print(">HAZIR !");
}
void action3() {
  lcd.clear();
  lcd.print(">HAZIRLANIYOR...");
  delay(5000);
  lcd.print(">HAZIR !");
}
void executeAction() {
  switch (menu) {
    case 1:
action1();
      
   buton_durumu = digitalRead(4);
   if(buton_durumu == 1)
  digitalWrite(7,HIGH);
  else
  digitalWrite(7,LOW);
      break;

    case 2:
action2();
    
  buton_durumu = digitalRead(4);
  if(buton_durumu == 1)
  digitalWrite(6,LOW);
  else
  digitalWrite(6,HIGH);
      break;
   
    case 3:
action3();
      
     
  buton_durumu = digitalRead(4);
  if(buton_durumu == 1)
  digitalWrite(2,HIGH);
  else
  digitalWrite(2,LOW);
      break;
  }
}


void reset_state()

{

    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("HAZIR!");
    
    
}

   





    
  

   
    
 

      
