---
layout: single
classes: wide
title:  "BRZ LightLink"
toc: true
hidden: true
---

## LightLink Overview

Control of a vehicle's external lighting is safety critical, so I've settled on designing a fully-redundant custom circuit board with 12 MOSFET-driven channels to handle 'dumb' lights and 8 addressable channels to handle a set of addressable LED arrays retrofitted throughout the vehicle. 

In addition to designing a fully-redundant board, I felt it necessary to minimize the number of functional components in an effort to limit design complexity and reduce the risk of component failure. All lights within the vehicle share a common ground which means they need to be toggled using a high side switch, ergo P-channel MOSFETs. The problem with this is that P-channel MOSFETs are off when the gate voltage high relative to *their* source which is the car battery whereas the Arduinos operate on regulated 5V. A typical solution for this would be to include some form of transistor to toggle the P-channel MOSFET which would effectively double the components required for a fully redundant solution. 

![Picture of plan](/assets/img/brz/weird_arduino_p_channel_mosfet.png){: style="float: right; width:50%; height:80%; margin-left: 10px;"}

This was not acceptable for me so I did some research and found the picture depicted here to the right. This schematic works by connecting the Arduino's 5V to the MOSFET supply's 12V but keeping the grounds disconnected. In this way, the Arduino can toggle the MOSFET when it sets digital pin 6 to LOW which appears as -5V to the MOSFET and should be enough to fully excite it. This may work however, in order to achieve proper redundancy, I want to know when either MOSFET fails. For this reason, I need some way to sense the output voltage of each MOSFET so I asked around and eventually converged to the solution of decoupling the 5V and 12V grounds using a capacitor at which point I could reference the 12V ground with the Arduino to measure potential at the MOSFET's drain. This design fell through since MOSFETs have an inherent capacitance between their gate and source that needs to be charged when their state is toggled. Due to the lack of common ground, it would be difficult to guarantee predictable behaviour when switching the MOSFET's state resulting in my final solution.

The design explained below follows the same principle of decoupling the voltage rails however a decoupling capacitor is now included on the positive sides of the 5V and 12V rails which allows both components to maintain a common ground. Additionally, this tremendously simplifies the sensing functionality since I no longer need to expend any effort converting a potential from the 12V 'domain' to the 5V 'domain'; I can simply connect an analog pin to the potential I wish to measure as long as it is less than 5V.

### Redundant MOSFET Schematic

MOSFET SCHEMATIC

Two MOSFETs are included in parallel, both of which are immediately followed by a Schottky diode to prevent backflow from the other MOSFET. This is necessary because a diagnostic pin is included prior to the Schottky and senses voltage which would be biased by backflow from the other MOSFET therefore hindering the detection of a MOSFET failure. The diagnostic connections use a summing amplifier with a op-amp IC as pictured below such that the output signal ranges between 0V and 3V with the Qx MOSFET contributing 80% of the output voltage and Qx only contributing the other 20%. This allows the microcontroller to detect the functionality of each individual MOSFET regardless of their parallel arrangement.

MOSFETS SCHEMATIC

For control, two Arduino Mega EMBEDs are inclded. These were chosen because I have about a dozen on hand right now and they have been reliable in my experience. Regardless, two Arduinos with separate CAN transceivers are included on the board as depicted in the schematic below:

OVERALL SCHEMATIC

At any given moment, one of the Arduinos is considered the 'master'. The master is in charge of outputting signals to the MOSFET and addressable channels while simoultaneously communicating its state over to the slave Arduino over a UART connection. The master updates the slave at regular intervals such that the updates themselves function as 'heartbeat' signals assuring the slave that the master is operating optimally. Should the master miss a heartbeat for whatever reason, the slave sends a hardware reset pin to the current master after which it assumes the master role and resumes signal outputs based on the last state update it received over UART. This system also allows the Arduinos to swap roles ever 30 days since my asynchronous code uses the millis() function to keep track of time and its value will exceed the maximum possible variable size in approximately 50 days.

### Lighting Modifications

Some modifications require extensive control of the vehicle's lights however I am not willing to alter anything about the car's computers so my solution consists of placing relays along the wiring harnesses controlling the vehicle's lights with two separate computers on each end of the car. Each computer has a combination of mechanical and solid state relays depending on how frequently a light is expected to toggle on/off. Mechanical relays are used for lights that rarely toggle and because they provide a normally closed and normally open set of contacts. This is important because the lighting computers receive constant power regardless of the vehicle's state and therefore power draw must be minimized when the vehicle is not running.

![Rear light manager computer](/assets/img/brz/rear_light_manager.jpg){: style="float: right; width:50%; height:80%; margin-left: 10px;"} The rear lighting computer is pictured to the right and I expect the front computer to be highly similar. It uses four solid state relays (top left) to drive the brake lights and turn signals of the car by connecting them to the car's battery. The third and fourth brake light of the car (third is in the windshield and fourth is F1 style in bumper) are converted to addressable LEDs and do not require relays. Addressable LEDs have also been integrated into the reverse lights (two lights adjoined to the fourth brake light) and the two rear quarter panel windows of the car. Excluding the solid state relays, the computer contains two mechanical relays (bottom left) for running lights, a CANBUS module (bottom right), an Arduino (top right), and a 5V regulator on the backside. Currently, this computer turns lights on based on whether or not live data is being sent across the CANBUS since it is always powered and I have not yet figured out what value represents the stte of the vehicle's running lights. This is not ideal since a side effect of my current solution is that running lights turn on when doors open or the car is unlocked etc.