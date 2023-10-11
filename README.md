# Night time light-detecting safety glass 
A prototype for a product that detects high-intensity light to protect the driver's vision

## Table of Contents
* Briefing
* Theory
* Code
* Summary
* Conclusion

## 1. Briefing
Problem: Headlights for carss are important for illuminating night-time road. They can provide the driver a clear vision of what is happening ahead and allows them to avoid any accidents. However, some types of cars have brought forth high-intensity headlights, which are causing unneccessary car accidents.
  High-intensity headlights and LEDS can cause temporary blindess to other drivers, much like the sun glaring into our eyes during daytime. These high-intensity headlights makes it impossible to clearly view objects moving directly in front of the driver, which makes the accdidents more fatal than the sunlight during daytime.
  Another problem is that LED headlights look more bluer in the output spectrum than halogen. The human eye have difficulty processing blue light, since blue wavelengths tend to focus just ahead of the retina rather than on it.
  These headlight problems have posed threats to a broad-range of drivers and appeared to be a problem yet to be resolved.

Solution Idea: We decided to invent a glass that could prevent such casualities. The class will detect high intensity lights or LEDS, send the coordinates for the position of the light input, and diminsh the light long enough to prevent problems for the driver.

## 2. Theory 
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

##  3. Code 
  ###  A. Python 
  ###  B. Arduino 
##  4. Summary 

##  5. Conclusion 
