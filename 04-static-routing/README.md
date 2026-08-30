# 🟡 Lab 4 — Static Routing

## 📌 Overview

This is the fourth networking home lab in my Cisco Packet Tracer networking series.

In this lab, I expanded the previous LAN topology by introducing a second router and a second network.

The main objective was to configure **static routing** between two separate LANs and verify end-to-end communication between devices located on different networks.

---

## 🎯 Objectives

- Build a network containing two routers
- Create two separate LAN networks
- Configure IPv4 addressing
- Configure router interfaces
- Configure a router-to-router connection
- Understand directly connected networks
- Configure static routes
- Understand next-hop addresses
- Verify routing tables
- Test end-to-end connectivity
- Troubleshoot routing connectivity
- Use Cisco IOS verification commands

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco 2911 Router × 2
- Cisco 2960 Switch × 2
- PCs × 4
- Copper Straight-Through cables
- Copper Cross-Over cable

---

# 🏗️ Network Topology

The network consists of two separate LANs connected through two routers.

    PC0                 PC2
     |                   |
    PC1                 PC3
     \                   /
      SW1             SW2
       |               |
       |               |
      R1──────────────R2
       |               |
       |               |
    LAN 1           LAN 2

The router-to-router connection uses the `10.0.0.0/30` network.

---

# 🌐 Network Design

The network was divided into three IP networks.

### LAN 1

    Network: 192.168.1.0/24
    Gateway: 192.168.1.1

### Router-to-Router Network

    Network: 10.0.0.0/30

    R1: 10.0.0.1
    R2: 10.0.0.2

### LAN 2

    Network: 192.168.2.0/24
    Gateway: 192.168.2.1

---

# 🔌 Physical Connections

The following connections were made:

| From | Port | To | Port | Cable |
|---|---|---|---|---|
| PC0 | FastEthernet0 | SW1 | Fa0/1 | Copper Straight-Through |
| PC1 | FastEthernet0 | SW1 | Fa0/2 | Copper Straight-Through |
| SW1 | Fa0/24 | R1 | Gig0/0 | Copper Straight-Through |
| R1 | Gig0/1 | R2 | Gig0/1 | Copper Cross-Over |
| R2 | Gig0/0 | SW2 | Fa0/24 | Copper Straight-Through |
| SW2 | Fa0/1 | PC2 | FastEthernet0 | Copper Straight-Through |
| SW2 | Fa0/2 | PC3 | FastEthernet0 | Copper Straight-Through |

---

# 💻 PC IP Configuration

The PCs were manually configured with static IPv4 addresses.

### PC0

    IP Address:       192.168.1.10
    Subnet Mask:      255.255.255.0
    Default Gateway:  192.168.1.1

### PC1

    IP Address:       192.168.1.11
    Subnet Mask:      255.255.255.0
    Default Gateway:  192.168.1.1

### PC2

    IP Address:       192.168.2.10
    Subnet Mask:      255.255.255.0
    Default Gateway:  192.168.2.1

### PC3

    IP Address:       192.168.2.11
    Subnet Mask:      255.255.255.0
    Default Gateway:  192.168.2.1

---

# ⚙️ Router 1 Configuration

R1 was configured with two interfaces.

### GigabitEthernet0/0

    IP Address: 192.168.1.1
    Subnet Mask: 255.255.255.0

### GigabitEthernet0/1

    IP Address: 10.0.0.1
    Subnet Mask: 255.255.255.252

The interfaces were enabled using:

    no shutdown

The configuration was verified using:

    show ip interface brief

The interfaces showed:

    GigabitEthernet0/0    192.168.1.1    up    up
    GigabitEthernet0/1    10.0.0.1       up    up

---

# ⚙️ Router 2 Configuration

R2 was configured with two interfaces.

### GigabitEthernet0/0

    IP Address: 192.168.2.1
    Subnet Mask: 255.255.255.0

### GigabitEthernet0/1

    IP Address: 10.0.0.2
    Subnet Mask: 255.255.255.252

The interfaces were enabled using:

    no shutdown

The configuration was verified using:

    show ip interface brief

The interfaces showed:

    GigabitEthernet0/0    192.168.2.1    up    up
    GigabitEthernet0/1    10.0.0.2       up    up

---

# 🔗 Router-to-Router Connectivity

Before configuring static routes, connectivity between R1 and R2 was tested.

From R1:

    ping 10.0.0.2

The result was:

    Success rate is 80 percent (4/5)

The first packet timed out during the initial test, which can occur while ARP resolution takes place in Packet Tracer.

A reverse test was then performed from R2:

    ping 10.0.0.1

The result was:

    Success rate is 100 percent (5/5)

This confirmed that the router-to-router connection was operational.

---

# 🧭 Initial Routing Table

Before static routes were configured, each router only knew about its directly connected networks.

### R1 knew:

    10.0.0.0/30
    192.168.1.0/24

R1 did not have a route to:

    192.168.2.0/24

### R2 knew:

    10.0.0.0/30
    192.168.2.0/24

R2 did not have a route to:

    192.168.1.0/24

The routing tables were verified using:

    show ip route

---

# 🛣️ Static Route Configuration

Static routes were manually configured on both routers.

## R1 Static Route

R1 was configured with a route to the LAN behind R2:

    ip route 192.168.2.0 255.255.255.0 10.0.0.2

This tells R1:

    To reach 192.168.2.0/24,
    forward traffic to 10.0.0.2.

The route appeared in the routing table as:

    S    192.168.2.0/24 [1/0] via 10.0.0.2

The `S` indicates that the route is a static route.

---

## R2 Static Route

R2 was configured with a route to the LAN behind R1:

    ip route 192.168.1.0 255.255.255.0 10.0.0.1

This tells R2:

    To reach 192.168.1.0/24,
    forward traffic to 10.0.0.1.

The route appeared in the routing table as:

    S    192.168.1.0/24 [1/0] via 10.0.0.1

---

# 📊 Final Routing Design

The final routing structure was:

    192.168.1.0/24
          |
         R1
      10.0.0.1
          |
          |
      10.0.0.0/30
          |
          |
      10.0.0.2
         R2
          |
    192.168.2.0/24

### R1

    192.168.2.0/24 → 10.0.0.2

### R2

    192.168.1.0/24 → 10.0.0.1

---

# 🧪 End-to-End Connectivity Test

After configuring the static routes, communication was tested between PCs located on different LANs.

## PC0 → PC2

From PC0:

    ping 192.168.2.10

Result:

    Reply from 192.168.2.10: bytes=32 time<1ms TTL=126
    Reply from 192.168.2.10: bytes=32 time=13ms TTL=126
    Reply from 192.168.2.10: bytes=32 time<1ms TTL=126
    Reply from 192.168.2.10: bytes=32 time<1ms TTL=126

    Packets: Sent = 4
    Received = 4
    Lost = 0 (0% loss)

### Result

✅ PC0 successfully communicated with PC2 across two different networks.

---

# 🔄 Reverse Connectivity Test

To verify two-way communication, PC2 was used to test connectivity back to PC0.

From PC2:

    ping 192.168.1.10

Result:

    Reply from 192.168.1.10: bytes=32 time<1ms TTL=126
    Reply from 192.168.1.10: bytes=32 time<1ms TTL=126
    Reply from 192.168.1.10: bytes=32 time<1ms TTL=126
    Reply from 192.168.1.10: bytes=32 time<1ms TTL=126

    Packets: Sent = 4
    Received = 4
    Lost = 0 (0% loss)

### Result

✅ Two-way communication was successfully verified.

---

# 🧠 Understanding the Packet Path

When PC0 sends a packet to PC2, the packet follows this path:

    PC0
    192.168.1.10
       ↓
    R1
    192.168.1.1
       ↓
    10.0.0.1
       ↓
    10.0.0.2
       ↓
    R2
    192.168.2.1
       ↓
    PC2
    192.168.2.10

R1 uses its static route to forward the packet toward R2.

R2 then delivers the packet to PC2.

For the return traffic, R2 uses its static route to send traffic back toward R1.

---

# 🔎 Verification Commands Used

### Check router interfaces

    show ip interface brief

Used to verify interface IP addresses and operational status.

### Check routing table

    show ip route

Used to verify connected and static routes.

### Test router-to-router connectivity

    ping 10.0.0.2

Used from R1 to test connectivity with R2.

### Test reverse router connectivity

    ping 10.0.0.1

Used from R2 to test connectivity with R1.

### Test end-to-end connectivity

    ping 192.168.2.10

Used from PC0 to test communication with PC2.

### Test reverse end-to-end connectivity

    ping 192.168.1.10

Used from PC2 to test communication with PC0.

---

# 📸 Screenshots

## Network Topology

Add the screenshot showing the complete topology:

    PC0 + PC1 → SW1 → R1 → R2 → SW2 → PC2 + PC3

---

## R1 Routing Table

Add the screenshot showing:

    show ip route

The screenshot should clearly show:

    S    192.168.2.0/24 [1/0] via 10.0.0.2

---

## R2 Routing Table

Add the screenshot showing:

    show ip route

The screenshot should clearly show:

    S    192.168.1.0/24 [1/0] via 10.0.0.1

---

## PC0 → PC2 Connectivity

Add the screenshot showing:

    ping 192.168.2.10

With:

    Sent = 4
    Received = 4
    Lost = 0%


---

## PC2 → PC0 Connectivity

Add the screenshot showing:

    ping 192.168.1.10

With:

    Sent = 4
    Received = 4
    Lost = 0%

---

# 📚 What I Learned

Through this lab, I learned and practiced:

- Static routing
- IPv4 network addressing
- Subnet masks
- Next-hop addresses
- Router-to-router communication
- Routing tables
- Directly connected routes
- Static routes
- End-to-end packet forwarding
- Default gateways
- Cisco IOS configuration
- Cisco IOS verification commands
- Network troubleshooting
- Cisco Packet Tracer simulation

---

# 🛠️ Troubleshooting

During the router-to-router connectivity test, the first ping from R1 resulted in:

    Success rate is 80 percent (4/5)

The first packet timed out while the subsequent packets succeeded.

A reverse test from R2 produced:

    Success rate is 100 percent (5/5)

The connection was therefore confirmed to be operational.

The final PC-to-PC tests produced:

    0% packet loss

This confirmed that the static routing configuration was functioning correctly.

---

# ✅ Final Result

The static routing lab was successfully completed.

Two separate LANs were connected through two routers using a point-to-point network.

The routers were configured with static routes that allowed traffic to travel between:

    192.168.1.0/24

and

    192.168.2.0/24

End-to-end connectivity was successfully verified in both directions with:

    0% packet loss

This lab demonstrated how routers use static routes to forward traffic between networks that are not directly connected.

---

# 📁 Project Structure

    04-static-routing/
    │
    ├── README.md
    ├── static-routing.pkt
    ├── topology.png
    ├── r1-routing-table.png
    ├── r2-routing-table.png
    ├── pc0-to-pc2-ping.png
    └── pc2-to-pc0-ping.png

---

