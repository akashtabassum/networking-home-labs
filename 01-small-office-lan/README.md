# Lab 1 — Small Office LAN

📌 Overview

This is my first hands-on networking home lab, created using **Cisco Packet Tracer**.

The purpose of this lab was to build a basic Local Area Network (LAN), configure IPv4 addressing, connect PCs through a switch, configure a router interface, and test network connectivity.

---

🎯 Objectives

- Build a basic LAN topology
- Connect a router, switch, and PCs
- Configure a router interface with an IPv4 address
- Configure static IPv4 addressing
- Configure subnet masks and default gateways
- Verify switch connectivity
- Test network connectivity using `ping`
- Practice basic network troubleshooting
- Investigate DHCP operation

---
 🖥️ Devices Used

- **Cisco 2911 Router**
- **Cisco 2960 Switch**
- **3 PCs**

---

🌐 Network Topology

The topology consists of one router connected to a switch, with three PCs connected to the switch.

```text
                    ┌──────────────┐
                    │  Router 2911 │
                    │ 192.168.1.1  │
                    └──────┬───────┘
                           │
                           │
                    ┌──────┴───────┐
                    │  Switch 2960 │
                    └─┬────┬────┬──┘
                      │    │    │
                     PC0  PC1  PC2
🔌 Physical Connections

The following connections were made:

Switch Port	Connected Device
Fa0/1	Router
Fa0/2	PC0
Fa0/3	PC1
Fa0/4	PC2

The switch was checked using:

show interfaces status

The connected ports were shown as:

Fa0/1    connected
Fa0/2    connected
Fa0/3    connected
Fa0/4    connected
🔀 VLAN Verification

The switch VLAN configuration was checked using:

show vlan brief

The connected ports were found in the default VLAN 1:

VLAN 1    default    active    Fa0/1, Fa0/2, Fa0/3, Fa0/4

No additional VLANs were configured for this lab.

⚙️ Router Configuration

The router was accessed through the CLI.

Initially, the router was in User EXEC mode:

Router>

The enable command was used to enter Privileged EXEC mode:

Router> enable

The prompt changed to:

Router#

The router interface configuration was then verified using:

show ip interface brief

The result showed:

Interface              IP-Address      Status    Protocol

GigabitEthernet0/0     192.168.1.1     up        up

This confirmed that the router's GigabitEthernet0/0 interface was operational.

🌐 IP Addressing

The LAN uses the following network:

Network: 192.168.1.0/24
Subnet Mask: 255.255.255.0

The router interface was configured as:

192.168.1.1

PC0 was configured with a static IP address:

IP Address:       192.168.1.10
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.1.1
🧪 Initial DHCP Test

Before using a static IP, DHCP was tested on PC0.

PC0 initially showed:

IPv4 Address: 0.0.0.0
Subnet Mask:  0.0.0.0
Default Gateway: 0.0.0.0

The PC displayed:

DHCP request failed.
APIPA is being used.
🔍 DHCP Troubleshooting

The router was checked to determine whether a DHCP pool existed.

The following command was used:

show running-config | section dhcp

A DHCP pool named OFFICE-LAN was present:

ip dhcp pool OFFICE-LAN
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1

The DHCP pool was also checked using:

show ip dhcp pool

The pool showed:

Pool OFFICE-LAN
Total addresses: 254
Leased addresses: 0

DHCP bindings were checked using:

show ip dhcp binding

No DHCP leases were present.

DHCP conflicts were also checked using:

show ip dhcp conflict

No DHCP conflicts were reported.

🛠️ Connectivity Troubleshooting

To determine whether the problem was with the network connection or DHCP, PC0 was manually configured with a static IP address.

The following configuration was used:

IP Address:       192.168.1.10
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.1.1

After configuring the static IP, the PC was able to communicate with the router.

📡 Connectivity Test

From PC0, the following command was used:

ping 192.168.1.1

The result was:

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255

Packets: Sent = 4
Received = 4
Lost = 0 (0% loss)
Result

The successful ping confirmed that:

PC0 was connected to the switch
The switch was forwarding traffic
The router interface was reachable
The IP address and subnet mask were correctly configured
The default gateway was reachable

The successful static-IP test helped isolate the problem to the DHCP process rather than basic LAN connectivity.

🔎 Verification Commands Used

The following commands were used during the lab:

show ip interface brief

Used to verify router interface status and IP addresses.

show interfaces status

Used to verify switch port connectivity.

show vlan brief

Used to verify VLAN membership.

show running-config | section dhcp

Used to inspect the DHCP configuration.

show ip dhcp pool

Used to inspect the DHCP pool and available addresses.

show ip dhcp binding

Used to check DHCP leases.

show ip dhcp conflict

Used to check for DHCP address conflicts.

ipconfig

Used on the PC to verify its IP configuration.

ping 192.168.1.1

Used to test connectivity between PC0 and the router.

📚 What I Learned

Through this lab, I practiced:

Basic LAN topology design
IPv4 addressing
Subnet masks
Default gateways
Cisco router interface configuration
Cisco switch connectivity
VLAN verification
Static IP configuration
DHCP configuration and troubleshooting
Using ipconfig
Using ping for connectivity testing
Using Cisco IOS verification commands
Troubleshooting a network systematically
Using Cisco Packet Tracer for network simulation
🧰 Tools Used
Cisco Packet Tracer
Cisco 2911 Router
Cisco 2960 Switch
📸 Screenshots
Network Topology
<img width="2876" height="1702" alt="topology" src="https://github.com/user-attachments/assets/43589187-32c2-4f69-9fbc-65e0eb1bcdf4" />

PC0 IP Configuration
<img width="2874" height="1424" alt="pc0-router-ping" src="https://github.com/user-attachments/assets/2cc4fb5c-7691-404e-9bf2-9b3d3ee100e4" />

PC0 → Router Connectivity Test
<img width="2862" height="1426" alt="pc0-ip-config" src="https://github.com/user-attachments/assets/4b823687-70d1-49d7-b620-2cd70a8d06e7" />

PC1 IP Configuration
<img width="2876" height="1432" alt="pc1-router-ping" src="https://github.com/user-attachments/assets/f1f8a1c6-3422-495f-9237-bdebe0218ac0" />

PC1 → Router Connectivity Test
<img width="2880" height="1414" alt="Screenshot 2026-08-24 173704" src="https://github.com/user-attachments/assets/92de5dd8-f495-4719-a753-118310bc95e4" />

PC2 IP Configuration
<img width="2864" height="1420" alt="pc2-router-ping" src="https://github.com/user-attachments/assets/1e4c3392-2553-4d43-b922-64e394804d17" />

PC2 → Router Connectivity Test
<img width="2874" height="1430" alt="Screenshot 2026-08-24 173741" src="https://github.com/user-attachments/assets/15e37f08-a44b-4522-b61d-cb9b771cbb62" />




