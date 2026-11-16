# CCNA Mega Lab – Learning & Troubleshooting Journey

A hands-on Cisco Packet Tracer project documenting my journey through a comprehensive CCNA-style enterprise network lab. This repository showcases my networking configurations, troubleshooting experience, verification commands, and practical understanding of networking concepts.

## Project Overview

The project involves building and configuring a multi-office enterprise network, covering Layer 2 and Layer 3 technologies, network services, security, IPv6, and wireless networking.

The lab was completed using Cisco Packet Tracer, with configurations and verification documented throughout each stage.

## Project Files

- **Packet Tracer Project (`.pka`):** The complete Cisco Packet Tracer lab file, containing the network topology and device configurations.
- **Screenshots:** A collection of configuration and verification screenshots organized by lab section.

## Skills & Technologies Practiced

- **Network Fundamentals:** IPv4, IPv6, subnetting, MAC addressing.
- **Switching:** VLANs, VTP, trunking, EtherChannel, STP, Rapid PVST+.
- **Routing:** Static Routing, OSPF, HSRP, IPv6 routing.
- **Network Services:** DHCP, DNS, NTP, SNMP, Syslog, FTP, NAT/PAT.
- **Network Security:** ACLs, Port Security, DHCP Snooping, Dynamic ARP Inspection.
- **Wireless Networking:** WLC configuration, WLANs, LWAPs, WPA2.
- **Network Management:** SSH, CDP, LLDP.
- **Troubleshooting:** Connectivity testing, routing verification, and network diagnostics.

---

# Lab Documentation

## Part 1 – Initial Setup

### Objective
Configured the initial settings on all routers and switches in the network.

### Completed Tasks
- Configured appropriate hostnames on all routers and switches.
- Configured the enable secret `jeremysitlab`, using Type 9 hashing where supported and Type 5 otherwise.
- Created the local user account `cisco` with a secret password, using Type 9 hashing where supported and Type 5 otherwise.
- Configured console access to authenticate using the local user database.
- Set the console inactivity timeout to 30 minutes.
- Enabled synchronous logging to prevent console messages from interrupting command input.

### Verification
Verified hostname, authentication settings, and console configurations across the network devices.

### Screenshots
- [part1-console-configuration.png](screenshots/part1-console-configuration.png)
- [part1-hostname-and-security.png](screenshots/part1-hostname-and-security.png)

---

## Part 2 – VLANs & Layer-2 EtherChannel

### Objective
Configured VLANs, Layer-2 EtherChannels, and trunk links across both offices to establish network segmentation and connectivity.

### Completed Tasks
- Configured a Layer-2 EtherChannel between DSW-A1 and DSW-A2 using PAgP, with both switches actively negotiating.
- Configured a Layer-2 EtherChannel between DSW-B1 and DSW-B2 using LACP, with both switches actively negotiating.
- Configured trunk links between Access and Distribution switches, including EtherChannel interfaces.
- Disabled DTP negotiation on all trunk links.
- Set the native VLAN to VLAN 1000 on all trunks.
- Restricted allowed VLANs on trunks according to each office's requirements.
- Configured VTP Version 2 using `JeremysITLab` as the domain name.
- Configured one Distribution switch in each office as a VTP server and all Access switches as VTP clients.
- Created and propagated the required VLANs:
  - **Office A:** VLAN 10 (PCs), VLAN 20 (Phones), VLAN 40 (Wi-Fi), VLAN 99 (Management).
  - **Office B:** VLAN 10 (PCs), VLAN 20 (Phones), VLAN 30 (Servers), VLAN 99 (Management).
- Configured Access ports for PCs, IP phones, and the server.
- Configured ASW-A1's connection to WLC1 to support the Wi-Fi and Management VLANs, with the Management VLAN untagged.
- Administratively disabled unused ports on Access and Distribution switches.

### Verification
Verified VLAN creation and propagation, EtherChannel formation, trunk configurations, and interface status using appropriate `show` commands.

### Screenshots
- [part2-interface-status.png](screenshots/part2-interface-status.png)
- [part2-vlan-and-vtp-configuration.png](screenshots/part2-vlan-and-vtp-configuration.png)
- [part2-vtp-status.png](screenshots/part2-vtp-status.png)

---

## Part 3 – IP Addressing, Layer-3 EtherChannel & HSRP

### Objective
Configured Layer 3 addressing and gateway redundancy to provide connectivity and high availability across the network.

### Completed Tasks
- Configured IPv4 addressing on R1, including two DHCP client interfaces and a Loopback interface.
- Enabled IPv4 routing on the Core and Distribution switches.
- Configured a Layer-3 EtherChannel between CSW1 and CSW2 using PAgP, with both switches actively negotiating.
- Configured IPv4 addresses on routed interfaces and Loopback interfaces of CSW1, CSW2, DSW-A1, DSW-A2, DSW-B1, and DSW-B2.
- Disabled unused interfaces on the Core switches.
- Manually configured the IP address, subnet mask, and default gateway on SRV1.
- Configured management IP addresses on VLAN 99 for all Access switches.
- Configured the appropriate first usable IP address in each management subnet as the default gateway for the Access switches.
- Configured HSRPv2 for Management, PC, Phone, Wi-Fi, and Server VLANs across Office A and Office B.
- Configured HSRP priorities and preemption to ensure the intended Distribution switch became the Active router for each subnet.

### HSRP Configuration

**Office A**

| VLAN / Network | HSRP Group | Virtual IP | Active Router |
|---|---:|---|---|
| Management – 10.0.0.0/28 | 1 | 10.0.0.1 | DSW-A1 |
| PCs – 10.1.0.0/24 | 2 | 10.1.0.1 | DSW-A1 |
| Phones – 10.2.0.0/24 | 3 | 10.2.0.1 | DSW-A2 |
| Wi-Fi – 10.6.0.0/24 | 4 | 10.6.0.1 | DSW-A2 |

**Office B**

| VLAN / Network | HSRP Group | Virtual IP | Active Router |
|---|---:|---|---|
| Management – 10.0.0.16/28 | 1 | 10.0.0.17 | DSW-B1 |
| PCs – 10.3.0.0/24 | 2 | 10.3.0.1 | DSW-B1 |
| Phones – 10.4.0.0/24 | 3 | 10.4.0.1 | DSW-B2 |
| Servers – 10.5.0.0/24 | 4 | 10.5.0.1 | DSW-B2 |

### Verification
Verified the following:
- Interface IP addressing and operational status.
- Layer-3 EtherChannel formation and PAgP negotiation.
- IPv4 routing.
- HSRPv2 Active and Standby roles.
- Correct HSRP virtual IP addresses.
- Access switch management connectivity.

### Screenshots
- [part3-interface-brief.png](screenshots/part3-interface-brief.png)
- [part3-office-a-standby-routers.png](screenshots/part3-office-a-standby-routers.png)
- [part3-officeB-standby-routers.png](screenshots/part3-officeB-standby-routers.png)

---

## Part 4 – Rapid Spanning Tree Protocol (Rapid PVST+)

### Objective
Configured Rapid PVST+ across Access and Distribution switches to prevent Layer 2 switching loops and provide redundant paths.

### Completed Tasks
- Configured Rapid PVST+ on all Access and Distribution switches.
- Configured the Distribution switch acting as the HSRP Active router for each VLAN as the STP Root Bridge by assigning the lowest possible STP priority.
- Configured the corresponding HSRP Standby router with an STP priority one increment above the Root Bridge.
- Aligned the STP Root Bridge with the HSRP Active gateway for more efficient traffic forwarding.
- Enabled PortFast on ports connected to end devices, including WLC1.
- Enabled BPDU Guard on end-host ports to protect against potential Layer 2 loops caused by incorrectly connected switches.

### STP and HSRP Alignment

| Location | VLANs | STP Root Bridge |
|---|---|---|
| Office A | VLAN 10, VLAN 99 | DSW-A1 |
| Office A | VLAN 20, VLAN 40 | DSW-A2 |
| Office B | VLAN 10, VLAN 99 | DSW-B1 |
| Office B | VLAN 20, VLAN 30 | DSW-B2 |

The HSRP Standby routers were configured with the next-highest STP priority to act as the secondary Root Bridge.

### Verification
Verified Rapid PVST+, Root Bridge elections, STP priorities, redundant link handling, PortFast, and BPDU Guard configurations.

### Screenshots
- [part4-officeA-root-bridge.png](screenshots/part4-officeA-root-bridge.png)
- [part4-portfast-bpdu-guard.png](screenshots/part4-portfast-bpdu-guard.png)

---

## Part 5 – Static & Dynamic Routing

### 1. OSPF Configuration

- Configured OSPF on R1's LAN-facing interfaces and all Core and Distribution switches.
- Used OSPF process ID 1 and Area 0.
- Manually configured each device's Router ID (RID) to match its Loopback interface IP.
- Used the `network` command with exact IP addresses and wildcard mask `0.0.0.0` on switches.
- Enabled OSPF directly on R1's interfaces using interface configuration mode.
- Enabled OSPF on all Loopback interfaces and configured them as passive.
- Configured Distribution switch SVIs as passive, except for the Management VLAN SVI.
- Configured physical OSPF neighbor connections to use point-to-point network type to prevent DR/BDR elections.
- Left the Layer-3 Port-channel interfaces between CSW1 and CSW2 at their default network type.

### 2. Static Default Routing

- Configured recursive static default routes on R1 for both Internet connections.
- Configured the route through G0/1/0 as a floating static route with an administrative distance of 2.
- Configured R1 to function as an OSPF Autonomous System Boundary Router (ASBR).
- Used `default-information originate` to advertise R1's default route to other routers in the OSPF domain.

### Verification
Commands used to verify the configuration:

- `show ip ospf` – Verify OSPF process and Router ID.
- `show ip ospf neighbor` – Verify OSPF neighbor adjacencies.

### Screenshots
- [part5-ospf-routerA.png](screenshots/part5-ospf-routerA.png)
- [part5-ospf-switch-configuration.png](screenshots/part5-ospf-switch-configuration.png)
- [part5-show-ospf.png](screenshots/part5-show-ospf.png)

---

## Part 6 – Network Services, Security & NAT

### Objective
Configured essential network services across the enterprise network, including DHCP, DNS, NTP, SNMP, Syslog, FTP, SSH, and NAT. Also configured secure remote management and tested Internet connectivity and failover.

### 1. DHCP Configuration

- Configured seven DHCP pools on R1 for Office A, Office B, and Wi-Fi clients.
- Configured the appropriate subnet, default gateway, domain name (`jeremysitlab.com`), and DNS server (`10.5.0.4`) for each pool.
- Excluded the first ten usable host addresses from each DHCP pool.
- Configured DHCP Option 43 in the Management pools to provide the WLC address (`10.0.0.7`).
- Configured the Distribution switches to relay DHCP broadcasts to R1's Loopback0 address.

### 2. DNS Configuration

- Configured DNS records on SRV1 for `google.com`, `youtube.com`, and `jeremysitlab.com`.
- Configured `www.jeremysitlab.com` as an alias of `jeremysitlab.com`.
- Configured all routers and switches to use `jeremysitlab.com` as their domain name and SRV1 (`10.5.0.4`) as their DNS server.

### 3. NTP Configuration

- Configured R1 as a Stratum 5 NTP server.
- Configured R1 to synchronize its time with the external NTP server (`216.239.35.0`).
- Configured all Core, Distribution, and Access switches to use R1's Loopback0 interface as their NTP server.
- Configured NTP authentication using key number 1 and password `ccna`.

### 4. SNMP & Syslog

- Configured the SNMP community string `SNMPSTRING` on all routers and switches.
- Restricted SNMP access to read-only (GET) operations.
- Configured all routers and switches to send Syslog messages to SRV1.
- Enabled logging of all severity levels and configured buffered logging with 8192 bytes of memory.

### 5. FTP & IOS Upgrade

- Configured R1's default FTP credentials using username `cisco` and password `cisco`.
- Used FTP to transfer the new IOS image (`c2900-universalk9-mz.SPA.155-3.M4a.bin`) from SRV1 to R1's flash storage.
- Configured R1 to boot using the new IOS image.
- Rebooted R1 and removed the previous IOS image from flash.

### 6. SSH Configuration

- Generated 4096-bit RSA key pairs on all routers and switches.
- Enabled SSH Version 2 (SSHv2) for secure remote management.
- Created standard ACL 1 to permit SSH access only from Office A's PCs subnet (`10.1.0.0/24`).
- Applied ACL 1 inbound to all VTY lines to restrict remote access.
- Configured VTY lines to accept SSH connections only, disabling Telnet access.
- Enabled local username authentication using `login local`.
- Configured synchronous logging on all VTY lines.

### 7. NAT Configuration

- Configured static NAT on R1 to allow external access to SRV1 using public IP address `203.0.113.113`.
- Configured standard ACL 2 to identify the inside local networks permitted to access the Internet:
  - Office A PCs: `10.1.0.0/24`
  - Office A Phones: `10.2.0.0/24`
  - Office B PCs: `10.3.0.0/24`
  - Office B Phones: `10.4.0.0/24`
  - Wi-Fi: `10.6.0.0/24`
- Configured a dynamic NAT pool named `POOL1` with public IP addresses ranging from `203.0.113.200` to `203.0.113.207` using a `/29` subnet mask.
- Mapped ACL 2 to POOL1 and enabled Port Address Translation (PAT).
- Verified Internet connectivity by pinging `jeremysitlab.com`.
- Tested Internet link failover by disabling R1's primary ISP interface and verifying connectivity through the secondary ISP connection.

### 8. CDP & LLDP

- Disabled Cisco Discovery Protocol (CDP) on all network devices.
- Enabled Link Layer Discovery Protocol (LLDP) as the alternative device discovery protocol.
- Disabled LLDP transmission on the specified Access switch access ports (F0/1).

### Verification
Verified:
- DHCP address allocation and DHCP relay operation.
- DNS name resolution using SRV1.
- NTP server configuration and authentication.
- SNMP and centralized Syslog configuration.
- Successful FTP transfer and IOS upgrade on R1.
- SSHv2 operation and ACL-based remote access restrictions.
- Static NAT and dynamic PAT translations.
- Internet connectivity and ISP failover.
- CDP disabled and LLDP enabled across network devices.

### Screenshots
- [part6-acl-config.png](screenshots/part6-acl-config.png)
- [part6-dhcp-pool-config.png](screenshots/part6-dhcp-pool-config.png)
- [part6-nat-translations.png](screenshots/part6-nat-translations.png)
- [part6-ssh-acl.png](screenshots/part6-ssh-acl.png)
- [part6-ssh-login.png](screenshots/part6-ssh-login.png)

---

## Part 7 – Security: ACLs & Layer-2 Security Features

### Objective
Configured network access controls and Layer-2 security features to protect the network against unauthorized access and common attacks.

### 1. Extended ACLs

- Created an extended ACL (`OfficeA_to_OfficeB`) to control traffic between Office A and Office B.
- Allowed ICMP traffic from Office A PCs to Office B PCs.
- Denied all other traffic between the two PC subnets while permitting all other traffic.
- Applied the ACL following extended ACL placement best practices.

### 2. Port Security

- Configured Port Security on F0/1 of each Access switch.
- Restricted the number of permitted MAC addresses.
- Configured violation handling to block unauthorized traffic without affecting valid traffic.
- Enabled notifications for security violations.
- Configured sticky MAC learning to automatically save secure MAC addresses to the running configuration.

### 3. DHCP Snooping

- Enabled DHCP Snooping on all active VLANs across the Access switches.
- Configured trusted uplink ports towards the Distribution switches.
- Disabled DHCP Option 82 insertion.
- Applied a DHCP rate limit of 15 packets per second on active untrusted ports.
- Increased the rate limit to 100 packets per second on ASW-A1's connection to WLC1.

### 4. Dynamic ARP Inspection (DAI)

- Enabled DAI on all active VLANs across the Access switches.
- Configured trusted ports.
- Enabled optional ARP validation checks to help prevent ARP spoofing attacks.

### Screenshots
- [part7-acl-extended-config.png](screenshots/part7-acl-extended-config.png)
- [part7-port-security-configuration.png](screenshots/part7-port-security-configuration.png)
- [part7-dhcp-snooping-config.png](screenshots/part7-dhcp-snooping-config.png)
- [part7-dai-configuration.png](screenshots/part7-dai-configuration.png)

---

## Part 8 – IPv6

### Objective
Configured IPv6 addressing and static routing to prepare the network for an IPv6 migration.

### 1. IPv6 Addressing

- Enabled IPv6 routing on R1, CSW1, and CSW2.
- Configured static IPv6 addresses on R1's G0/0/0 and G0/1/0 interfaces.
- Used EUI-64 to automatically generate interface IDs on R1's LAN-facing interfaces and the corresponding Core Switch interfaces.
- Enabled IPv6 on CSW1 and CSW2 Port-channel 1 interfaces without manually assigning IPv6 addresses.

### 2. IPv6 Static Default Routes

- Configured a recursive static default route through next hop `2001:db8:a::1`.
- Configured a fully specified floating static default route through next hop `2001:db8:b::1`.
- Set the floating route's administrative distance to 2, making it a backup to the primary route (AD 1).

### Screenshots
- [part7-ipv6-configurations.png](screenshots/part7-ipv6-configurations.png)
- [part8-ipv6-route-config.png](screenshots/part8-ipv6-route-config.png)

---

## Part 9 – Wireless

### Objective
Configured a Wireless LAN Controller (WLC) to provide wireless connectivity through centrally managed Lightweight Access Points (LWAPs).

### 1. WLC Management

- Accessed the WLC1 web GUI from a PC using its management IP address (`10.0.0.7`).

### 2. Dynamic Interface Configuration

- Created a dynamic interface named `Wi-Fi` on VLAN 40.
- Configured port 1 and assigned IP address `10.6.0.4/24`.
- Set the default gateway to `10.6.0.1`.
- Configured the DHCP server address as `10.0.0.76`.
- Used `10.6.0.4` instead of `10.6.0.2` to avoid an IP address conflict with DSW-A1.

### 3. WLAN Configuration

- Created and enabled a WLAN with profile name and SSID `Wi-Fi`.
- Assigned WLAN ID 1.
- Configured WPA2 security with AES encryption.
- Set the pre-shared key (PSK) to `cisco123`.

### 4. Lightweight Access Point Verification

- Verified that both LWAPs successfully associated with WLC1.
- Noted Packet Tracer's limitation preventing wireless clients from obtaining IP addresses through the Wi-Fi DHCP pool.

### Screenshots
- [part9-gui-configuration.png](screenshots/part9-gui-configuration.png)
- [part9-lwap-security-config.png](screenshots/part9-lwap-security-config.png)

---

## Key Takeaways

Completing this project provided hands-on experience with configuring and troubleshooting a multi-office enterprise network.

The lab covered:
- Building a segmented network using VLANs and trunking.
- Implementing Layer-2 and Layer-3 EtherChannels.
- Configuring redundant default gateways using HSRP.
- Preventing switching loops using Rapid PVST+.
- Implementing dynamic routing with OSPF and static route failover.
- Deploying essential network services and NAT.
- Securing network infrastructure using ACLs and Layer-2 security features.
- Configuring IPv6 addressing and routing.
- Managing wireless networks using a Wireless LAN Controller.

## Disclaimer

This repository contains my own notes, configurations, screenshots, and troubleshooting process. It does not include or reproduce proprietary course materials or video content.
