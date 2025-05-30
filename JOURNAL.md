---
title: "PiRPKey"
author: "@Hash-ak"
description: "A small, fully-open-source security skey upporting multiple protocol (WIP project)"
created_at: "2025-05-26"
---
# Journal
**Total : 6:45**
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

## Day 3 (29 May 2025)

So as my LCD fully work with martinkooij's libary and Arduino's one, I'm reasdy to start coding! AS I'm not really an expert in C/C++ I want to first try to make a small mockup of a TOTP generation mechanism, so very very basic, not encrypted in memory, etc.

The main problem is that the TOTP generation mechanism require the current date & time, but as there is no wifi, the time on the pico (and by extension, the board I'll design) always reset at boot. So either I will need to :

- Use NTP (but require wifi so i cant test on my orph pico, and my board will need to have wifi module so be bigger)/a onboard external RTC (again, will add to the size), or
- The user will need to set it up at each reboot (if I put a battery it _shouldn't_ be that often but still), **OR**,
- I try to implement something to get the time via serial from the PC where the pico/key is plugged (but will require some sort of 'companion app'

Considering those options, I think that no 2 is the best for this use case. 
AS I'm just going to try to make a  mockup TOTP generation thing on my LCD, I will just hardcode dates & time (as shown in [RPI's official examples](https://github.com/raspberrypi/pico-examples/blob/master/rtc/hello_rtc/hello_rtc.c), in the future of course so it will ocver the compilation time, and i just fire up my pico when the true date come so it can generate TOTPs.



**Code : 40 min**

## Day 4 (30 May 2025)  

Today I decided that, beside trying to start the TOTP generation code, I will also try to start of the PCB design in KiCAD. To help me out, I foudn a few ressources, [like RPI's official guide](https://datasheets.raspberrypi.com/rp2040/hardware-design-with-rp2040.pdf) or [this blog article](https://deepbluembedded.com/raspberry-pi-pico-rp2040-schematic-pcb-design-in-kicad/), wich will highly help me making the basic (decoupling, wiring up the flash, etc), and that I will be able to use as a base for my project (like adding more inputs/leds, adding the OLED screen, maybe adding NFC support (if I don't leave that for the v2).

As I'm working on a USB-stick-format PCB I will use USB male plug (duh, who wants to have to use a cable to plug it to their PC), but most guide use female/receptacle USB. Normally I think I shouldn't have too much adapting [the oen I choose](https://jlcpcb.com/partdetail/383026-918118A2021Y40006/C399938), still new to that tho.

After using EasyEDA2KiCAD I was able to import the symbol in KiCAD. So I'm wiring up the connection in schematics. 


I also found [this awesome gist](https://gist.github.com/sm-Fifteen/df1a94b6b6e0670e0b5a0c362ef2faa2) explaining what are Yubikeys

**Schematics + Research : 3:00 min**  


## Brainstorm 
For now v1 will be :

   - RP2040 based
   - 4 mb of flash (originally planned for 16 but that's prob overkill considering [that apparently even Yubikey 5 have only 512kb of flash](https://gist.github.com/sm-Fifteen/df1a94b6b6e0670e0b5a0c362ef2faa2)
   - OLED display for OTPS/menu
   - Many entry to switch between sites otps, change settings, etc
   - Support fido/fido 2, u2f, TOTP



WORKFLOW for TOTP
The user setup 2fa on the website. He take the encoded secret, then plug in he's key, enter the decoding pin, and select "Mount ad usb in the menu" .


Back on his pc, he open the usb drive, search for the otp.json that just got decrypted, and add a new field ( something like "sites :Google:com. Secret : xxxxxxxxxxx)

He then unmount the key, which reboot (ask for pin to re encrypt the files?)

Now, when he wants to log in that same site; he just scroll troughs his otp in the display, go on that sites otp, and then manually enter it on the website before th stone expire, as showed by a progression bar/countdown timer on the display

He can also just go in the correct field and press the "auto type otp" button

