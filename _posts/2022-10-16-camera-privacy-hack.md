---
layout: single
title:  "Security Camera Privacy Hack"
classes: wide
---

## Enhancing privacy of cheap security cameras

I've always been a little paranoid of having IoT devices live streaming views of my living space however I have a long trip coming up so a couple security cameras would be nice to keep an eye on my place. I considered a couple ways of modifying the cameras to provide more privacy: 

- A Python script that forges HTTP requests and orders the camera to turn around;
- A physical shutter external to the camera that blocks the lens; and
- An internal relay that cuts power to the camera module.

Unfortunately these were not ideal. The first idea was unreliable because turning the camera through a script still allows the camera to turn itself back on its own and may not be reliable should a software update be pushed that depreciates my integration of the camera's API. The second was not an elegant solution and would be difficult to integrate on a PTZ camera that moves around. 
![Final camera with LED](/assets/img/foscam-hack/whole_camera.jpg){: style="float: right; width:50%; height:80%;"}
Finally, the third did not work because the camera would not come back online after the camera module got disconnected from its motherboard, no matter what I tried. In the end I settled on modifying the camera's internal IR-CUT filter by covering the IR-CUT part with a sticker and integrating a microcontroller to toggle the state of the filter assembly. This was an elegant and invisible solution that should not noticeably affect image quality since the camera is being used indoors so there should be very little ambient IR light. The final state of the camera is pictured to the right with a status LED jankily integrated below the FOSCAM logo.

The privacy mode of these cameras is toggled using a Home Assistant - Ring integration that allows the cameras to see when the Ring alarm is armed. 

# Hardware

While understandably not the most secure way to go about this, I used a few NodeMCU 1.0 modules I had laying around for this project. A more ideal solution would be to either use separate transceivers to air-gap the privacy microcontrollers from the internet or to use a WiFi/Bluetooth capable microcontroller that can sniff and determine what devices are around it. Regardless, the hardware here is relatively simple. The camera has:

- An ESP-12E module;
- A WS2812B LED; and
- A MX1508 H-bridge IC.

The MX1508 was needed since the ESP operates at 3.3V however the IR-CUT filter mechanism requires a higher voltage to actuate. The LED provides a hardcoded correlation between the state of the IR-CUT filter and a visual indication of whether the camera is able to see anything. The code for this is rather simple and included on my GitHub.

![IR cut assembly](/assets/img/foscam-hack/IR_cut_assembly.jpg){: style="float: left; width:50%; height:80%;"}
![MX1508 wired](/assets/img/foscam-hack/MX1508_wired.jpg){: style="float: right; width:50%; height:80%;"}
![ESP position](/assets/img/foscam-hack/ESP.jpg){: style="float: left; width:50%; height:80%;"}
![ESP tucked in](/assets/img/foscam-hack/ESP_tucked_in.jpg){: style="float: right; width:50%; height:80%;"}

The first picture (top left) depicts the IR cut filter before I modified it by adding tape to one of the transparent partitions. The second depicts the wiring for the H-bridge, nothing too complex there. The H-bridge was covered in tape to prevent it from shorting with anything else in the camera and then placed in the back of the camera's head, next to the motor (not pictured). The ESP was tucked in above the motherboard of the camera and leeches power off of the motherboard for itself. The camera receives regulated 5V at its DC input jack so no special considerations were needed.