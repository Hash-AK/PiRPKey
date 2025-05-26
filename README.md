# PiRPKey
A small, fully-open-source security supporting multiple protocols (WIP project)


## Overview
This project aims to make a fully open source security key, supporting multiple protocols, like FIDO/U2F and TOTP.
It will use a small OLED display to show the TOTPs of the different registered websites the user has entered, FIDO capability for WebAuthn and have a battery for mobile TOTP use.  
It is made as a project in [HackClub's Highway event](https://highway.hackclub.com)

# #What I would want for V1

- FIDO 1.2/2 cabability
- 0.91" OLED screen to show TOTPs
- Small menu to set up different things, like emergency wipeout, timeout, preferred websites, auto-type of the TOTPs, setting up the device's PIN (more about that in the next point)
- Use of a PIN to encrypt/decrypt the TOTPS stored for easy and secure TOTP secret storage
- RP2040 as the MCU
- 16MB of flash
- Small battery for mobile TOTPs usage
- Secure/travel mode that require PIN to show TOTPs/unlock the device


## Status
There is no schematic/code yet because hardware design, firmware, and documentation are all works in progress, I just created it for the Journal for now

## License
This work is licensed under CERN-OHL-S v2. 
