---
layout: archive
title:  "Smart Holographic Sight"
classes: wide
---

## Intelligent weapon optics

My overarching goal with a lot of the things I build is to develop a small ecosystem of tactically useful devices that can exchange information and provide the user with an advantage over their adversary. These devices include anything from motorized ascenders to augmented reality devices and UAVs. The SmartSight may work in conjunction with a motorized ascender such that the user can maintain the optic at eye level while also being able to control their ascender. This would provide the ability to keep a weapon tracked on a target while moving up or down the side of a building, for example. The SmartSight idea came about as I saw an old project by a [friend](https://andymeng.dev/) of mine and decided to elaborate on it.

# Basic Premise

The SmartSight contains a small display which reflects through a mirrored lens that allows the user to see both the display and the real world, just like a traditional holographic sight. I'm not too sure how much information I will present on the display; so far I intend to display a shot counter and a reticle which can adjust based on weapon cant. Furthermore, I will add indicators that describe the motorized ascender's state and possibly a rangefinder at the very least. I only intend to use this on airsoft rifles so the rangefinder doesn't need to be excessively good.

# UPDATE Mar 2022

The last version used a 0.96" 80x160 [ST7735S display](https://www.aliexpress.com/item/1005003514645335.html) which has great pixel density but does not manage to cover the whole lens that it's being reflected through. To rectify the issue, I recently purchased a 1.3" 240x240 [ST7789 display](https://www.aliexpress.com/item/4001282467099.html) that is large enough to mostly cover the exposed area of the lens. 

**TODO** Generate new model with bigger display and whatnot.

# UPDATE Feb 2022

![Front](/assets/img/smart-sight/front.PNG){: style="float: right; width:50%; height:50%; margin-left: 10px;"}
Got around to creating a CAD model of the sight; this model is technically the third iteration but it represents the first reasonably functional version. It has space for an Arduino Nano, LoRa SX1278 transceiver, display, and a LiPo though no antenna mounting hole has been added. Having created this model with a transceiver onboard, I considered the possibility of including a remote kill feature for disabling weapons and as such I will probably need to wire the sight into the gun's electronics. This means that future designs don't strictly need to include space for a LiPo but I'm unsure of whether or not I will remove it just yet.

![Back](/assets/img/smart-sight/back.PNG){: style="float: left; width:50%; height:50%; margin-right: 10px;"}
Unfortunately, this design was built to fit some standardized Picatinny rail dimensions I found [online](https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Picatinny.svg/1200px-Picatinny.svg.png) and these did fit one of the rifles I own but not the functional one. I was picky in finding my dimensions since the design was built all in metric units which may have caused some inaccuracies so the next design will be built using real measurements from the functional rifle. The intent with this design was to stick the antenna out the side of the sight but this is a poor decision considering the wear and tear that the weapon will likely be subjected to so I try to will integrate the antenna within the sight for the next iteration.  

Here is an exploded view of this model:
![Exploded View](/assets/img/smart-sight/exploded_noWriting.PNG){: style="display: block; margin-left: auto; margin-right: auto; margin-top: 10px"}
