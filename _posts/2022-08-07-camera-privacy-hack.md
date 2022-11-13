---
layout: single
title:  "Security Camera Privacy Hack"
classes: wide
---

## Enhancing privacy of cheap security cameras
# get picture of whole camera to put here as well

I've always been a little paranoid of having IoT devices live streaming views of my living space however I have a long trip coming up so a couple security cameras would be nice to keep an eye on my place. I considered a couple ways of modifying the cameras to provide more privacy: 
- A Python script that forges HTTP requests and orders the camera to turn around;
- A physical shutter external to the camera that blocks the lens; and
- An internal relay that cuts power to the camera module.
Unfortunately these were not ideal. The first idea was unreliable because turning the camera through a script still allows the camera to turn itself back on its own and may not be reliable should a software update be pushed that depreciates my integration of the camera's API. The second was not an elegant solution and would be difficult to integrate on a PTZ camera that moves around. Finally, the third did not work because the camera would not come back online after the camera module got disconnected from its motherboard, no matter what I tried. In the end I settled on modifying the camera's internal IR-CUT filter by covering the IR-CUT part with a sticker and integrating a microcontroller to toggle the state of the filter assembly. This was an elegant and invisible solution that should not noticeably affect image quality since the camera is being used indoors so there should be very little ambient IR light.

The privacy mode of these cameras is toggled using a Python service running on a Linux server I have. The script regularly monitors what devices are connected to the local WiFi network, and once a known device is connected, the Python service blinds any security cameras within the apartment.

# Hardware

While understandably not the most secure way to go about this, I used a few NodeMCU 1.0 modules I had lying around for this project. A more ideal solution would be to either use separate transceivers to air-gap the privacy microcontrollers from the internet or to use a WiFi/Bluetooth capable microcontroller that can sniff and determine what devices are around it. Regardless, the hardware here is relatively simple, the camera has:
- A ESP-12E module;
- A WS2812B LED; and
- A MX1508 H-bridge IC.
The MX1508 was needed since the ESP operates at 3.3V however the IR-CUT filter mechanism requires a higher voltage to actuate. The LED provides a hardcoded correlation between the state of the IR-CUT filter and a visual indication of whether the camera is able to see anything.

# Code