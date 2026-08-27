# 🧪 Lab 1 — Small Office LAN

## 📌 Objective

The objective of this lab was to build a basic Local Area Network (LAN) using Cisco Packet Tracer.

The lab focused on IPv4 addressing, router and switch configuration, static IP addressing, default gateways, VLAN verification, DHCP investigation, and connectivity testing.

This lab serves as the foundation for the networking home-lab series.

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco 2911 Router
- Cisco 2960 Switch
- 3 × PCs
- Copper Straight-Through Ethernet cables

---

# 🏗️ Network Topology

The network consists of one Cisco 2911 router connected to a Cisco 2960 switch, with three PCs connected to the switch.

    Router 2911
    192.168.1.1
          |
          |
    Switch 2960
      |    |    |
     PC0  PC1  PC2

![Network Topology](https://github.com/user-attachments/assets/43589187-32c2-4f69-9fbc-65e0eb1bcdf4)

---

# 1. 🎯 Objectives

The main objectives of this lab were to:

- Build a basic LAN topology
- Connect a router, switch, and PCs
- Configure IPv4 addressing
- Configure a router interface
- Configure static IPv4 addresses
- Configure subnet masks
- Configure default gateways
- Verify switch port connectivity
- Verify VLAN membership
- Investigate DHCP operation
- Perform basic network troubleshooting
- Test connectivity using ping
- Practice Cisco IOS verification commands

---

# 2. 🔌 Physical Connections

The following physical connections were made:

| Switch Port | Connected Device |
|---|---|
| Fa0/1 | Router |
| Fa0/2 | PC0 |
| Fa0/3 | PC1 |
| Fa0/4 | PC2 |

The switch ports were verified using:

    show interfaces status

The connected ports were shown as:

    Fa0/1    connected
    Fa0/2    connected
    Fa0/3    connected
    Fa0/4    connected

This confirmed that the router and all three PCs had active connections to the switch.

---

# 3. 🏷️ VLAN Verification

The switch VLAN configuration was checked using:

    show vlan brief

The connected devices were operating in the default VLAN:

    VLAN 1    default    active    Fa0/1, Fa0/2, Fa0/3, Fa0/4

No additional VLANs were configured for this lab.

This provided a simple Layer 2 LAN environment before introducing VLAN segmentation in later labs.

---

# 4. ⚙️ Router Configuration

The router was accessed through the Cisco IOS CLI.

Initially, the router was in User EXEC mode:

    Router>

The following command was used to enter Privileged EXEC mode:

    enable

The prompt changed to:

    Router#

The router interface configuration was then verified using:

    show ip interface brief

The relevant interface showed:

    Interface              IP-Address      Status    Protocol

    GigabitEthernet0/0     192.168.1.1     up        up

This confirmed that the router's GigabitEthernet0/0 interface was operational.

---

# 5. 🌐 IP Addressing

The LAN uses the following IPv4 network:

    Network:       192.168.1.0/24
    Subnet Mask:   255.255.255.0

The router interface was configured as:

    192.168.1.1

PC0 was configured with a static IPv4 address:

| Parameter | Value |
|---|---|
| IP Address | `192.168.1.10` |
| Subnet Mask | `255.255.255.0` |
| Default Gateway | `192.168.1.1` |

---

# 6. 🧪 Initial DHCP Test

Before using a static IP address, DHCP operation was tested on PC0.

Initially, PC0 showed:

    IPv4 Address:       0.0.0.0
    Subnet Mask:        0.0.0.0
    Default Gateway:    0.0.0.0

The PC displayed:

    DHCP request failed.
    APIPA is being used.

This indicated that PC0 was not successfully obtaining an IPv4 address through DHCP.

---

# 7. 🔍 DHCP Troubleshooting

The router was investigated to determine whether a DHCP pool existed.

The following command was used:

    show running-config | section dhcp

A DHCP pool named OFFICE-LAN was present:

    ip dhcp pool OFFICE-LAN
     network 192.168.1.0 255.255.255.0
     default-router 192.168.1.1

The DHCP pool was then inspected using:

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

---

# 8. 🛠️ Connectivity Troubleshooting

To determine whether the problem was related to the physical network or DHCP, PC0 was manually configured with a static IP address.

The following configuration was used:

    IP Address:       192.168.1.10
    Subnet Mask:      255.255.255.0
    Default Gateway:  192.168.1.1

After configuring the static IP address, PC0 was able to communicate with the router.

This helped isolate the issue to the DHCP process rather than basic LAN connectivity.

---

# 9. 📡 Connectivity Testing

Connectivity between PC0 and the router was tested using:

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

### Result

✅ Connectivity test successful

The successful ping confirmed that:

- PC0 was connected to the switch
- The switch was forwarding traffic
- The router interface was reachable
- The IP address was correctly configured
- The subnet mask was correct
- The default gateway was reachable

---

# 10. 📊 Network Configuration Summary

| Device | Interface | IP Address | Network |
|---|---|---|---|
| R1 | Gig0/0 | `192.168.1.1` | `192.168.1.0/24` |
| PC0 | NIC | `192.168.1.10` | `192.168.1.0/24` |
| PC1 | NIC | `192.168.1.x` | `192.168.1.0/24` |
| PC2 | NIC | `192.168.1.x` | `192.168.1.0/24` |

---

# 11. 🔎 Verification Commands

The following commands were used throughout the lab.

### Verify router interfaces

    show ip interface brief

Used to verify router interface status and IP addresses.

### Verify switch ports

    show interfaces status

Used to verify physical switch port connectivity.

### Verify VLAN membership

    show vlan brief

Used to verify VLAN membership and connected ports.

### Inspect DHCP configuration

    show running-config | section dhcp

Used to inspect the router's DHCP configuration.

### Check DHCP pool

    show ip dhcp pool

Used to inspect the DHCP pool and available addresses.

### Check DHCP leases

    show ip dhcp binding

Used to check whether DHCP leases had been assigned.

### Check DHCP conflicts

    show ip dhcp conflict

Used to check for reported DHCP address conflicts.

### Check PC configuration

    ipconfig

Used to verify the PC's IPv4 configuration.

### Test connectivity

    ping 192.168.1.1

Used to test connectivity between PC0 and the router.

---

# 12. 📸 Screenshots

## Network Topology

![Network Topology](https://github.com/user-attachments/assets/43589187-32c2-4f69-9fbc-65e0eb1bcdf4)

## PC0 IP Configuration

![PC0 IP Configuration](https://github.com/user-attachments/assets/2cc4fb5c-7691-404e-9bf2-9b3d3ee100e4)

## PC0 → Router Connectivity Test

![PC0 Router Ping](https://github.com/user-attachments/assets/4b823687-70d1-49d7-b620-2cd70a8d06e7)

## PC1 IP Configuration

![PC1 IP Configuration](https://github.com/user-attachments/assets/f1f8a1c6-3422-495f-9237-bdebe0218ac0)

## PC1 → Router Connectivity Test

![PC1 Router Ping](https://github.com/user-attachments/assets/92de5dd8-f495-4719-a753-118310bc95e4)

## PC2 IP Configuration

![PC2 IP Configuration](https://github.com/user-attachments/assets/1e4c3392-2553-4d43-b922-64e394804d17)

## PC2 → Router Connectivity Test

![PC2 Router Ping](https://github.com/user-attachments/assets/15e37f08-a44b-4522-b61d-cb9b771cbb62)

---

# 13. 🧠 What I Learned

Through this lab, I practiced:

- Basic LAN topology design
- IPv4 addressing
- Subnet masks
- Default gateways
- Cisco router interface configuration
- Cisco switch connectivity
- VLAN verification
- Static IP configuration
- DHCP configuration and troubleshooting
- Using ipconfig
- Using ping for connectivity testing
- Using Cisco IOS verification commands
- Systematic network troubleshooting
- Network simulation using Cisco Packet Tracer

---

# 14. 🛠️ Troubleshooting Summary

The main issue encountered during the lab was the failed DHCP request.

The PC initially received:

    IPv4 Address: 0.0.0.0

and reported:

    DHCP request failed.
    APIPA is being used.

The router's DHCP configuration was investigated using several Cisco IOS commands.

Although the OFFICE-LAN DHCP pool existed, no DHCP leases were present.

A static IP configuration was then applied to PC0:

    192.168.1.10/24
    Gateway: 192.168.1.1

The successful ping to the router confirmed that the underlying LAN connectivity was functioning.

This troubleshooting process demonstrated how to separate Layer 2/Layer 3 connectivity problems from DHCP-specific problems.

---

# 15. ✅ Final Result

The Small Office LAN was successfully built and tested.

The final network provided:

    PC0 ─┐
    PC1 ─┼── SW1 ─── R1
    PC2 ─┘          192.168.1.1

The router, switch, and PCs were successfully connected, IPv4 addressing was configured, and connectivity between PC0 and the router was verified with 0% packet loss.

The DHCP issue was investigated systematically, and static addressing was used to confirm that the underlying LAN was operational.

---

# 📁 Project Structure

    01-basic-lan/
    │
    ├── README.md
    ├── small-office-lan.pkt
    ├── topology.png
    ├── pc0-ip-config.png
    ├── pc0-router-ping.png
    ├── pc1-ip-config.png
    ├── pc1-router-ping.png
    ├── pc2-ip-config.png
    └── pc2-router-ping.png

---
