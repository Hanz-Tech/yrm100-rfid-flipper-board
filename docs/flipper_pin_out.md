========================================
FLIPPER ZERO GPIO PINOUT REFERENCE
========================================

Pinout is viewed from the FRONT (screen facing you), starting from the top.

LEFT SIDE (Pins 1-8)
----------------------------------------
Pin 1 : 5V 
Pin 2 : PC0  GPIO 
Pin 3 : PC1  GPIO 
Pin 4 : PC2  GPIO
Pin 5 : PC3  GPIO 
Pin 6 : PB3  GPIO 
Pin 7 : PB2  GPIO 
Pin 8 : GND  (Ground)

RIGHT SIDE (Pins 9-18)
----------------------------------------
Pin 9 : 3.3V   
Pin 10 : SWC
Pin 11 : GND
Pin 12 : SIO
Pin 13 : TX
Pin 14 : RX
Pin 15 : C1
Pin 16 : C0
Pin 17 : 1 Wire
Pin 18 : GND  (Ground)
========================================
NOTES
========================================
- I/O pins are +3.3V tolerant.
- Do NOT connect 5V to the I/O pins.
- UART Bridge (USB-UART):
  TX -> Pin 15
  RX -> Pin 14