---
layout: archive
title:  "BRZ Steering Wheel"
toc: true
hidden: true
---

### Steering Wheel Button Integration

Steering wheel buttons were added to BRZs in 2017 and mine is a 2013 so I purchased a salvage steering wheel in order to integrate its buttons into my systems. Should anybody else contemplate to do the same, be aware that even though steering wheels are cheap the airbag most certainly is not: a brand new OEM airbag for my car is $CAD 1100 from the dealer while a used one is ~$CAD 600.

The steering wheel uses an Arduino Pro Micro to convert button presses into keyboard output for the Khadas VIM4 to understand. Furthermore, it connects to the CANBUS to forward hardware-related commands to decrease latency as opposed to transferring commands from the VIM4 to other devices. In order to interface the Arduino with the steering wheel, I took apart the button clusters on each side of the 
![Steering wheel button connector](/assets/img/brz/steering_wheel_button_connector.jpg){: style="float: right; width:50%; height:80%; margin-left: 10px;"}
steering wheel and followed traces to figure out the function of each wire. My findings are listed below for anybody else intending to reverse engineer the steering wheel buttons on a BRZ (wires are listed from left to right, top to bottom, looking at the connector from behind as depicted to the right):
- Light green --> Volume and arrow keys of left side cluster
- Red --> Common of left side cluster
- Green --> Arrow keys of right side cluster
- Purple --> Enter and back key of right side cluster
- Yellow --> Common of right side cluster
- Black --> Call buttons, source button, left enter button, and voice button
- Blue --> Steering wheel ground
- Brown --> Cruise control pin 1
- Grey --> Cruise control pin 2
- Black + White --> LED ground
- White --> LED VCC (5V works but dim. Not sure I'm willing to go higher so I'll suck it up)

The 'voice' button on the right side of the steering wheel is wired to the left button cluster and its state is transmitted through the (light green or black) wire. Button states are transmitted by varying the resistance between the common pin of each button cluster and one of the two output pins of each button cluster. The Arduino interprets these using an analog pin using the INPUT_PULLUP pin mode with the common pins being connected to ground. The resulting resistance and output wire for any button press is listed below in hopes of saving someone else the effort required to decode these circuit boards though I ended up using analogRead values in my code so these are useless to me.

| Left Side Cluster | Right Side Cluster |
| ------- | ------- |
| Source --> 115 Ohm BLACK | Up --> 330 Ohm GREEN |
| Call pickup --> 425 Ohm BLACK | Right --> 3.1 kOhm GREEN |
| Call hangup --> 225 Ohm BLACK | Down --> 1 kOhm GREEN |
| Volume up --> short LIGHT GREEN | Left --> short GREEN |
| Volume down --> 50 Ohm LIGHT GREEN | Enter --> 100 kOhm PURPLE |
| Right --> 115 Ohm LIGHT GREEN | Back --> 101 kOhm PURPLE |
| Left --> 245 Ohm LIGHT GREEN | Voice --> 50 Ohm BLACK |
| Enter --> short BLACK | |