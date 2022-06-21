---
layout: archive
title:  "DIY 3D Printed Powered Ascender"
classes: wide
---
## Shouldn't work but it might

I figured that, considering people have 3D printed functional guns, surely it must be possible to 3D print an ascender without worrying about the material's strength. Initial designs are solely for prototyping and, as such, have no accomodations for the ESCs, batteries, or any hardware besides pulleys and motors. 

# UPDATE Mar 2022

**TODO: make new CAD design**

# UPDATE Feb 2022

Previous design did not work well, there was a substantial lack of grip on the rope which led to excessive friction that melted part of the rope. Furthermore, the 'teeth' on the drive pulley wore out in no time so I will be abandoning these in the future; I knew they wouldn't last long but I assumed they would simply round off as opposed to completely melting off. 

One solution to the lack of grip is increasing the number of drive pulleys in the design, but in the interest of keeping things simple and compact the new design simply has the rope loop around the drive pulley multiple times. The new issue to consider here is is whether or not the drive pulley will get crushed now because, by looping the rope around multiple times, the drive pulley is now effectively supporting the full weight of the wearer. 

# UPDATE Jan 2022 

I only have 5015 320kV motors available to me which are meant to be used for the Jaguar UAV; given their high kV and low torque rating I will be using two of these in tandem. I am confident in the motor mount and side panel of this prototype, however I fully expect the main pulley to fail but only time will tell.

I started with the main gear that is responsible for feeding the rope through the ascender. Initially, the gear was 80 mm in diameter to provide more surface area thereby having greater grip on the rope; this was later reduced to 40mm because the motors I own have a relatively high kV so they need to work at a higher RPM to produce their rated torque. Below is a preliminary model of the main pulley. It has a hole in the center to accomodate the motor's shaft and four holes that are meant to be occupied by the heads of 4 screws mounted to each motor. I was unsure of how else to link the motors and pulley without overcomplicating the design.
![Main pulley design](/assets/img/ascender/V1-main-pulley.PNG)
The image below depicts the assembly without a sidepanel to display the internals. The blue solid demonstrates where the rope is meant to go and the red arrow point upwards.
![Assembly w/o side panel](/assets/img/ascender/V1-assy-without-sidepanel.PNG)
Below is the complete assembly.
![Complete assembly](/assets/img/ascender/V1-assy.PNG)