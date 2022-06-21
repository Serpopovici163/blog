---
layout: archive
title:  "IP Camera Hacking Toolkit"
classes: wide
---

## Exploit collection/automation for networked cameras

This is a project I haphazardly began upon discovering **FIND CVE #** which allows an attacker to bypass authentication on Hikvision cameras using a crafted URL. The exploit is rather simplistic but made me consider how useful camera exploits are when infiltrating facilities with unknown camera locations. Aside from connectivity issues I've faced with IPv6 networks, it should be theoretically possible to tap into any ethernet cable within a camera network and gain access to every single camera. I have not yet built a device capable of this but I intend to do so in the near future, here is the theoretical approach I have considered.

Axis, Avigilon, and Hikvision cameras are most common in construction site and commercial security so these are a priority.

# Theoretical device

The device would have two sets of 8 punch down connectors for each wire within an ethernet cable. The attacker would have to strip roughly 15-20 cm of a target cable and separate the wires enough to punch each wire down in both sets of connectors. After all the wires have been connected, the attacker would cut them between the punchdown connectors at which point the device will activate and act as a network switch and instantly reconnect the affected camera while providing the attacker with a physical connection into the network. The device would leech POE power off of the ethernet cable though having a standalone power supply may be required because the POE device powering the camera could technically trigger an alarm and notify someone of a discrepancy in POE power draw. 

# UPDATE June 2022

Just purchased three items to hopefully build a device that can parasitically leech off of a POE camera wire. I bought a POE splitter which accepts one ~30W input and splits it into two ~15W POE outputs; I'm not entirely sure about these specifications but it should be capable of powering a single camera even with the device patched in to the existing POE line. I also purchased a board which converts POE power into a 12V 1A output and managed to find some punchdown connectors that should work with 22-23AWG wire. Fortunately, the punchdown connectors I found are single 'T-taps' as pictured to the right. These T-taps will make it significantly easier to patch in to an existing installation without needing to strip a large length of wire. I was initially worried about how difficult it may be to align and punch down 8 wires given past experience when installing [ESP-RFID-Tools](https://github.com/rfidtool/ESP-RFID-Tool) which only require me to punch down 4 wires. I am eager to proceed with this project once I receive the parts, at the very least I will develop a tool capable of jamming camera networks using a [IPv6 Router Advertisment Flood Attack](https://www.researchgate.net/publication/266022049_ICMPv6_Router_Advertisement_Flooding). I have had issues in the past getting my computer to communicate with devices on NVR networks since these are designed more for communication between the NVR and the camera as opposed to communication between the cameras themselves. For some reason, my computer fails to obtain a valid IP configuration since it does not seem to gather that it is connected to an IPv6 network as opposed to an IPv4 network. I will be using a smaller SBC such as the [Orange Pi Zero](https://www.aliexpress.com/item/1005002918902225.html) when building this concept which will ideally perform better.

# Current issues

IPv6 is the most significant issue I'm facing at the moment. Both my laptop and cell phone refuse to obtain a valid IP address when on a wired IPv6 network even though they should both be fully compatible. This may be due to MAC address filtering but I'm not sure.

# Hikvision

Working exploit for individual cameras but no exploit against NVRs. Camera enumaration/fingerprinting possible.

# UNV

Found a NVR password disclosure exploit but not tested. My building uses UNV cameras on an IPv4 network so I am able to experiment with things but I haven't done much work thus far.

# Axis

I know remote code execution is possible but have yet to experment with any Axis cameras.

# Avigilon

Probably the most important target to me given their deep learning AI that makes them more detrimental than regular cameras.

# Sony IPELA

Idk if I'll bother with Sony, they are mostly used in retail so I have little interest in them.