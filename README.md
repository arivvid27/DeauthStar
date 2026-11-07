# DeauthStar

i got kinda bored one day and decided i wanted to go absolutely massive on a custom pcb project and cybersec. i already built a custom keyboard from scratch as my first shot at hardware, but i wanted a real challenge. this is an enterprise-grade, high-performance hacking and security analysis appliance driven by a full linux core and a real-time hardware coprocessor.
its basically a phone-sized beast that combines desktop-class packet processing with hardware timing features that standard microcontrollers just cant touch.

## Why I Built It

most handheld pentesting tools you buy off the shelf are seriously limited. they use weak, single-channel 2.4 ghz chips, drop packets bc they run out of memory, or they can only do one task at a time before freezing up.
i wanted something with actual power. the DeauthStar uses a dual-chip setup: a Raspberry Pi Compute Module 5 running a minimal headless linux kernel, paired with an stm32 dual-core real-time engine. with 8gb of ram, it can capture and buffer millions of raw network packets directly into memory without dropping a single frame, and it looks like a premium field meter using a custom mechanical corner wheel for navigation.

## What It Natively Does (Main Module)

even without any expansion blades attached, the main module has serious hardware capability built straight into the custom 4-layer pcb:
- Multi-Platform BLE Flooding & Spoofing: natively crafts and transmits real-time proximity pairing loops that simultaneously target apple, android fast pair, and windows swift pair.
- High-Frequency 13.56 MHz NFC: reads, writes, cracks, and emulates nfc tags and badges (like MIFARE Classic) directly through the back of the case.
- 125 kHz Low-Frequency RFID: uses a discrete analog op-amp envelope detector circuit to sniff and clone older building proximity access cards.
- Infrared Learn & Transmit: captures raw tv, ac, and projector remote control pulse lengths to save and replay them.
- Single-Cable Desktop Docking: fully routed usb-c port that supports displayport alternate mode, high-speed data streaming, and power delivery negotiation. when you plug it into a laptop, it acts as a network tap streaming live monitor traffic into wireshark.
- Hard-Kill Safety Switch: features a low-resistance p-channel mosfet power switch circuit triggered by a physical mechanical edge slide-switch. you can run the device purely off desk power while keeping the lithium battery safely isolated.

## The Modular Expansion System

the back of the device features a 20-pin Samtec mezzanine dock port. this passes out internal usb 2.0 channels and the stm32's high-speed spi/uart buses so i can snap on specialized modules without making the main board a messy routing nightmare.
the expansion blades live under this same repository:

### 1. The Network Auditor Blade (Pineapple Mode)

an independent board featuring an onboard microchip usb hub controller that splits the dock's usb line into (hopefully) two high-power MediaTek dual-band wireless transceivers. one radio acts as a rogue honeypot access point running hostapd while the second radio handles continuous, line-rate promiscuous sniffing and packet injection across 2.4 ghz and 5 ghz networks.
- **Active Wi-Fi Deauth Intrusion Detection:** works with the network blade to monitor the airwaves for rogue `0x0C` management frames. if it catches an attack, it bypasses the main cpu and triggers a hardware-level panic mode—flashing a warning modal on the 1024x600 screen and firing an onboard smt magnetic buzzer loop so you can physically track down the source. It uses the buzzer that i will have on the main board. thought it's worth putting it under here instead of the main board.


### 2. The Sub-GHz Signal Lab Blade

a separate rf blade built around the Texas Instruments CC1101 transceiver. i might get something else based on if i feel like learning more stuff or not. it bypasses the wi-fi noise entirely by snapping directly onto the stm32's spi bus lines on the dock, allowing for microsecond-precise capturing, cloning, and auditing of garage doors, gate remotes, and sub-ghz protocols.
