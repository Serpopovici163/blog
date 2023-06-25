---
layout: single
title:  "BRZ Wiring"
toc: true
hidden: true
---

Don't even get me started.

![Wiring 1](/assets/img/brz/wiring_1.jpg){: style="float: left; width:50%; height:80%;"}
![Wiring 2](/assets/img/brz/wiring_2.jpg){: style="float: right; width:50%; height:80%;"}

![Wiring 3](/assets/img/brz/wiring_3.jpg){: style="float: left; width:50%; height:80%;"}
![Wiring 4](/assets/img/brz/wiring_4.jpg){: style="float: right; width:50%; height:80%;"}

![Wiring 5](/assets/img/brz/wiring_5.jpg)
![Wiring 2](/assets/img/brz/wiring_6.jpg)

## General overview

This page is somewhat just documentation for myself with regards to connector pinouts but also contains some general information lower down.

I bought a bunch of 'waterproof' connectors off of AliExpress for this project. I say waterproof in quotes because I disconnected one of the connectors at some point and it was filled with water. Regardless, I ordered a variety of connectors with different sizes and pin numbers for various purposes. Their purpose and pinout is included below:

- SP13 (4-pin) --> Used for power and USB data to cameras outside the cabin.
1. GND
2. PWR
3. D+
4. D-
- SP20 (2-pin) --> Used for audio or power, one for the siren speaker up front and others will be installed wherever power is needed outside of the cabin.
1. GND
2. PWR
- SP20 (3-pin) --> Used for power and data to addressable LEDs outside the cabin.
1. GND
2. PWR
3. DAT
- SP20 (4-pin) --> Used for power and CAN outside the cabin.
1. GND
2. PWR
3. CAN_HI
4. CAN_LO

## Network

![Diagnostic Port](/assets/img/brz/diagnostic_ethernet_port.jpg){: style="float: right; width:50%; height:80%;"}

The car has an ethernet-based network to connect its various computers and an ethernet-based camera up front. This network also provides WiFi to passengers and internet access to the aforementioned devices. The [WiFi-LTE modem](https://www.aliexpress.com/item/4001224227702.html) I purchased only has 4 ports available but I need 5 to connect the three computers, camera up front, and a diangostic port included in the dash for debugging purposes. The slot below the ethernet jack was meant to be a USB-C port for tethering my phone and running Android Auto but I have not installed the port in the car just yet.

![LTE modem](/assets/img/brz/LTE_modem.jpg)
![Ethernet switch](/assets/img/brz/ethernet_switch.jpg)

Above are pictures of the network setup in the trunk. The LTE modem is mounted to the top of the trunk with double-sided tape and zip ties whereas the ethernet switch is left on the trunk floor. The extra wiring strapped to the modem is what I had used to power it prior to installing the fusebox and I have yet to remove it. The switch position is not finalized which is why the wires are left bundled up for the time being.

## Power

I installed a separate fuse box in the trunk to power a few things, namely:

- The subwoofer;
- The rear defence computer (not yet installed);
- The networking hardware;
- The CAN network;
- The LightLink module; and
- The vision computer

Thanks to the fusebox, I can add individual fuses to all of these systems. I ran an 8-gauge wire from the battery to this fusebox, with an inline fused installed immediately next to the battery as well as another installed on the other end, in the fusebox.

## CAN

For CAN, I am using a rather thick (18-gauge) wire intended for RS485 that contains two twisted pairs. The CAN network begins in the trunk, where it obtains power from the rear fusebox. It then proceeds along he center console, goes to the head unit, then under the steering clumn, after which it makes its way under the hood to interface with things like the thermal camera.