---
layout: single
title:  "BRZ Head Unit"
toc: true
hidden: true
---

This page outlines the process of modifying the BRZ's stock head unit to contain custom components. I maintained the casing of the original head unit to avoid designing custom mounting hardware and alleviate the need to reverse engineer the casing design.

## Architecture

*include block diagram of design here*

## CAD

The head unit CAD is split into two parts: the internal components (SBCs, amplifiers, etc.) and the external components (display and driver-facing camera). 

### Internal CAD

The inside of the OEM head unit's casing has been redesigned to utilize a series of stackable brackets providing mounting points for the following components:

1. Khadas VIM4 --> Linux backend computer
2. USB 5.1 sound card --> Required since Khadas VIM4 does not have a DAC onboard
3. RCA to HDMI converter --> Used to convert thermal camera to HDMI since Khadas VIM4 has an HDMI in
4. NVMe SSD --> used for dashcam/blackbox purposes
5. SPI CAN shield --> connects Khadas VIM4 to the CAN bus
6. Raspberry Pi 4B --> Android frontend computer
7. Head unit display driver boards --> two boards, one for power delivery and another for logic
8. 2CH amplifier boards (2 of these) --> amplify audio for FL, FR, RL, and RR channels
9. 5V regulator --> provides 5V for Khadas VIM4, Raspberry Pi, USB DAC, and RCA to HDMI converter
10. Voltage splitter --> splits battery voltage for display driver, amplifiers, and 5V regulator which do not require upstream power regulation

There are 4 total brackets in the CAD design depicted to the right *PUT UPDATED PIC ONCE RCA to HDMI thingy is in there*. The bottom bracket solely holds the display driver and Raspberry Pi, the second bracket holds the VIM4 alongside its NVMe SSD and CAN shield, the third holds one of the amplifiers and the display driver's power board, and the fourth holds the final amplifier alongside the 5V regulator, USB DAC, and RCA to HDMI converter. The third bracket has a gap on the right side of the image to provide clearance for the VIM4's fan.

The entire assembly mounts to three screw holes on the bottom of the head unit casing as well as two screw holes halfway up each side of the casing. An additional bracket, *pictured below*, mounts to the rear of the head unit and holds the stock power in/speaker out connector as well as other various I/O connectors for interfacing with the microphone, the CAN bus, and the secondary display of the head unit. All other connections go directly into their respective boards. 



The head unit bracket is a perfect example of this technique. The design was built symmetrically, and as such, I began by modelling the left side before mirroring it to generate the entire assembly. I knew I would need two 'platforms' to house all the required components so I began by measuring the mount hole positions from the old head unit and generating two basic mount 'ears' before extruding an arbitrary platform attached to each ear. The platforms were extended an arbitrary amount in both directions based on how much space I estimated to be available inside the vehicle's cavity and how far the head unit display would be mounted. The head unit bracket holds the following hardware as can be seen in the *PIC* to the right:

1. Head unit display
2. Head unit display driver
3. Khadas VIM4
4. Voltage regulator
6. Driver-facing camera

No amp needed up front since it's in the trunk from factory. That being said it only drives the front speakers and I can't figure out how the rear side speakers are powered so there may be more amplifiers throughout the vehicle. I also need to include an amplifier for a loudspeaker up front which may end up in the head unit if there is space.