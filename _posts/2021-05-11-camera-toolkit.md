---
layout: archive
title:  "IP Camera Hacking Toolkit"
classes: wide
---

## Exploit collection/automation for networked cameras

This is a project I haphazardly began upon discovering **FIND CVE #** which allows an attacker to bypass authentication on Hikvision cameras using a crafted URL. The exploit is rather simplistic but made me consider how useful camera exploits are when infiltrating facilities with unknown camera locations. Aside from connectivity issues I've faced with IPv6 networks, it should be theoretically possible to tap into any ethernet cable within a camera network and gain access to every single camera. I have not yet built a device capable of this but I intend to do so in the near future, here is the theoretical approach I have considered.

Axis, Avigilon, and Hikvision cameras are most common in construction site and commercial security so these are a priority.

# Theoretical device

The device would have two sets of 8 punch down connectors for each wire within an ethernet cable. The attacker would have to strip roughly 15-20 cm of a target cable and separate the wires enough to punch each wire down in both sets of connectors. After all the wires have been connected, the attacker must cut them between the punchdown connectors at which point the device will activate and act as a network switch which will almost instantly reconnect the affected camera and provide the attacker with a physical connection into the network. The device would leech POE power off of the ethernet cable though having a standalone power supply may be required because it may be possible for the target to actively monitor changes in POE power draw. 

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