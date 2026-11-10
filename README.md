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

## Part 3 – IP Addresses, Layer-3 EtherChannel & HSRP

In this section, I configured the Layer 3 addressing and gateway redundancy for the network.

### Completed Tasks

- Configured IPv4 addressing on **R1**, including two DHCP client interfaces and a Loopback interface.
- Enabled **IPv4 routing** on the Core and Distribution switches.
- Configured a **Layer 3 EtherChannel** between CSW1 and CSW2 using Cisco's proprietary **PAgP** protocol, with both switches actively negotiating the EtherChannel.
- Configured IPv4 addresses on the routed interfaces and Loopback interfaces of **CSW1, CSW2, DSW-A1, DSW-A2, DSW-B1, and DSW-B2**.
- Disabled unused interfaces on the Core switches.
- Configured the IP address, subnet mask, and default gateway manually on **SRV1**.
- Configured management IP addresses on **VLAN 99** for all Access switches.
- Configured the appropriate first usable IP address in each management subnet as the default gateway for the Access switches.
- Configured **HSRPv2** for the Management, PC, Phone, Wi-Fi, and Server VLANs across Office A and Office B.
- Configured HSRP priorities and preemption to ensure the intended Distribution switch became the Active router for each subnet.

### HSRP Configuration

#### Office A

| VLAN / Network | HSRP Group | Virtual IP | Active Router |
|---|---:|---|---|
| Management – 10.0.0.0/28 | 1 | 10.0.0.1 | DSW-A1 |
| PCs – 10.1.0.0/24 | 2 | 10.1.0.1 | DSW-A1 |
| Phones – 10.2.0.0/24 | 3 | 10.2.0.1 | DSW-A2 |
| Wi-Fi – 10.6.0.0/24 | 4 | 10.6.0.1 | DSW-A2 |

#### Office B

| VLAN / Network | HSRP Group | Virtual IP | Active Router |
|---|---:|---|---|
| Management – 10.0.0.16/28 | 1 | 10.0.0.17 | DSW-B1 |
| PCs – 10.3.0.0/24 | 2 | 10.3.0.1 | DSW-B1 |
| Phones – 10.4.0.0/24 | 3 | 10.4.0.1 | DSW-B2 |
| Servers – 10.5.0.0/24 | 4 | 10.5.0.1 | DSW-B2 |

### Verification

The configuration was verified using appropriate `show` commands to confirm:

- Interface IP addressing and operational status
- Layer 3 EtherChannel formation
- PAgP negotiation
- IPv4 routing
- HSRPv2 Active and Standby roles
- Correct HSRP virtual IP addresses
- Access switch management connectivity

## Part 4 – Rapid Spanning Tree Protocol (Rapid PVST+)

In this section, I configured Rapid PVST+ across the Access and Distribution switches to prevent Layer 2 switching loops and provide redundant paths.

### Completed Tasks

- Configured **Rapid PVST+** on all Access and Distribution switches.
- Configured the Distribution switch acting as the **HSRP Active router** for each VLAN as the **STP Root Bridge** by assigning the lowest possible STP priority.
- Configured the corresponding HSRP Standby router with an STP priority one increment above the Root Bridge.
- Aligned the STP Root Bridge with the HSRP Active gateway to provide more efficient traffic forwarding.
- Enabled **PortFast** on ports connected to end devices, including WLC1.
- Enabled **BPDU Guard** on all end-host ports to protect the network from potential Layer 2 loops caused by incorrectly connected switches.

### STP and HSRP Alignment

The STP Root Bridge was configured to match the HSRP Active router for each VLAN:

| Location | VLANs | STP Root Bridge |
|---|---|---|
| Office A | VLAN 10, VLAN 99 | DSW-A1 |
| Office A | VLAN 20, VLAN 40 | DSW-A2 |
| Office B | VLAN 10, VLAN 99 | DSW-B1 |
| Office B | VLAN 20, VLAN 30 | DSW-B2 |

The HSRP Standby routers were configured with the next-highest STP priority to act as the secondary Root Bridge.

### Verification

The configuration was verified using STP show commands to confirm:

- Rapid PVST+ was enabled.
- The intended Distribution switches became the Root Bridge for their assigned VLANs.
- STP priorities were correctly configured.
- Redundant links were managed to prevent Layer 2 loops.
- PortFast and BPDU Guard were enabled on end-host ports.

## Part 5 – Static and Dynamic Routing

### 1. OSPF Configuration

- Configured OSPF on R1 (LAN-facing interfaces), all Core switches and all Distribution switches.
- Used OSPF process ID 1 and Area 0.
- Manually configured each device's Router ID (RID) to match its Loopback interface IP.
- Used the `network` command with exact IP addresses and wildcard mask `0.0.0.0` on switches.
- Enabled OSPF directly on R1's interfaces using interface configuration mode.
- Enabled OSPF on all Loopback interfaces and configured them as passive.
- Configured Distribution switch SVIs as passive, except for the Management VLAN SVI.
- Configured physical OSPF neighbor connections to use point-to-point network type to prevent DR/BDR elections.
- Left the Layer 3 Port-channel interfaces between CSW1 and CSW2 at their default network type.

### 2. Static Default Routing

- Configured recursive static default routes on R1 for both Internet connections.
- Configured the route through G0/1/0 as a floating static route with an administrative distance of 2.
- Configured R1 to function as an OSPF Autonomous System Boundary Router (ASBR).
- Used `default-information originate` to advertise R1's default route to other routers in the OSPF domain.

### Verification

Commands used to verify the configuration:

- `show ip ospf` – Verify OSPF process and Router ID.
- `show ip ospf neighbor` – Verify OSPF neighbor adjacencies.


