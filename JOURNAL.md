---
title: "PiRPKey"
author: "@Hash-ak"
description: "A small, fully-open-source security skey upporting multiple protocol (WIP project)"
created_at: "2025-05-26"
---
# Journal
**Total : 3:05**
## Day 1 (26 May 2025)

Read about FIDO U2F and WebAuthnn
https://www.yubico.com/products/how-the-yubikey-works/


Today I started planning/bbrainstorming/researching for my project. I tested [Fidelio](https://github.com/danielinux/fidelio) on my Orpheus Pico to learn more about U2F/FIDO. 
I'm also following [Raspberry's official Getting Started With The Pico](https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf) guide to learn how to use it and program using the VSCode extension.


**Research : 1:30 min**  
**Code : 30 min**  


## Day 2 (28 May 2025)

I'm trying to get my Orpheus Pico to print on an LCD 16x2 (non I2C). It may seem odd but that's because I'm (trying) to make up a hardware base to test a very rudimentary TOTPs generation mechanism on, and I need a display (technically could use serial but dispaly is cooler). I would really had loved to test o a 0.91" OLED (what I'll use for the board, when I design it), but I don't have any so I'm forced to use a 16x2 LCD.  
Saddly it doesn't seems to work at all. I tried [this libary](https://github.com/martinkooij/pi-pico-LCD) with some minor modification to the circuit. I'm really stuck so I tried on an Arduino Uno with the Arduino IDE/libraries (good ol' Arduino). Work _fine_, with the exact same LCD and circuit (had to change the pisn in the code that's really all)  
![Image of the Arduino displaying on the LCD](/assets/LCDWithArduino.jpg)  
_Arduino running the LCD beside the non-working Orpheus Pico_  



Finally, after asking my favourite AI, [Perplexity](https://perplexity.ai), to try to help me debug, it gave me a fully-contained code and circuit that worked. I'm not 100% sure if the problem was hardware-related or if it was just that the library's example I used was not okay, so I'm trying to test other stuff with the same pins.  
![Image of the pico running the LCD](/assets/PicoRunningLCD.jpg)  
_The Orpheus Pico finally running the LCD with the AI-generated code!! Wooo-hoo!!! 🎊_

Even better, I tried to run a simple code from the Arduino IDE (that I configured correctly so I can flash the code on my Pico), _and it worked even tho I tried the **exact** same code earlier but with different pins connected._ The problem definitely look like it was some pins problem (either I wrongly connected wires multiple times in a row or the GPIOs that I used where faulty, I'm not sure  
Here's the Arduino code I used :  
```C
#include <LiquidCrystal.h>

// Pin mapping: LiquidCrystal(rs, en, d4, d5, d6, d7)
LiquidCrystal lcd(16, 17, 18, 19, 20, 21);

void setup() {
  lcd.begin(16, 2); 
  lcd.print("Hello, Pico!");
  lcd.setCursor(0, 1);
  lcd.print("LCD Test OK");
}

void loop() {
}
```  
Now I only need to test if [the libary for the pico I mentionned earlier](https://github.com/martinkooij/pi-pico-LCD) works now.



**Electronic : 50 min**  
**Code : 15 mins**

## Brainstorm 
For now v1 will be :

   - RP2040 based
   - 16 mb of flash
   - OLED display for OTPS/menu
   - Many entry to switch between sites otps, change settings, etc
   - Support fido/fido 2, u2f, TOTP



WORKFLOW for TOTP
The user setup 2fa on the website. He take the encoded secret, then plug in he's key, enter the decoding pin, and select "Mount ad usb in the menu" .


Back on his pc, he open the usb drive, search for the otp.json that just got decrypted, and add a new field ( something like "sites :Google:com. Secret : xxxxxxxxxxx)

He then unmount the key, which reboot (ask for pin to re encrypt the pins?)

Now, when he wants to log in that same site; he just scroll troughs his otp in the display, go on that sites otp, and then manually enter it on the website before th stone expire, as showed by a progression bar/countdown timer on the display

He can also just go in the correct field and press the "auto type otp" button

