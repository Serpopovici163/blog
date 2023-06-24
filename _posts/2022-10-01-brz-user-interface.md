---
layout: single
title:  "BRZ User Interface"
toc: true
hidden: false
---

### Head Unit Display

The large head unit display is split horizontally into two sections: a 'big container' which is 3/4 of the display's width and a 'small container' which occupies the remaining space. The views displayed by the big container and their respective purpose are as follows:
- Navigation fragment --> Contains an interactive map that the user
- Lighting control fragment --> Handles interior accent colours and vehicle under-lighting (legal light functions)
- Safety fragment --> Displays live view of cameras around the car as well as any computer vision-related safety warnings
- Settings fragment --> Allows user to modify settings of any subsystem 
- Defense fragment --> Compliments defense fragment in small container, displays advanced countermeasures

The views displayed by the small container and their respective purpose are as follows:
- Media fragment --> Handles media playback controls
- Diagnostic fragment --> Mirror of center console display, describes states of vehicle systems
- Soundboard fragment --> Enables playback of custom sounds through a loudspeaker in front of vehicle
- Pop-up defense fragment --> Brief rundown of countermeasure states, this fragment is automatically overlaid when vehicle detects radar/lidar etc.
- Traffic advisor fragment --> Allows user to control illegal light patterns of vehicle including police and hazard strobes

In addition to the two primary fragment containers, a few fragments can be overlaid on top of the whole display in specific circumstances:
- Parking fragment --> Overlaid when vehicle is put into reverse by driver and persists until vehicle speeds up regardless of which gear the vehicle is in. This fragment provides data from parking sensors and cameras around vehicle to facilitate parking.
- Merging fragment --> Pops up when merging right, provides live view of traffic in right lane so driver can see all vehicles in adjacent lane and blind spot.

Here are screenshots of what the head unit display looks like. I wrote the sofware for his in Android, however the weird display architecture of the head unit combined with the fact that I'm trying to run Android on a Raspberry Pi makes it unusable. Instead of fighting with the kernel to get something to work, I've decided to rewrite the page in NodeJS with electron and keep Raspbian running on the Pi. Ideally, this will facilitate fine tuning the behaviour of the Pi as Linux is much more flexible and also allows the Pi to easily interface with the CAN bus or whatever other I/O I may add in the future.

![Media Display Sample Layout](/assets/img/brz/media_display_sample_layout.png)
![Media Display Sample Layout](/assets/img/brz/media_display_sample_layout_2.png)

### Diagnostic Display

This display layout is very much a work in progress because I'm unsure of what functionality I will assign to the display and so I'm unsure as to how to lay things out. I do intend to add touch screen buttons to replace the four physical buttons that used to be below this display (2x seat warmers, sport mode, and traction control).

![Center Console Display Sample Layout](/assets/img/brz/center_console_layout_sample.png)

### Gauge Cluster Display

I have not begun work on this. I bought a second cluster to modify it into a digital one but I'm focused on other things.

Listens to car's CANBUS and displays the same information as was available with original gauge cluster though it also adds data for range and displays alerts from vehicle systems. (TRUE?) Furthermore, can be used to display night vision camera in front of vehicle to drive in less than optimal lighting conditions.

## Interactive User Interface

The diagnostic displays is the only one with touchscreen functionality and the head unit display can only be controlled using steering wheel buttons for a handsfree experience. The right side steering wheel buttons are used to control the contents of the big container while the left side steering wheel buttons are used for the small container's contents. Additionally, there are dedicated buttons or combinations thereof that can always be used to toggle critical functions such as the vehicle's legal mode regardless of what content is live at the time.