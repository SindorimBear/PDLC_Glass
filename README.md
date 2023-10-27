# Night time light-detecting safety glass 
A prototype for a product that detects high-intensity light to protect the driver's vision

## Table of Contents
1. Briefing
2. Theory
3. Code
4. Procedure
5. Summary
6. Conclusion

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
```
from imutils import contours
from skimage import measure
import numpy as np
import imutils
import cv2 as cv
import serial
import time
```
These are the necessary packages for the code to run.

imutils package functions as a basic image proccessing method. The function is necessary for two reasons: 
-resize and divide the video capture images
-add a contour to track the driver's vision.

skimage package was necessary for the same reason as imutils


The following code is where the USB serial port for Arduino will be called into python.
The first line calls in the serial ports as the variable ser.
The second line will print "detect_multiple.py launched" if the serial port is properly recognized. Otherwise, the code will return a syntax error.
Due to the battery supply of the serial port and Arduino Nano takes time to give and take signals to each other, it needs a break in the middle.
```
ser = serial.Serial('/dev/ttyUSB0',9600)
print("\ndetect_multiple.py launched\n")
print("\n============time sleep 1===========\n")
time.sleep(1)
```
detect_multiple function does the following operations:
1. Detect the brightest light in the caemra view by grayscaling, dilating and eroding the image. Doing this will remove unnecessary noises from the image. For more information, read the following article about eroding and dilating in opencv:  https://docs.opencv.org/3.4/db/df6/tutorial_erosion_dilatation.html
2. Track the brightest light in image and return the coordinates by masking the image. I used contours to add a new layer to the image and return the brightest area from the image. For more information, you can visit the link: https://docs.opencv.org/4.x/dd/d49/tutorial_py_contour_features.html
3. Calculate the position of the circle and return which area the circled contour is at. The returned position will signal the arduino nano code and shut down it's power supply, causing it to block the light.
```
def detect_multiple(image) : 
    gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY)
    blurred = cv.GaussianBlur(gray, (51, 51), 0)
    thresh = cv.threshold(blurred, 245, 255, cv.THRESH_BINARY)[1]

    thresh = cv.erode(thresh, None, iterations=2)
    thresh = cv.dilate(thresh, None, iterations=4)

    labels = measure.label(thresh, connectivity=2, background=0)
    mask = np.zeros(thresh.shape, dtype = "uint8")

    # loop over the unique components
    for label in np.unique(labels):
        if label == 0 :
            continue
            
        labelMask = np.zeros(thresh.shape, dtype="uint8")
        labelMask[labels == label] = 255
        numPixels = cv.countNonZero(labelMask)
        if numPixels > 300:
            mask = cv.add(mask, labelMask)

    cnts = cv.findContours(mask.copy(), cv.RETR_EXTERNAL, cv.CHAIN_APPROX_SIMPLE)
    cnts = imutils.grab_contours(cnts)
    cnts = contours.sort_contours(cnts)[0]
    
    lst = []
    time.sleep(0.1)
    print("\n============time sleep 0.1===========\n")
    # ser.write(b'a')
    for (i, c) in enumerate(cnts):
        # Draw the bright spot on the image
        (x,y,w,h) = cv.boundingRect(c)
        spot = destination(x,y,w,h)
        
        if (spot is not None):
            if spot not in lst:
                lst.append(spot)
            print("Detected Square : ", spot)
            ser.write(int2bin(spot))
            print(lst)
        ((cX, cY), radius) = cv.minEnclosingCircle(c)
        cv.circle(image, (int(cX), int(cY)), int(radius), (0,0,255), 3)
        cv.putText(image, "#{}".format(i+1), (x,y-15),cv.FONT_HERSHEY_SIMPLEX, 0.45, (0,0,255),2)
    offSpot(lst)
    return image
```
The code below divides the image into 6 sections and return the value to the detect_multiple function.
```
def destination(x,y,w,h):
    dest_x=x+w/2
    dest_y=y+h/2
    if dest_x<213 and dest_y<240:
        return 1
    elif dest_x<213 and dest_y>=240:
        return 2
    elif 213<=dest_x<426 and dest_y<240:
        return 3
    elif 213<=dest_x<426 and dest_y>=240:
        return 4
    elif 426<=dest_x<640 and dest_y<240:
        return 5
    elif 426<=dest_x<640 and dest_y>=240:
        return 6
    else:
        pass
```
Since the arduino program only receive values as binary code, the integer in python needed to be altered into binary function manually.
```
def int2bin(i):
  if i == 1:
        return b'1'
    elif i == 2:
        return b'2'
    elif i == 3:
        return b'3'
    elif i == 4:
        return b'4'
    elif i == 5:
        return b'5'
    elif i == 6:
        return b'6'
    else :
        return 
```
2. main.py
This file activates the camera. If the camera is properly activated, detect_multiple.py will be called in. Otherwise, it will return an error.
```
import Detect.detect_multiple as detect
import cvTools
import cv2 as cv

# module import
def main() :
    # Reading capture device
    capture = cv.VideoCapture(0)

    while True:
        isTrue, frame = capture.read()
        
        try:
            detect_frame = detect.detect_multiple(frame)
            
            cv.imshow('Video', detect_frame)


            if cv.waitKey(1) & 0xFF==ord('d'):
                break
        except:
            cv.imshow('Video', frame)
            if cv.waitKey(1) & 0xFF==ord('d'):
                break
    capture.release()
    cv.destroyAllWindows()

    return 0

if __name__ == "__main__":
    if main() == 0:
        print("\nLight_Detecting terminated successfully!")
    else:
        print("\nThere is something wrong. I recommend you to kill every processes which is related to this program.")
    
```
   
  ###  B. Arduino 
The arduino nano used in this experiment had 6 serials connected to it. Whenver the signal is returned from the python code, the serial port will be deactivated. Since the PDLC film turns invisible when electicity flows, the arduino code acts as a on/off switch.
  ```
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
```

##  Procedure
  1. Connecting serial ports
  2. Cooking arduino nano breadboard
  3. Connect PDLC films to the serial port
  4. Connect Camera with Computer
  5. Connect the arduino nano with computer
  6. Run Python code from terminal
  7. Flash light beams on each
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
