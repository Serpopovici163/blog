---
layout: archive
title:  "LTE Mavic Pro"
classes: wide
---

## Limitless range and enhanced autonomy

LTE based drone control has been a longstanding goal with the purpose of creating UAVs requiring minimal user input to function. I have found the need for constant input to significantly reduce the useability of drones for recon during my adventures because neither myself nor the people with me can dedicate significant attention to piloting them. This project aims to achieve basic autonomous control through an Android application that allows the user to place the drone at specific coordinates or land it. The first rendition was built to be submitted as an electronics project for a university course and is solely capable of navigation; takeoff and landing must be manually with a separate remote controller. This is because a F1 flight controller was used which is only capable of managing stability. 

# Basic Idea

The drone will be centered around a Raspberry Pi Zero and a LTE modem for internet access. A Raspberry Pi CM4 could fit and would be beneficial but I am not currently capable of developing carrier boards and a full size Raspberry Pi will not fit. The Pi's serial port is dedicated to communicating with the flight controller through iBus since iBus operates at 115200 baud and bit bashing on the Pi becomes unreliable past 19200 baud. The GPS data will be received using a GPIO pin and bit bashing and another bit bashing serial connection will be used to communicate with an Arduino. The Arduino serves as an IO expander for the Pi to manage the LEDs on the drone as well as an ultrasonic sensor and sense voltage and current throughout the drone. Using an Arduino as such is advantageous since it has an ADC whereas the Pi does not. Additionally, offloading time-sensitive operations such as reading the ultrasonic sensor and running the drone's LED patterns decreases the complexity of the Pi's code.

## Data Link

My current LTE modem filters all ports no matter what settings I chose within its menu. For this reason, I've elected to use a P2P UDP protocol which is able to punch through firewalls. A second Raspberry Pi will serve as an intermediary P2P server to connect the cell phone and drone given that neither has a static IP. The P2P code was largely copied from https://github.com/grakshith/p2p-chat-python.git and modified to better fit my protocol. Both the drone and cell phone should identify themselves during their initial UDP broadcast in order to identify what function they serve; this future proofs the server's code since I can selectively connect devices based on functionality as opposed to the time at which they contact the server. Once two compatible hosts have broadcast their address/port to the P2P server, they will each receive the address/port of the other client and the server logs the connection before discarding all saved host data. 

## Flight Algorithm

The version submitted for my course uses a rather primal flight algorithm in which the Pi has a barometer and compass connected to it through I2C. Using GPS, compass, and altitude data the Pi is able to compute a vector from its current coordinates to the target coordinates using the haversine libary for Python and a bearing function I stole from StackExchange. It then points the drone in the target direction and goes forward; the drone's speed varies based on distance to target and it progressively slows down as the distance decreases. The current version offloaded all navigation to the flight controller running iNav thereby reserving the Pi's looptime for more useful functions such as WiFi, cellular, and UHF sniffing. OpenCV can also be used on the Pi to provide some level of active tracking and increase the effective autonomy of this platform. For this, I may need to choose a UAV platform with surround cameras such as the Mavic 2 or Skydio 2 and upgrade to an nVidia Jetson computer since having the drone actively follow subjects would require some level of obstacle avoidance.

# UPDATE Feb 2022

Obtained a SucceX-D Mini F7 Twin G stack and redesigned the 3D printed bracket which holds all the electronics inside the Mavic. 
![Bracket V2](/assets/img/hacked-mavic/board-mount-V3.PNG){: style="float: right"}
The new design no longer requires tape and glue but rather relies on three pre-existing screwholes used by the original DJI flight control board. It took a few revisions to finalize the fit of the board such that the components comfortably fit within the restricted space originally occupied by DJI's proprietary electronics. This is the fifth revision of the bracket and it includes mount points for a 4-in-1 ESC board, flight controller, the Raspberry Pi, and a Flysky receiver. I'm sticking with the Flysky receiver over a Crossfire system since it is able to take advantage of the 2.4 GHz antennas already integrated in the Mavic leading to a more 'stock' look. The 4-in-1 ESC board is on the bottom such that it can be in contact with the bottom heatsink of the Mavic and hopefully take advantage of it. Realistically, this ESC board is rated for 45A per motor which is likely over double what the motors should be drawing. Finally, I have decided to remove the Arduino since I should be able to accomplish its tasks with threaded processes on the RPi. In doing so, I am now once again able to install the OEM gimbal on the drone and, since the DJI Mavic's camera seems to use MIPI, I should be able to interface it with the RPi (I haven't yet tested the camera's protocol but its connector looks identical to that of the Caddx Polar camera I own).

Unfortunately I still do not have a compact cellular modem and I am not sure where I will be able to fit it. I also need to create a custom target to flash the F7 board with iNav since I don't believe Betaflight is capable of executing waypoint missions through telemetry but I will see if that's a possibility. I also need to figure out if the flight controller is able to arm and takeoff without any contribution from the receiver since I only intend to keep it for debugging/testing purposes and it is not intended to be used during regular operation.


# UPDATE Dec 2021

Initial bracket designs relied on double sided adhesive and a small bracket to effectively hold down all the internals. 
![Bracket V1](/assets/img/hacked-mavic/board-mount-V1.PNG){: style="float: right"}
This design makes maintenance virtually impossible without considerably disassembling the drone so current versions use existing screwholes to ensure a more secure fit. The varying boards within the Mavic are installed as follows: 
- The flight controller is installed sideways on the front of the bracket immediately following the pre-existing fan 
- A 20x20mm 20A ESC board is installed immediately following the flight controller on the underside of the bracket. This allows for thermal pads to be placed between the ESCs and the Mavic's pre-existing heatsink thereby providing some cooling.
- Following the ESC, the LTE modem and an Arduino Nano are installed on the bottom of the board.
- The top of the bracket is exclusively reserved for the Raspberry Pi Zero. Ideally, the Pi would be on the bottom so that the heatsink could cool it but unfortunately the ultrasonic sensors make this difficult so I had to place it on top. Fortunately, the Pi Zero does not need any cooling so I will keep it like this for future designs.
- Finally, a FlySky i6X receiver is shoved within the wiring towards the end of the drone body for debugging purposes.

![Mavic Build V1](/assets/img/hacked-mavic/build-V1.jpg)

# Code

## Raspberry Pi

The Raspberry Pi runs off of a Python program split into four key files: 
- **main.py** --> this file is what runs upon startup. It calls upon the other three and synchronizes data between files (takes updated target coordinates from network.py and sends them to flight.py)
- **network.py** --> this file handles all P2P communication and is responsible for maintaining a link with the cell phone. It receives target coordinates and saves them in a local variable which is then queried by main.py
- **flight.py** --> handled flight algorithm in implementation for my course however **TODO** current version dispatches target coords to the flight controller and monitors telemetry data so it can be passed on to the Android application
- **misc.py** --> contains random functions used by all files such as logging

## Arduino

Pins A0 and A1 are used for detecting voltage from the battery and the 5V rail. Pins 2 and 13 are used as digital outputs for controlling the LEDs on the left and right front arms of the Mavic, respectively. Pin 8 is used for the LEDs throughout the drone which are single WS2812b pixels while pins 5 and 6 are used to monitoring the ultrasonic sensor. The LED patterns were done by keeping track of loop time: every time an LED pattern begins the value of millis() is saved in cycleStartTime, and for every time the code runs the current output is compared to cycleStartTime. Each cycle has an arbitrary time step, for example a cycle may have 8 time steps all of which last 800ms. These time steps are encoded in if statements that compare millis() to cycleStartTime and figure out which time step the cycle should be running. Once all 8 time steps have finished, cycleStartTime is updated to represent the current time and the cycle restarts.

## Android Studio

Not sure where exactly to go with the application. Probably gonna open to a full screen map but also have another activity for the drone camera feed/more complex controls. The user will be able to have either activity open full screen or split the screen so both can be viewed at the same time. 