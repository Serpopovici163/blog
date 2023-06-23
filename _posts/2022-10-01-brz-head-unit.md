---
layout: single
title:  "BRZ Head Unit"
toc: true
hidden: true
---

This page outlines the process of modifying the BRZ's stock head unit to contain custom components. I maintained the casing of the original head unit to avoid designing custom mounting hardware and alleviate the need to reverse engineer the casing/mounting point dimensions.

## Architecture

*include block diagram of design here*

## Final Product

![Head unit assembly process](/assets/img/brz/head_unit_assembly.jpg){: style="float: right; width:50%; height:80%; margin-left: 10px;"}

The updated head unit was a very satisfying thing to assemble. It effectively assembles from bottom to top as you would a layered cake, with each 'level' of electronics stacking on top of each other. Each layer as well as the components it houses is discussed more in depth in the Internal CAD section. The pictures below display the assembly process and final result:

![Head unit internals](/assets/img/brz/head_unit_internals_real.jpg)
![Final head unit](/assets/img/brz/final_head_unit.jpg)

The final product, pictured above, came out rather clean! There's a ribon cable protruding from the front that goes to the main media display and there are a few small JST-SM connectors as well as some generic I/O on the back to interface with the center console display, CAN bus, and various other things. My initial plan was to mount the display to the head unit itself, but aligning it with the trim and designing a mount that can be secured while also being removeable for a display as large as mine proved incredibly difficult. I therefore settled for mounting the display in the trim piece itself with only a ribon cable connecting it to the head unit.

I found a blog post and managed to obtain the stock wiring plugs for the head unit (Metra 70-1761) to get power and interface with the vehicle's speakers. Lastly, the head unit gets a few state-related signals from the car such as a reverse signal (to turn on the backup camera) and an illumination signal (to notify it when the running lights are on). Both of these signals are forwarded back out through the JST-SM connectors and will connect to a transceiver computer by the car's OBD port such that they can be encoded into CAN packets that are passed through my CAN network. This allows all of my hardware to keep track of their state and reduces the amount of hardware required in the head unit itself.

## CAD

The head unit CAD solely consists of a mounting framework to include all the necessary hardware within the casing of the stock head unit. I initially intended to mount the media display to the head unit itself however I ended up simply mounting it to the trim piece which saved me a ton of work. Of the two pictures included below, the first shows the CAD of the internal framework that holds all the components in the head unit whereas the second is a picture of the assembled head unit. 
![Head unit internals](/assets/img/brz/head_unit_internals.png)

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

There are 4 total brackets in the CAD design depicted above. The bottom bracket solely holds the display driver and Raspberry Pi, the second bracket holds the VIM4 alongside its NVMe SSD and CAN shield, the third holds one of the amplifiers and the display driver's power board, and the fourth holds the final amplifier alongside the 5V regulator, USB DAC, and RCA to HDMI converter. The third bracket has a gap on the right side of the image to provide clearance for the VIM4's fan.

## Behavioural Nightmares

There was no shortage of software issues when trying to get this head unit to function as a head unit. Part of the premise of this project was the idea that there was no way I could end up with less functionality than my original head unit, which would only really play bluetooth media. This statement remained rather untrue for the first couple weeks of having the new head unit installed for a variety of reasons which I will list below:

- Wireplumber: This problem took a buddy and I many hours to solve because it is not thoroughly documented and we suspected many other things to be wrong. The issue was that Wireplumber would check for a logged in user and refuse to play audio unless someone was logged in. There ended up being a config file burried in Ubuntu's filesystem that disabled this behaviour and allowed the system to play audio regardless of there being a user logged in. This issue also prevented a bluetooth device from being connected to the head unit since the Khadas VIM4 would instantaneously drop the connection as a result of there being no audio session manager active.
- Amp fine-tuning: I got cheap audio. Of course I did. The setup works but it has its flaws. The speakers in the car seem to be rather low impedance and therefore sensitive to electrical noise which is a tremendous issue with low quality amplifiers. I've mitigated this by setting the volume adjustment potentiometer of each amp just a hair above 0%. This generally works but the amplifier or USB sound card clearly clips when I try to play things at an excessive volume. I know it's not the speakers topping out, it sounds artificial and could probably be fixed if I increased the volume setting of the amp. The noise I'm referring to is a clicking sound that lines up with the status LED on the Khadas VIM4. There is no electrical noise prior to the VIM4 booting which seems to imply that the issue lies with my USB DAC.
- Phone calls: This is likely related to the Wireplumber issue but I have yet to figure out how to get phone calls working in the car. My phone recognizes the car as a bluetooth headset but the car neither plays the phone call audio or returns any audio from the microphone. I've been told this could be because ofono is installed and Wireplumber attemps to use it for phone calls instead of handling the audio forwarding itself, however I have not been able to find ofono and I frankly have yet to analyze how the VIM4 handles audio sources/sinks when a phone call is active.
- Touch screen issues: The driver for the center console display has an input for the digitizer and a USB output labeled "touch", however I do not get any touch inputs on the Raspberry Pi. This could be a software issue but it's most likely a hardware issue since the manufacturer shipped me an extra, small PCB dedicated for handling the digitizer. I will hopefully swap this in soon and see if that fixes it. My weird display architecture (large media display w/o touch and smaller center console display w/ touch) does not allow me to run Android on the head unit as I had initially planned so I am going to rewrite the UI, most likely in NodeJS with Electron.