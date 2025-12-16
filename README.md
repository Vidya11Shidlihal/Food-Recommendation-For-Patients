Title: Food-Recommendation-For-Patients

Introduction: Many people today suffer from diseases like diabetes, blood pressure, and high cholesterol because they do not know which foods are healthy for them or how much they should eat. Even when doctors give diet instructions, patients often forget them after leaving the hospital. Not everyone uses mobile apps or the internet, but almost everyone can receive a simple SMS on their phone. To solve this problem, we developed Diet Care, a low-cost system that sends correct food recommendations along with portion or quantity details directly to the patient’s phone using a GSM module. The system provides simple daily diet messages such as what food to eat, what to avoid, and how much quantity is safe to consume. By giving clear and easy-to-follow diet tips through SMS, this system helps patients maintain healthy eating habits and manage their health better every day.

Problem statement:
People with diseases like diabetes, BP, and cholesterol often do not know which foods are good for their health. They forget diet advice given by doctors, which leads to wrong food choices and health problems. There is a need for a simple way to guide patients with correct food recommendations based on their health condition.

Objective:The main objective of this project is to provide correct diet suggestions to patients based on their health condition. It aims to help patients avoid unhealthy food choices and follow a proper diet every day. The system focuses on giving diet information in a simple and quick manner, so patients can easily maintain a healthier lifestyle and manage their disease better.

Methodology 
1.Understand the Requirement
 *A system is needed to give food recommendations for diseases like diabetes, BP, etc.
The recommendations should reach the user through SMS easily.

2.Select Required Components
 *ESP8266 microcontroller
 *SIM900A GSM module (for sending SMS)
 *I2C LCD display (to show messages)
 *Serial Monitor (for user input)
 *Power supply and connection wires

3.Store Diet Data in Program
 *Pre-define diet recommendations for each disease (1–9).
 *Save them inside ESP8266 program memory.
 
4.Develop the Code in Arduino IDE
 *Read disease number entered in Serial Monitor
 *Match it with the correct stored food recommendation
 *Show disease and diet on LCD
 *Send SMS command to GSM module with diet message

5.Hardware Assembly
 *Connect ESP8266 with GSM module and LCD display
 *Insert SIM card in GSM module
 *Provide stable power supply

6.Testing the System
 *Enter different disease numbers and check the response
 *Verify LCD display, GSM working, and SMS sending
 *Ensure correct recommendation goes to mobile every time give me this 

Result:
*The DietCare system was tested and it performed as expected. When the user entered a disease number through the Serial Monitor, the correct diet recommendation was sent to the user’s phone through SMS. The message delivery was quick and reliable every time. The Serial Monitor also helped to verify that each step of the process worked properly without any errors.

Conclusion:
The DietCare project successfully helps patients receive the right diet suggestions for their health problem. The system sends personal food recommendations through SMS, making it simple for anyone to use. By entering a disease number and phone number, users can quickly get useful diet guidance. The ESP8266, GSM module, and LCD work together smoothly, proving that the system is reliable and helpful for better health management.

Future Scope:
 *The system can be upgraded to use a mobile app for easier user interaction
 *Add features to share diet tips with friends and family to encourage healthy habits.
 *Add more food items and local dishes so the system can help many different types of users.
 *Add a chatbot to answer user questions and give personal diet advice.
*Include options for mental health and drinking water reminders for complete health care.
*Improve the system using advanced AI to understand what users like and suggest healthier food choices.

CODE:
#INCLUDE <WIRE.H>
#INCLUDE <LIQUIDCRYSTAL_I2C.H>
#INCLUDE <SOFTWARESERIAL.H>
// LCD SETUP (I2C ADDRESS 0X27)
LIQUIDCRYSTAL_I2C LCD(0X27, 16, 2);
// SIM900A SETUP: ESP8266 D5 = TX, D6 = RX
SOFTWARESERIAL SIM(14, 12); // RX = 14 (D5), TX = 12 (D6)
STRING PHONENUMBER = "";
STRING DISEASECHOICE = "";
BOOL GOTPHONE = FALSE;
BOOL GOTDISEASE = FALSE;
// VALID DISEASE LIST (UPDATED AS YOU REQUESTED)
STRING GETRECOMMENDATION(INT D) {
  SWITCH (D) {
    CASE 1: RETURN "DIABETES: EAT OATS, LEAFY VEG, AVOID SUGAR.";
    CASE 2: RETURN "BP: EAT BANANAS, AVOID SALT & FRIED FOOD.";
    CASE 3: RETURN "THYROID: EAT EGGS, NUTS, AVOID JUNK.";
    CASE 4: RETURN "FEVER: DRINK FLUIDS, FRUITS, SOUP.";
    CASE 5: RETURN "CHOLESTEROL: EAT OATS, NUTS, AVOID OILY FOOD.";
    CASE 6: RETURN "HEART ISSUE: EAT FISH, NUTS, AVOID JUNK.";
    CASE 7: RETURN "STOMACH ISSUE: EAT CURD, SOFT FOODS, AVOID SPICY.";
    CASE 8: RETURN "COLD & COUGH: DRINK HOT WATER, GINGER, HONEY.";
    CASE 9: RETURN "WEAKNESS: EAT FRUITS, DATES, DRY FRUITS.";
    DEFAULT: RETURN "INVALID DISEASE CODE!";
  }
}
// SCROLL TEXT
VOID LCDSCROLLLINE(STRING TEXT, INT LINENUM, INT INITIALDELAY = 2000, INT SCROLLDELAY = 350) {
  INT LEN = TEXT.LENGTH();
  LCD.SETCURSOR(0, LINENUM);
  LCD.PRINT(TEXT.SUBSTRING(0, MIN(LEN, 16)));
  DELAY(INITIALDELAY);
  IF (LEN > 16) {
    FOR (INT I = 1; I <= LEN - 16; I++) {
      STRING SUB = TEXT.SUBSTRING(I, I + 16);
      LCD.SETCURSOR(0, LINENUM);
      LCD.PRINT(SUB);
      DELAY(SCROLLDELAY);
    }
    DELAY(INITIALDELAY);
  }
}
VOID SETUP() {
  SERIAL.BEGIN(9600);
  SIM.BEGIN(9600);
  LCD.INIT();
  LCD.BACKLIGHT();
  LCD.SETCURSOR(0, 0);
  LCD.PRINT("FOOD RECOMMENDER");
  LCD.SETCURSOR(0, 1);
  LCD.PRINT("ENTER 1-9 (SERIAL)");
  DELAY(2000);
  LCD.CLEAR();
  SERIAL.PRINTLN("ENTER DISEASE NUMBER (1-9) AND PRESS ENTER:");
  SERIAL.PRINTLN("THEN ENTER PHONE NUMBER (WITH +COUNTRY) E.G. +919876543210");
}
VOID LOOP() {

  // =============== STEP 1 — READ DISEASE CODE ==============
  IF (SERIAL.AVAILABLE() && !GOTDISEASE) {
    DISEASECHOICE = SERIAL.READSTRINGUNTIL('\N');
    DISEASECHOICE.TRIM();
    // VALIDATION: CHECK IF ONLY DIGITS
    BOOL ISNUMBER = TRUE;
    FOR (INT I = 0; I < DISEASECHOICE.LENGTH(); I++) {
      IF (!ISDIGIT(DISEASECHOICE[I])) {
        ISNUMBER = FALSE;
        BREAK;
      }
    }
    IF (!ISNUMBER) {
      SERIAL.PRINTLN("INVALID DISEASE CODE! ENTER 1–9 ONLY.");
      LCD.CLEAR();
      LCD.SETCURSOR(0, 0);
      LCD.PRINT("INVALID CODE!");
      LCD.SETCURSOR(0, 1);
      LCD.PRINT("TRY 1-9 ONLY");
      DELAY(2000);
      LCD.CLEAR();
      LCD.SETCURSOR(0, 0);
      LCD.PRINT("ENTER DISEASE 1-9");
      RETURN;
    }
    INT DNUM = DISEASECHOICE.TOINT();
    // VALIDATION: NUMBER MUST BE 1–9
    IF (DNUM < 1 || DNUM > 9) {
      SERIAL.PRINTLN("INVALID DISEASE CODE! ENTER 1–9 ONLY.");
      LCD.CLEAR();
      LCD.SETCURSOR(0, 0);
      LCD.PRINT("INVALID CODE!");
      LCD.SETCURSOR(0, 1);
      LCD.PRINT("TRY 1-9 ONLY");
      DELAY(2000);
      LCD.CLEAR();
      LCD.SETCURSOR(0, 0);
      LCD.PRINT("ENTER DISEASE 1-9");
      RETURN;
    }
    // VALID INPUT
    GOTDISEASE = TRUE;
    STRING REC = GETRECOMMENDATION(DNUM);
    LCD.CLEAR();
    LCD.SETCURSOR(0, 0);
    LCD.PRINT("DISEASE: ");
    LCD.PRINT(DNUM);
    LCDSCROLLLINE(REC, 1, 2000, 350);
    LCD.CLEAR();
    LCD.SETCURSOR(0, 0);
    LCD.PRINT("ENTER PHONE:");
    LCD.SETCURSOR(0, 1);
    LCD.PRINT("(SERIAL) E.G.+91...");
    SERIAL.PRINTLN("ENTER PHONE NUMBER (WITH COUNTRY CODE, E.G. +91XXXXXXXXXX) AND PRESS ENTER:");
  }
  // =================== STEP 2 — READ PHONE NUMBER =====================
  IF (SERIAL.AVAILABLE() && GOTDISEASE && !GOTPHONE) {
    PHONENUMBER = SERIAL.READSTRINGUNTIL('\N');
    PHONENUMBER.TRIM();
    IF (PHONENUMBER.LENGTH() >= 7) {
      GOTPHONE = TRUE;
      LCD.CLEAR();
      LCD.SETCURSOR(0, 0);
      LCD.PRINT("SENDING SMS...");
      LCDSCROLLLINE(PHONENUMBER, 1, 1000, 400);
      INT DNUM = DISEASECHOICE.TOINT();
      STRING MESSAGE = GETRECOMMENDATION(DNUM);
      SENDSMS(PHONENUMBER, MESSAGE);

      LCD.CLEAR();
      LCD.SETCURSOR(0, 0);
      LCD.PRINT("SMS SENT!");
      LCDSCROLLLINE(MESSAGE, 1, 2000, 350);
      SERIAL.PRINTLN("SMS SENT SUCCESSFULLY!");
      SERIAL.PRINTLN("MESSAGE PREVIEW: " + MESSAGE);
      LCD.CLEAR();
      LCD.SETCURSOR(0, 0);
      LCD.PRINT("ENTER DISEASE 1-9");
      SERIAL.PRINTLN("ENTER DISEASE NUMBER (1-9): ");
      GOTDISEASE = FALSE;
      GOTPHONE = FALSE;
      DISEASECHOICE = "";
      PHONENUMBER = "";
    }
  }
}
VOID SENDSMS(STRING NUMBER, STRING MESSAGE) {
  SIM.PRINTLN("AT");
  DELAY(1000);
  SIM.PRINTLN("AT+CMGF=1");
  DELAY(1000);
  SIM.PRINT("AT+CMGS=\"");
  SIM.PRINT(NUMBER);
  SIM.PRINTLN("\"");
  DELAY(1000);
  SIM.PRINT(MESSAGE);
  DELAY(1000);
  SIM.WRITE(26);
  DELAY(3000);
}












