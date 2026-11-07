# CCNA Mega Lab – Learning & Troubleshooting Journey

This repository documents my personal learning and troubleshooting process while completing a large CCNA-style Packet Tracer lab.

## Goal

The goal of this project is to reinforce CCNA networking concepts and configuration skills through hands-on practice.

## Skills Practiced

This section will be updated as I progress through the lab.

## Documentation

Each day/session will include notes about:

- Configurations completed
- Commands practiced
- Problems encountered
- Troubleshooting steps
- Key lessons learned

## Disclaimer

This repository contains my own notes, configurations, screenshots, and troubleshooting process. It does not include or reproduce proprietary course materials or video content.

## Part 1 – Initial Setup

### Objective
Configured the initial settings on all routers and switches in the network.

### Tasks completed
- Configured appropriate hostnames on all routers and switches.
- Configured the enable secret `jeremysitlab`, using Type 9 hashing where supported and Type 5 otherwise.
- Created the local user account `cisco` with a secret password, using Type 9 hashing where supported and Type 5 otherwise.
- Configured console access to authenticate using the local user database.
- Set the console inactivity timeout to 30 minutes.
- Enabled synchronous logging to prevent console messages from interrupting command input.

### Verification
Verified the hostname, authentication settings, and console configuration on the network devices.

## Part 2 – VLANs, Layer-2 EtherChannel

### Objective

Configured VLANs, Layer-2 EtherChannels, and trunk links across both offices to establish the required network segmentation and connectivity.

### Tasks completed

- Configured a Layer-2 EtherChannel between DSW-A1 and DSW-A2 using PAgP (Cisco proprietary), with both switches actively negotiating.
- Configured a Layer-2 EtherChannel between DSW-B1 and DSW-B2 using LACP (open standard), with both switches actively negotiating.
- Configured trunk links between Access and Distribution switches, including EtherChannel interfaces.
- Disabled DTP negotiation on all trunk links.
- Set the native VLAN to VLAN 1000 on all trunks.
- Restricted allowed VLANs on trunks according to each office's requirements.
- Configured VTP version 2, using `JeremysITLab` as the domain name.
- Configured one Distribution switch in each office as a VTP server and all Access switches as VTP clients.
- Created and propagated the required VLANs:
  - Office A: VLAN 10 (PCs), VLAN 20 (Phones), VLAN 40 (Wi-Fi), VLAN 99 (Management).
  - Office B: VLAN 10 (PCs), VLAN 20 (Phones), VLAN 30 (Servers), VLAN 99 (Management).
- Configured Access ports for PCs, IP phones, and the server.
- Configured ASW-A1's connection to WLC1 to support the Wi-Fi and Management VLANs, with the Management VLAN untagged.
- Administratively disabled unused ports on Access and Distribution switches.

### Verification

Verified VLAN creation and propagation, EtherChannel formation, trunk configurations, and interface status using appropriate show commands.
