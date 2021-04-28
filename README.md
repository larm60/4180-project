# 4180 Project - Security System
By Kenta Xu, Abi Ivemeyer, Nishalini Shanmugan, Seung Wook Jin
*<p align="center">![Our Entire Setup](https://github.com/aivemeyer/4180-project/blob/main/images/20210427_121204.jpg)<br/>
Figure 1. Our complete setup.</p><br/>*

*<p align="center">![Security System Architecture Diagram](https://github.com/aivemeyer/4180-project/blob/main/images/4180Diagram.JPG)<br/>
Figure 2. Architecture Diagram for Security System.</p><br/>*

## Overview
<p> Our project is a security system on a breadboard that will upload data to the cloud. When turned on, the system will be armed with a constant red light. When motion is detected, the alarm will go off and the light will blink, sending an email notification to the user. IT will continue going off until the passcode is correctly entered, in which the sound stops and the light dims. While the system is on, it will be connected to Amazon Web Services and send distance readings. </p>

## Components 
### mbed
*<p align="center">![mbed](https://github.com/aivemeyer/4180-project/blob/main/images/Mbed.jpg)<br/>
Figure 3. Mbed.</p><br/>*
[mbed Wiki](https://os.mbed.com/handbook/Homepage)
 
### Raspberry Pi 3
*<p align="center">![Raspberyy Pi](https://github.com/aivemeyer/4180-project/blob/main/images/Raspberry%20Pi.jpg)<br/>
Figure 4. Raspberry Pi 3.</p><br/>*
[Raspberry Pi 3 Wiki](http://raspberrypiwiki.com/Raspberry_Pi_3_Model_B_Plus)

### Touchpad
*<p align="center">![Touchpad](https://github.com/aivemeyer/4180-project/blob/main/images/Touchpad.jpg)<br/>
Figure 5. Touchpad.</p><br/>*
[Touchpad Wiki](https://os.mbed.com/users/4180_1/notebook/mpr121-i2c-capacitive-touch-sensor/)

### Speaker and Class D Amp
*<p align="center">![Speaker](https://github.com/aivemeyer/4180-project/blob/main/images/Speaker.jpg)
Figure 5. Speaker.</p><br/>*
*<p align="center">![Amp](https://github.com/aivemeyer/4180-project/blob/main/images/Amp.jpg)<br/>
Figure 6. Class D amplifier.</p><br/>*
[Speaker and Class D Amp Wiki](https://os.mbed.com/users/4180_1/notebook/using-a-speaker-for-audio-output/)

### WiFi SOC
*<p align="center">![Wifi](https://github.com/aivemeyer/4180-project/blob/main/images/Wifi.jpg)<br/>
Figure 7. WiFi SOC.</p><br/>*
[WiFi SOC Wiki](https://os.mbed.com/users/4180_1/notebook/using-the-esp8266-with-the-mbed-lpc1768/)

### Sonar Motion Sensor
*<p align="center">![Sonar](https://github.com/aivemeyer/4180-project/blob/main/images/Sonar.jpg)<br/>
Figure 8. Sonar motion sensor.</p><br/>*
[Sonar Motion Sensor Wiki](https://os.mbed.com/users/4180_1/notebook/using-the-hc-sr04-sonar-sensor/)

### LED
*<p align="center">![LED](https://github.com/aivemeyer/4180-project/blob/main/images/LED.jpg)<br/>
Figure 9. Red LED light.</p><br/>*
[LED Wiki](https://os.mbed.com/users/4180_1/notebook/rgb-leds/)

### uLCD
*<p align="center">![LCD](https://github.com/aivemeyer/4180-project/blob/main/images/LCD.png)<br/>
Figure 10. uLCD.</p><br/>*
[uLCD Wiki](https://os.mbed.com/users/4180_1/notebook/ulcd-144-g2-128-by-128-color-lcd/)

### External 5V Source
*<p align="center">![5V Source](https://github.com/aivemeyer/4180-project/blob/main/images/5V.png)<br/>
Figure 11. External 5V source.</p><br/>*

## Connection
##Mbed Pin Connections
Figure 12. mbed-pinout.</p><br/>*
As shown in figure 3, our mbed is grounded, and Vout is 3.3 V. Pin 9 of the mbed is connected to the SDA pin of the touchpad. Pin 10 is connected to the SCL pin of the touchpad. A 4.7k Ohm resistor is connected between both pins. Pin 13 is connected to the TX pin of the Wifi SOC, and pin 14 is connected to the RX of the Wifi SOC. Pin 17 is connected to the Trig Pin of the Sonar Motion Sensor. Pin 18 is connected to the Echo Pin of the Sonar Motion Sensor. Pin 21 is connected to a 4.7k Ohm resistor, which is limiting the current of a Red Led. Pin 22 is connected to the In+ pin of the Class D Audio Amplifier. Pin 26 is connected to IRQ of the Touchpad. Pin 27 is connected to RX of the uLCD. Pin 28 is connected to TX of the uLCD. 
##Raspberry Pi Pin Connections
For the Raspberry Pi 3, the usb connects to the microusb on the mbed. The HDMI on the Raspberry Pi 3 connects to the monitor.  
##Touchpad Pin Connections
Figure 13. touchpad-pinout.</p><br/>*
##Speaker and Class D Audio Amplifier 
Figure 14. speaker-pinout.</p><br/>*
### WiFi SOC
Figure 15. wifi-pinout.</p><br/>*

### Sonar Motion Sensor
Figure 16. sonar-pinout.</p><br/>*
For our project, we used pin 17 and pin 18 on the mbed instead of pin 6 and pin 7. 
### LED
The shorter leg of the led is grounded. The longer leg is connected to a 4.7K Ohm resistor, which is connected to pin 21 on the mbed. 
### uLCD
Figure 17. uLCD-pinout.</p><br/>*
 
## Software Setup
 
#### Raspberry Pi
 
<ol>
<li>Ready Raspberry Pi (3 or above recommended).</li>
<li>Make sure your Raspberry Pi has an Internet connection.</li>
<li>Pull this repository’s raspberrypi directory onto your Pi.</li>
<li>Enter your AWS access key, secret access key, region, and table name into main.py.</li>
<li>While the mbed is running, run the main.py script on your Pi.</li>
</ol>
 
#### Amazon Web Services
 
<ol>
<li>Create IAM Role for a Lambda Function and attach the policy for full access to DynamoDB.</li>
<li>Create a new DynamoDB table to store station ID (partition key), timestamp (sort key), and sonar distance readings in Python 3.38.</li>
<li>On AWS Simple Email Services, add and verify your email for sending and receiving the notifications </li>
<li>Create a new Lambda Function and attach the role created in step 1.</li>
<li>Download the main.py from this repository’s lambda directory.</li>
<li>Update the AWS access key, secret access key, region, and table name into main.py. </li>
<li>Upload the main.py’s code to the Lambda Function.</li>
<li>For the Lambda Function, create a CloudWatch Events trigger that is scheduled for every minute.</li>
</ol>
 
## Development Notes
 
### 2021.4.13
* Wired touchpad, sonar, LED, LCD, and WiFi SOC to the mbed.
* Programmed touchpad to go into disarmed state when 4-digit PIN is entered.
* Programmed sonar to print distances.
 
### 2021.4.15
* Wired speaker and audio amplifier.
* Created disarmed state, armed state, and intruder state.
* Programmed LCD to print one of three states at a time.
* Programmed speaker to go off as an alarm.
* Programmed everything in RTOS.
 
### 2021.4.20
* Programmed WiFi to run website that can be connected.
* Removed RTOS due to issues.
* Got everything working out of RTOS.
 
### 2021.4.22
* Set up Raspberry Pi 3.
* Created Python script for Pi to send data to AWS DynamoDB.
* Created Lambda Function to read DynamoDB entries and send email if data (distance) passes threshold.
 
### 2021.4.27
* Programmed mbed to send distance readings from sonar through serial to Raspberry Pi.
* Raspberry Pi sends distance data to AWS DynamoDB successfully.
