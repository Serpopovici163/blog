---
layout: archive
title:  "LoRa Sensor Suite"
---

## A scalable dynamic solution for monitoring activity in unknown environments

At some point in 2021, a friend and I had just finished exploring a construction site and as we were preparing to leave on the first floor, a security guard haphazardly walked in. kLuckily we were out of sight and managed to get away undetected but this presented a need for a system capable of monitoring activity within an uncontrolled environment. So far I have only build PIR sensors for this purpose though I intend to build tripwires and possibly audio sensors. 

# Basic Idea

Each sensor runs off of an 18650 Li-Ion cell with a battery protection circuit that enables USB charging and maintains the cell voltage within its acceptable range. Each sensor has an Arduino Nano with a LoRa SX1278 transceiver in addition to its sensing equipment. LoRa modules are ideal since they act as a mesh network and these sensors are intended for use within concrete buildings thus signals will have difficulty travelling far. An additional LoRa module with a high gain antenna connects the LoRa mesh network to the attacker's phone or some other notification method. Due to the RF challenges faced by these sensors, they should transmit repeteadly once triggered until the attacker's device is able to acknowledge the signal. Based on experimentation, the LoRa modules I own do not natively do this.

# UPDATE Feb 2022

![Front](/assets/img/lora-sensor-suite/PIR-V2-front.PNG){: style="float: right; width:50%; height:50%"}
I've decided to migrate from an Arduino to an ESP32 module; this CAD design has space for an ESP32-WROVER module. The reasoning behind this change is that an ESP32 module can be used to pick up bluetooth devices nearby which would allow the attacker to configure the device with their cell phone 
![Back](/assets/img/lora-sensor-suite/PIR-V2-back.PNG){: style="float: right; width:50%; height:50%"} 
and can be used to pick up unknown devices and send alerts even if the sensor itself fails to trip.  

The current design does not have space for a battery management circuit so I will need to reiterate but it does successfully house the LoRa SX1278, 18650, and PIR sensor. The general shape of the sensor allows for it to be placed on the floor with the PIR facing 45 degrees upwards or it can be flipped and placed on top of a cabinet, for example, with the PIR facing 45 degrees downwards. The next iteration will likely also include magnets such that the sensor can attach to metallic cabinets, metal hand rails on stairs, light fixtures, etc. I also took apart one of the 433 antennas and noticed that at least 10mm of the rubber housing is left empty so I will take this in account and ommit the rubber housing in future designs.



# UPDATE Oct 2021

## Sensor Construction

![Front](/assets/img/lora-sensor-suite/PIR-V1-front.png){: style="float: right; width:50%; height:50%"}
Pins 0 and 1 of the Arduino are used for serial communication with the LoRa module (the RX pin of the LoRa module must be disconnected when code is uploaded to Arduino). Pin A0 is for the pair button and other pins are used as required for the relevant sensor. 
![Back](/assets/img/lora-sensor-suite/PIR-V1-back.png){: style="float: right; width:50%; height:50%"} 
Pins M0 and M1 on the module are shorted to ground in order for the module to transmit. The whole assembly is zip tied together in a rather minimalistic form factor but a bit of paint is needed to cover the green and blue circuit boards. 

## Communication Protocol

Each sensor broadcasts an 'alive' packet once the pair button has been pressed; this packet contains a unique ID based on the microcontroller's serial number to differentiate its 'alive' packet from that of other sensors. Alive packets are repeated up until the sensor receives an acknowledgment packet from the head unit in which the head unit will assign a numerical value to the sensor between 0 and 9998. The user is then able to assign a more descriptive identifying string to the sensor so that its location can be more apparent. Upon being triggered, the sensor will begin boardcasting a packed in the form of "SENSOR_ID:PACKET_ID:TRIG_VAL" where 
- PACKET_ID is a unique integer identifying the packet to ensure the head unit doesn't register the same alert twice
- TRIG_VAL is an integer representative of the sensor's states

This packet is repeated every 0.5 seconds up until the sensor receives a packet from the head unit in the form of "9999;SENSOR_ID;PACKET_ID" where
- 9999 is the equivalent SENSOR_ID for the head unit
- SENSOR_ID is the id of whichever sensor tripped
- PACKET_ID is the packet id