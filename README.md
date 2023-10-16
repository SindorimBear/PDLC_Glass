# Night time light-detecting safety glass 
A prototype for a product that detects high-intensity light to protect the driver's vision

## Table of Contents
1. Briefing
1. Theory
1. Code
1. Procedure
1. Summary
1. Conclusion

## Briefing
Problem: Headlights for carss are important for illuminating night-time road. They can provide the driver a clear vision of what is happening ahead and allows them to avoid any accidents. However, some types of cars have brought forth high-intensity headlights, which are causing unneccessary car accidents.
  High-intensity headlights and LEDS can cause temporary blindess to other drivers, much like the sun glaring into our eyes during daytime. These high-intensity headlights makes it impossible to clearly view objects moving directly in front of the driver, which makes the accdidents more fatal than the sunlight during daytime.
  Another problem is that LED headlights look more bluer in the output spectrum than halogen. The human eye have difficulty processing blue light, since blue wavelengths tend to focus just ahead of the retina rather than on it.
  These headlight problems have posed threats to a broad-range of drivers and appeared to be a problem yet to be resolved.

Solution Idea: We decided to invent a glass that could prevent such casualities. The class will detect high intensity lights or LEDS, send the coordinates for the position of the light input, and diminsh the light long enough to prevent problems for the driver.

## Theory 
  Generally, most of the dual headlights attached on vehicles emit white light, which is produced by using halogen bulbs, xenon/ HID (High-Intensity Discharge) bulbs, LED (Light Emitting-Diode) bulbs, etc. 
  High-intensity lights are the main sources of temporary blindness. According to the headlight regulations in United Sates governed by Federal Motor Vehicle Safety Standard (FMVSS) No 108., There are no minimum and maximum intensity regulations for headlights yet. The human eye can take 10,000 lux for 8 hours. Considering the distance between the driver and the headlight of the opposite lane diminishes as they cross path and the fact that a high-intensity light beam can produce 75,000 lux, a direct light beam to the human eye would cause not only instant temporary blindness, but also permanent eye damage.
  LED lights have proven to be less problematic to the human eye than that caused by high-intensity lights. However, prolonged and instense exposure to LEDs can cause temporary visual impairment.

  In order to resolve this issue, the light beam must prevented from directly reaching the human eye. Two cameras will be installed in the car. The first camera will detect the position of the driver's eye and the second camera will detect the position of the light beam. If the light beam's intensity goes over certain intensity, the camera will calculate the driver's eye angle. It will prevent the light beam from hitting the driver's eye by either deflecting the beam or blocking the light completely. 
  In order to create such tool, we will need the following:
  1. PDLC Film (Polymer Disperse Liquid Crystal)
     PDLC films are capable of adjusting light transmission between transparent and opaque using AC power. For more information, look into the link below: https://www.fabmacglass.com/post/what-is-a-smart-pdlc-switchable-film-and-how-does-it-work#:~:text=The%20PDLC%20film%3A%20PDLC%20means,controlled%20by%20an%20electric%20volt.
  2. Arduino Serial
     In order to control the PDLC's electric support, we can use arduino and control each of the PDLC films. Since the PDLC maintains it's transparency when the electric current is flowing and the transparency of the film depends on how much electricity is flowing through, a lot of power supply is required for this product.
  3. Camera
     Using the opencv package in python, we can pick up the position and intensity of the light intensity from the camera. Since most of the vehicles these days have black box cameras, which have a resolution of 720p, we used a camera with a similar resolution.

##  Code 
  ###  A. Python 
  There are two codes I have written for this project
  1. detect_multiple.py
  2. main.py

1. detect_multiple.py
  '''
from imutils import contours
from skimage import measure
import numpy as np
import imutils
import cv2 as cv
import serial
import time

These are the necessary packages for the code to run.

imutils package functions as a basic image proccessing method. The function is necessary for two reasons: 
-resize and divide the video capture images
-add a contour to track the driver's vision.

skimage package was necessary for the same reason as imutils


ser = serial.Serial('/dev/ttyUSB0',9600)
print("\ndetect_multiple.py launched\n")
ser.write(b'a')
print("\n============time sleep 1===========\n")
time.sleep(1)




def detect_multiple(image) : 


def destination(x,y,w,h):


def int2bin(i):



def offSpot(lst):



2. main.py

   
  ###  B. Arduino 

  char input;
void setup() {
  Serial.begin(9600);
  pinMode(6,OUTPUT);
  pinMode(7,OUTPUT);
  pinMode(8,OUTPUT);
  pinMode(9,OUTPUT);
  pinMode(10,OUTPUT);
  pinMode(11,OUTPUT);
  
  digitalWrite(6, HIGH);
  digitalWrite(7,HIGH);
  digitalWrite(8,HIGH);
  digitalWrite(9,HIGH);
  digitalWrite(10,HIGH);
  digitalWrite(11,HIGH);
}

void loop() {

  if(Serial.available())
  {
    while(Serial.available())
      {
      input = Serial.read();
     if(input == 'A')
      {
        digitalWrite(6,LOW);
        digitalWrite(7,HIGH);
        digitalWrite(8,HIGH);
        digitalWrite(9,HIGH);
        digitalWrite(10,HIGH);
        digitalWrite(11,HIGH);
        delay(150);
      }
      else if(input == 'B')
      {
        digitalWrite(6,HIGH);
        digitalWrite(7, LOW);
        digitalWrite(8,HIGH);
        digitalWrite(9,HIGH);
        digitalWrite(10,HIGH);
        digitalWrite(11,HIGH);
        delay(150);
      }
      
      else if(input == 'C')
      {
        digitalWrite(6,HIGH);
        digitalWrite(7,HIGH);
        digitalWrite(8,LOW);
        digitalWrite(9,HIGH);
        digitalWrite(10,HIGH);
        digitalWrite(11,HIGH);
        delay(150);
      }
      
      else if(input == 'D')
      {
        digitalWrite(6,HIGH);
        digitalWrite(7,HIGH);
        digitalWrite(8,HIGH);
        digitalWrite(9,LOW);
        digitalWrite(10,HIGH);
        digitalWrite(11,HIGH);
        delay(150);
      }
      else if(input == 'E')
      {
        digitalWrite(6,HIGH);
        digitalWrite(7,HIGH);
        digitalWrite(8,HIGH);
        digitalWrite(9,HIGH);
        digitalWrite(10,LOW);
        digitalWrite(11,HIGH);
        delay(150);
      }
      else if(input == 'F')
      {
        digitalWrite(6,HIGH);
        digitalWrite(7,HIGH);
        digitalWrite(8,HIGH);
        digitalWrite(9,HIGH);
        digitalWrite(10,HIGH);
        digitalWrite(11,LOW);
        delay(150);
      }
      break;
    }
    
  }
      else
      {
        digitalWrite(6, HIGH);
        digitalWrite(7,HIGH);
        digitalWrite(8,HIGH);
        digitalWrite(9,HIGH);
        digitalWrite(10,HIGH);
        digitalWrite(11,HIGH);
        delay(150);
      }
      
 }


      


      
      /*if(input == String("[1]"))
      {
        digitalWrite(6, LOW);
        digitalWrite(7,HIGH);
        digitalWrite(8,HIGH);
        digitalWrite(9,HIGH);
        digitalWrite(10,HIGH);
        digitalWrite(11,HIGH);
        delay(100);
      }
      
      else if (input == String("[2]"))
      {
        digitalWrite(6, HIGH);
        digitalWrite(7,LOW);
        digitalWrite(8,HIGH);
        digitalWrite(9,HIGH);
        digitalWrite(10,HIGH);
        digitalWrite(11,HIGH);
        delay(100);
      }
      else if (input == String("[3]"))
      {
        digitalWrite(6, HIGH);
        digitalWrite(7,HIGH);
        digitalWrite(8,LOW);
        digitalWrite(9,HIGH);
        digitalWrite(10,HIGH);
        digitalWrite(11,HIGH);
        delay(100);
      }
      else if (input == "D")
      {
        digitalWrite(6, HIGH);
        digitalWrite(7,HIGH);
        digitalWrite(8,HIGH);
        digitalWrite(9,LOW);
        digitalWrite(10,HIGH);
        digitalWrite(11,HIGH);
        delay(100);
      }
      else if (input == String("[5]"))
      {
        digitalWrite(6, HIGH);
        digitalWrite(7,HIGH);
        digitalWrite(8,HIGH);
        digitalWrite(9,HIGH);
        digitalWrite(10,LOW);
        digitalWrite(11,HIGH);
        delay(100);
 
      }
      else if (input == String("[6]"))
      {
        digitalWrite(6, HIGH);
        digitalWrite(7,HIGH);
        digitalWrite(8,HIGH);
        digitalWrite(9,HIGH);
        digitalWrite(10,HIGH);
        digitalWrite(11,LOW);
        delay(100);
      }
      
      else
      {
        digitalWrite(6, HIGH);
        digitalWrite(7,HIGH);
        digitalWrite(8,HIGH);
        digitalWrite(9,HIGH);
        digitalWrite(10,HIGH);
        digitalWrite(11,HIGH);
        delay(100);
        
      }
   }*/


##  Procedure
  1. Connecting serial ports
  2. Cooking arduino breadboard
  3. 
##  Summary 

  After connecting 6 PDLC films to each of the serial port.
##  Conclusion
  After testing with 6 PDLC films on a screen, we concluded that the basic concept of detecting light and deflecting it is possible. However, there were some criticla issues that still needed attention.
  1. Inconsistency of light detection
openCV package of python was successful of detecting lights one at a time. However, we realize that the road consists of myriad of lights, such as the traffic lights, house lights, and etc. If the product was to successfully be imported in a vehicle, it would need an AI to selectively detect the light beam directed right at the driver's vision.
  2. Controlling transparency of PDLC film
PDLC films are capable of blocking light beams, but we needed further research on controlling the transparency of the films. Since we were using Arduino serial ports, which are only capable of controlling the on/off switch of the PDLC, we were unable to control the transparency. For further improvement, the transparency of the films are pivotal so that the driver's vision doesn't get hindered.
  3. Power supply limit of Arduino pack
A small arduino package only consists battery packages, which couldn't support the PDLC power supply for much long. Importing a car battery would work, but the battery usage of the car would increase drastically, causing the car to malfunction during driving. A reliable power source seems necessary to perfect the product.
