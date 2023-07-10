---
layout: single
classes: wide
title:  "BRZ LightLink"
toc: true
hidden: true
---

## Light Modifications

The car uses [my custom LightLink](/brz-custom-hardware) module will be installed under the steering column, next to the car's body control module which provides it with immediate access to all of the car's stock lighting wiring. I may have to run some extra wires for things like my brake and running lights since the running lights, for example, probably have a single wire connecting them to the body control module but I want the ability to toggle both independently. The only lights that will remain controlled by the body control module are the third brake light and the headlight highbeams; everything else will be handled by the LightLink module.

### License Plate LEDs

![License Plate LED CAD](/assets/img/brz/license_plate_LED_CAD.png){: style="float: right; width:40%; height:80%;"}
There wasn't much of a reason to redo these, the light bulbs were perfectly adequate but I figured I might as well add more RGB. These mounts are currently flawed since the LEDs got incredibly hot and managed to melt the ABS so they are currently only partially supported by what's left of the plastic bracket. The current CAD is pictured to the right. I'm going to fix this by mounting the LEDs onto some sort of metal piece, likely some sheet metal cut to size, such that they can better dissipate the heat the generate. From there, the ABS mount will solely come in contact with the sheet metal and not the LED. I'm hoping this will provide enough insulation to prevent the ABS from melting again.

### Side Markers

The following may be useful to other people hoping to get custom side markers for their BRZ. I managed to lose one of mine while driving and it got run over before I had the chance to go retrieve it off the side of the road.
![Side marker CAD](/assets/img/brz/side_marker_CAD.png){: style="float: right; width:30%; height:80%;"}
As it turns out, it is in fact cheaper to buy high power addressable RGB LEDs and 3D-print two custom side markers than it is to obtain a single OEM one so I naturally chose that route. The CAD to the right shows the final iteration of the side marker which houses a 9W RGB LED. No space is provided for the LED's driver board as it will be housed on the inside of the bumper. I'm not sure if this design is functional. I did a similar thing for the license plate LEDs, as explained in the section above, and may redo this design to avoid having the ABS melt. The picture below provides an idea of how the LED would currently fit in the design.

![Side marker LED test fit](/assets/img/brz/side_marker_LED_test_fit.jpg){: style="width:50%; display:block; margin-left: auto; margin-right: auto;"}

### Fourth Brake Light

<iframe style="float: right; width:20%;" src="/assets/img/brz/fourth_brake_light_video.mp4" frameborder="0" allowfullscreen="allowfullscreen">&nbsp;</iframe>

This was a fun one. The stock fourth brake light only contains two bulbs that turn on when the vehicle is in reverse. The plastic housing of the light quite literally has a spot dedicated to having a light bulb behind the red lens but none is included from the factory. I knew I was going to add a fourth brake light to the car and decided that I might as well add RGB to the two reverse light compartments as well. I don't seem to have any pictures of the modified fourth brake light though I do have this video where I had programmed it to flash red and blue. The assembly process was rather simple, I cut some holes in the fourth brake light casing with my soldering iron that were just large enough for the portion of the LED that lights up. I then screwed the LEDs into the plastic of the fourth brake light using the two mounting holes they have and lathered the completed thing with high temp hot glue. I've had this running for quite a while now and it has yet to fail. Aesthetically, the fourth brake light truly completes the rear end of the car.

![BRZ rear](/assets/img/brz/fourth_brake_light_rear_end.jpg){: style="width:50%; display:block; margin-left: auto; margin-right: auto;"}