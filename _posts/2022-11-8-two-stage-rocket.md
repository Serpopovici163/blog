---
layout: single
title:  "Two G-class Stage Rocket w/ Basic Telemetry"
classes: wide
---

## Experimenting with rocketry electronics

While I may never get a high power rocketry license, I do thoroughly enjoy developing and flying model rockets. None of my past rockets have incorporated any amount of electronics so the purpose of this project is to get a feel for rocketry electronics without any risk by developing only non-terribly-flight-critical components. For this project, I purchased the two biggest rocket engines I can legally get my hands on without any license: AeroTech G80-7T motors.

# UPDATE June 2023

In order to test the avionics before a flight in which their functionality is critical, I am going to test fly the main stage off of a G80-10T motor which shouldn't require any special considerations for when the main chute can deploy. I've done simulations in OpenRocket (depicted below) which concluded that the 10 second ejection delay should be plenty to allow the rocket to reach apogee. 
![Single stage flight simulation](/assets/img/habibi-express/single_stage_simulation.png)
The simulation above shows that the motor burnout occurs roughly around 1.5 seconds which means that our ejection charge would deploy at 11.5 seconds and therefore slightly past apogee.

In this test, the avionics system will log its decisions to the SD card onboard the rocket alongside a time-stamp. Ideally, I can start recording video with my phone, send a message to the rocket over telemetry on camera which will reset the rocket's clock to 0 at that point in time, and then proceed with the launch while maintaining the recording. In this manner, I should be able to match events seen in the video to the timestamped events logged by the rocket and hopefully determine whether or not it detects launch appropriately, detects apogee appropriately, and attempts to deploy the main chute at the appropriate altitude.

# UPDATE May 2023

![Assembled airframe](/assets/img/habibi-express/assembled_airframe.jpg){: style="float: right; width:30%; height:50%; margin-left: 10px;"}

It's been a minute. I've been really busy with other commitments but the airframe is finally coming together! Unfortunately, the GPS I got does not seem to work very well. This is the second BN-880 module I've gotten, I assumed the first one was a fluke but I guess these are just bad GPS sensors in general. As such, I will need to reprint the nose cone to accomodate a uBlox-M8N chip I have and I will also attempt to install some cameras in the nose cone for the DVRs that I've added to the avionics bay. The avionics bay is also rather messy however everything works well from a software perspective. The one issue it has is that the voltage on the 5V rail drops dramatically whenever the radio sends out a telemetry packet. This does not affect the avionics systems but does affect the servo that is in charge of preventing the main chute from deploying straight away. This could be because of a bad LiPo battery that sags a bunch or simply the servo being too cheap so I will first attempt to swap the servo out with a nicer one before going to a nicer battery. I've already given the radio a separate 5V regulator from the other avionics and added a capacitor in parallel on the radio's power input which should theoretically take care of the voltage drop but that has not really fixed anything so far. Given that the other electronics behave themselves, it's almost certainly an issue with the servo.

![Avionics front](/assets/img/habibi-express/avionics_front.jpg){: style="float: left; width:50%; height:80%;"}
![Avionics rear](/assets/img/habibi-express/avionics_rear.jpg){: style="float: right; width:50%; height:80%;"}

Definitely not a fan of how messy the wiring is but I don't see it getting much better with the amount of tech stuffed in there. I've added an APM power module to use its regulator and add current/voltage sensing to the Arduino so I can get this info through telemetry. There are two sets of wires coming out of the top of the avionics assembly: the first (with the white connector) go to the GPS in the nose cone and the second set (4 free wires) are for power and two video feeds coming from cameras in the nose cone. The four wires at the bottom are for the second stage ignitor and the ejection charge ignitor.

# UPDATE Nov 2022

Project is not on track, too many things going on in school however the avionics bay has been printed and mostly assembled. The nose cone is being redesigned to house status LEDs, the GPS, and two cameras for in-flight footage. Pictures coming soon.

# UPDATE Oct 2022

![Recovery layout update](/assets/img/habibi-express/recovery_layout_update.jpg){: style="float: right; width:10%; height:20%; margin-left: 10px;"} 
Progress is being made on flight controller firmware, the I2C issue has been fixed, and the IMU data is now being read as well. Turns out the Adafruit BMP280 library was looking for the wrong address and I had to force it to read data from the true address. Engine mounts and fins have been attached to the rocket, currently focused on recovery charge and how to protect the rocket's internals from the motor's ejection charge as well as the true ejection charge. The motor mounts have plenty of space around them to allow the motor's ejection charge gases to escape however I need to add a component following the motor to shield the parachute from the motor's charge and contain the true ejection charge as well. The planned layout is pictured to the right where the green block represents the blast shield, red represents the true ejection charge, yellow represents the drogue chute, and cyan represents the final chute. The rocket splits between the cyan and yellow blocks when the ejection charge detonates.

The avionics bay has been expanded to include MOSFETs for the second stage and ejection charge igniters as well as an SD card reader for data logging, servo to retain the primary parachute, and two DVRs (scrapped the camera inside the body tube). The second stage will be ignited based on the following criteria:
- Rocket attitude is within 30 degrees of launch attitude (makes sure the rocket is vertical)
- Rocket altitude is greater than 250 meters (should be 500 meters or so based on simulation)
- Rocket acceleration drops noticeably (simulation suggests peak of 13 Gs however the software will just look for a drop of ~5Gs)
- Launch was detected less than 10 seconds ago (makes sure the conditions cannot be met unless the rocket just took off)

Once launch is detected (as a peak in acceleration), the rocket will begin a 3-second timer before checking the conditions listed above and igniting the second stage. Apogee will be detected when vertical acceleration drops below -5m/s^2 after which the ejection charge will be ignited. The final chute will be deployed 250 meters above ground level based on barometric data. Launch window has been slightly pushed back however it should be achievable by end of November :)

# UPDATE Sep 2022

![Laser cutting fins](/assets/img/habibi-express/fin_laser_cutting.jpg){: style="float: right; width:30%; height:50%; margin-left: 10px;"}
Finally got around to laser cutting the fins and I have worked slightly on the firmware for the flight controller. Unfortunately I'm having issues with the barometer since it communicates using 3.3V so the Arduino Mega is not able to interpret the I2C data coming from it. The current solution is to replace the barometer with a proven chip that I can find online as working with the Arduino Mega. I will also attempt to read the I2C data using another 5V Arduino to verify that this is indeed the issue. Finally, I have decided to add a few [video recorders](https://www.aliexpress.com/item/1005002457700952.html) and a few analog cameras to capture a few angles of the flight. I'm hoping to add a downward facing camera, a side facing camera, and one looking down the body tube towards the main stage engine to capture the ejection from inside the rocket. I intend model camera mounts and redo the avionics bay by the end of October such that the rocket can be flown early November at the latest.

# UPDATE Jun 2022

![Assembled avionics bay](/assets/img/habibi-express/avionicsFront.jpg){: style="float: right; width:30%; height:50%; margin-left: 10px;"}
The avionics bay has been assembled though it is missing a couple key elements. Firstly, there is no voltage regulator to step down the LiPo's voltage for the Arduino and related electronics; furthermore, I need to integrate a relay, so the Arduino can ignite the second stage motor once the initial one burns out. Finally, the antenna installed on the design right now can not be used and is solely there to ensure that I don't accidentally burn the LoRa transceiver by powering it on without an antenna. Everything on the module at the moment is completely wired and ready to go. I have noted the I2C addresses of the MPU6050 and BMP280 and the wire with the white connector sticking out the top of the avionics bay goes to the GPS sensor in the nose cone of the aircraft. 

The software for this design won't be too involved as the Arduino only needs to ignite the second stage 3 seconds after the MPU6050 detects a decrease in vertical acceleration all while broadcasting GPS, altitude, and attitude data at regular intervals. The main concern now is developing viable fins for the rocket body. I am hoping to have them laser-cut at the university but I have yet to inquire about using the equipment.

# UPDATE Mar 2022

![Avionics bay V2 back](/assets/img/habibi-express/avionics-CAD-V2-back.PNG){: style="float: right; width:30%; height:50%; margin-left: 10px;"}
The avionics bay is mostly finalized, and I will attempt to print this soon; I've done without the 18650 battery and kept the 3s LiPo which is now housed in a casing at the top of the avionics bay. The LoRa module is now positioned such that an antenna can be directly attached to the module and have space in the rocket fuselage. The only thing lacking from the model to the right is a relay holder to ignite the second stage. I still need to remodel the nose cone since the current GPS holder may not have clearance, so it must be shifted upwards after which I will begin 3D printing these components and assembling the rocket!

# UPDATE Feb 2022

![Rocket model](/assets/img/habibi-express/rocket-CAD.PNG){: style="float: right; width:50%; height:50%; margin-left: 10px;"}
CAD model is now mostly finished, lots of small things still need to be figured out however most of the grunt work is done. I don't have a concrete plan for how to wire the main stage motor's igniter in a way that won't risk tangling the parachute during recovery. Furthermore, I purchased two G80-7T motors however the rocket will need at least 10 seconds after the main stage burns out to reach its apogee meaning that I most likely need to source a G80-13T motor to avoid using any complex recovery mechanisms. Unfortunately, [Great Hobbies](https://www.greathobbies.com/) does not carry G80 motors anymore, so I can only source these for unreasonably high prices from [AllRockets](https://www.allrockets.ca/G80-13). 

![Avionics model](/assets/img/habibi-express/avionics-CAD.PNG){: style="float: right; width:30%; height:50%; margin-left: 10px;"}
Main focus now is the avionics bay. The model pictured here has space for an Arduino Mega 2560 (embed), a BMP280, an MPU6050, a LoRa SX1278 transceiver, and two batteries. The nose cone contains a GPS antenna bracket for positioning as well. This is the second iteration of the avionics bay, and it contains space for both an 18650 Li-Ion cell (in green) and a small 3S 700mAh LiPo next to the Li-Ion cell. I'm concerned that the ~3V from the 18650 will not be enough to ignite the main stage of the rocket even with a boost converter which is why I included the LiPo. I originally considered using the 18650 in light of its higher energy density however it only has roughly 1.5 times the energy stored by the LiPo so it will likely be discarded in the next iteration.

# UPDATE Dec 2021
Rocket design was finalized in [OpenRocket](https://openrocket.info/) and basic simulation was done; maximum altitude estimated at roughly 1.1km with a max speed of Mach 0.52. I made some faster designs and others with higher apogees, but this model seems the most predictable based on how much tech I want to fly with the rocket and how lightly I can manufacture things.

![OpenRocket Model](/assets/img/habibi-express/open-rocket-sim.PNG)
![OpenRocket Simulation](/assets/img/habibi-express/open-rocket-sim-graph.PNG)

# Design
## Motor mounts

![Recovery layout update](/assets/img/habibi-express/motor-mount-and-coupler.png){: style="float: right; width:35%; margin-left: 10px;"} 
I watched a [video](https://www.youtube.com/watch?v=4fhoCt9vXA8) by ProjectAir on YouTube about a rocket he built using similarly powerful motors and I based my rocket motor mounts on his design. The motor mounts consist of a small tube for the motor and a larger tube that fits snugly into the rocket body. These two are connected by 8 perpendicular supports between the two cylindrical extrusions. The holes on the bottom of this motor mount also work to hold the first stage onto the main stage by using 8 pegs protruding from the first stage which fit snugly into the 8 holes of the motor mount above.

A screenshot of the motor mount CAD (black) and the first stage coupler (gray) is included to the right. One of the pegs on the coupler is hollow, allowing ignition wires to reach the bottom of the second stage motor.

## Avionics

I intend to have live data logging and telemetry at the very least. This rocket will include a GPS, barometer, and accelerometer as well as an ignition system for the main stage engine. I would also like to include some system that can delay the deployment of the main chute since it will significantly increase the rocket's drift during recovery however I'm unsure of how reliably I can integrate such a feature. All the electronics will be housed near/in the nose cone and the rocket will separate close to the main stage motor for recovery such that the body tube/nose cone section containing the electronics remains intact.