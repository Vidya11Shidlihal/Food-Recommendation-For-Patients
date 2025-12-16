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











