---
layout: single
title:  "Roof Anchor Drone"
classes: wide
---

## A physical infiltration tool

This device was originally designed in hopes of rooftopping the second tallest building in Canada at the time of this writing: The St. Regis hotel in Toronto. The concern was that the lower roof at the St. Regis hotel is accessible while the higher roof is roughly 3 storeys taller and unnaccessible through conventional ways. In the end, there was an easier way of reach the top and the drone was not used however it remains a genuinely useful tool for ascending moderately tall structures with no internal access.

# General premise

The goal is to build a 3D-printable assembly that attaches to the bottom of a drone and holds a carbiner in an open position which can then be remotely shut. The drone must be small enough to be easily portable and nimble, while the carabiner assembly must be able to support the weight of the rope being carried along by the carabiner. In cases where the distance to be ascended is too large for the drone to reasonably carry the required length of rope, a thinner and lighter 'tag line' can be passed through the carabiner and used to pull the climbing rope up once the carabiner is hooked onto an anchor. Finally, a camera must be integrated on the drone to provide the operator visual feedback to line up the carabiner with the target roof anchor.
 
# BUILD

The initial design for this project was a singular 3D printed piece that mounts to a DJI F450 frame since I had a few of these lying around. While this design is functional, the large propeller diameter negatively impacts the drone's agility and the drone's size make it a poor solution in terms of portability. [PICS] For these reasons, a new design was created using a 215mm race drone frame. The new design (depicted) is much more portable and breaks down into two pieces such that it can be more reasonably stored in a small volume. 
![CAD view](/assets/img/roof-anchor-drone/CAD_assembly_view.PNG)
Here the green component houses the flight controller, ESCs, battery, and radio gear whereas the purple component is the carabiner holder.

## Carabiner mechanism

In addition to its modularity, the final design has two servos on the carabiner holder component: the first (1) retains a rubber band which is wrapped around the holder to keep the latch open while the second (2) supports the weight of the rope being carried by the carabiner. 