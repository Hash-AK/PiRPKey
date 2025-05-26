# PiRPKey
A small, fully-open-source security supporting multiple protocols (WIP project)


# Overview
This project aims to make a fully open source security key, supporting multiple protocol, like FIDO/U2F and TOTP.
It will use a small OLED display to show the TOTPs of the different registered website the user has entered, FIDO capability for WebAuthn and have a battery for mobile TOTP use.  
It is made as a project in [HackClub's Highway event](https://highway.hackclub.com)

# What I would want for V1

- FIDO 1.2/2 cabability
- 0.91" OLED screen to show TOTPs
- Small menu to set up different things, like emergency wipeout, timeout, preferred websites, auto-type of the TOTPS, setting up the device's PIM (more about that in the next point)
- Use of a PIN to encrypt/decrypt the TOTPS stored for easy and secure TOTP secret storage
- RP2040 as the MCU
- 16MB of flash
- Small battery for mobile TOTP usage
- Secure/travel mode that require PIN to show TOTPs/unlock the device

