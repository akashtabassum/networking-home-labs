# 🟡 Lab 5 — OSPF Dynamic Routing

## 📌 Overview

This is the fifth networking home lab in my Cisco Packet Tracer networking series.

In this lab, I expanded the previous static routing topology by replacing manually configured static routes with **OSPF (Open Shortest Path First)** dynamic routing.

The main objective was to configure OSPF between two routers, establish an OSPF neighbor relationship, exchange routing information, and verify end-to-end communication between devices located on different networks.

---

## 🎯 Objectives

- Build a network containing two routers
- Create two separate LAN networks
- Configure IPv4 addressing
- Configure router interfaces
- Configure a router-to-router connection
- Configure OSPF dynamic routing
- Configure OSPF Area 0
- Establish an OSPF neighbor relationship
- Verify OSPF routing information
- Verify routing tables
- Test end-to-end connectivity
- Understand dynamic route learning
- Compare OSPF with static routing
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

The same physical topology from Lab 4 was used for this lab.

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

# 🛣️ Removing Static Routes

This lab was created from a copy of Lab 4.

Lab 4 used static routing, so the static routes were removed before configuring OSPF.

On R1:

    no ip route 192.168.2.0 255.255.255.0 10.0.0.2

On R2:

    no ip route 192.168.1.0 255.255.255.0 10.0.0.1

This allowed OSPF to dynamically learn the remote networks.

---

# 🔄 OSPF Configuration

OSPF was configured using process ID `1`.

Both routers were placed in **Area 0**, also known as the backbone area.

---

# ⚙️ OSPF Configuration — R1

The following commands were used on R1:

    enable
    configure terminal
    router ospf 1
    network 192.168.1.0 0.0.0.255 area 0
    network 10.0.0.0 0.0.0.3 area 0
    exit
    exit

R1 advertises the following networks:

    192.168.1.0/24
    10.0.0.0/30

---

# ⚙️ OSPF Configuration — R2

The following commands were used on R2:

    enable
    configure terminal
    router ospf 1
    network 192.168.2.0 0.0.0.255 area 0
    network 10.0.0.0 0.0.0.3 area 0
    exit
    exit

R2 advertises the following networks:

    192.168.2.0/24
    10.0.0.0/30

---

# 🤝 OSPF Neighbor Verification

The OSPF neighbor relationship was verified using:

    show ip ospf neighbor

R1 successfully identified R2 as an OSPF neighbor.

The neighbor state was:

    FULL/BDR

The output showed:

    Neighbor ID     Pri   State       Dead Time   Address      Interface

    192.168.2.1     1     FULL/BDR    00:00:33    10.0.0.2     GigabitEthernet0/1

The `FULL` state confirmed that the OSPF adjacency was successfully established.

R2 also successfully identified R1 as its OSPF neighbor.

---

# 🧭 OSPF Routing Table

The routing table was verified using:

    show ip route

Before OSPF was configured, each router only knew about its directly connected networks.

After OSPF was configured, the routers dynamically learned the remote LAN networks.

### R1 learned:

    192.168.2.0/24

through OSPF.

### R2 learned:

    192.168.1.0/24

through OSPF.

Routes learned through OSPF are identified by:

    O

The `O` indicates an OSPF-learned route.

---

# 🔍 OSPF Process Verification

The OSPF process was verified using:

    show ip ospf

The output confirmed:

    Routing Process "ospf 1"

    Number of areas in this router is 1

    Area BACKBONE(0)

This confirmed that OSPF process 1 was running and the router was participating in Area 0.

---

# 🔎 OSPF Protocol Verification

The configured OSPF networks were verified using:

    show ip protocols

The output showed:

    Routing Protocol is "ospf 1"

    Routing for Networks:

    192.168.1.0 0.0.0.255 area 0
    10.0.0.0 0.0.0.3 area 0

This confirmed that the correct networks were being advertised through OSPF.

---

# 📡 Router-to-Router Connectivity Test

Before testing end-to-end connectivity, the router-to-router connection was verified.

From R1:

    ping 10.0.0.2

The ping was successful.

From R2:

    ping 10.0.0.1

The ping was also successful.

This confirmed that the underlying router-to-router connection was operational and ready for OSPF communication.

---

# 🧪 End-to-End Connectivity Test

After configuring OSPF, communication was tested between PCs located on different LANs.

## PC0 → PC2

From PC0:

    ping 192.168.2.10

The ping was successful.

The test confirmed that PC0 on:

    192.168.1.0/24

could communicate with PC2 on:

    192.168.2.0/24

---

# 🔄 Reverse Connectivity Test

The reverse direction was also tested.

From PC2:

    ping 192.168.1.10

The ping was successful with all four packets received.

This confirmed two-way communication between the two LANs.

---

# 📊 Test Results

| Test | Result |
|---|---|
| R1 → R2 connectivity | ✅ Successful |
| R2 → R1 connectivity | ✅ Successful |
| OSPF neighbor formation | ✅ FULL |
| R1 learns LAN 2 | ✅ Successful |
| R2 learns LAN 1 | ✅ Successful |
| PC0 → PC2 ping | ✅ Successful |
| PC2 → PC0 ping | ✅ Successful |
| Packet loss | ✅ 0% |

---

# 🧠 Understanding OSPF Packet Routing

When PC0 sends traffic to PC2, the packet follows this path:

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

R1 uses the OSPF-learned route to reach the network behind R2.

R2 then forwards the packet to PC2.

For the return traffic, R2 uses the OSPF-learned route to reach the network behind R1.

---

# 🆚 Static Routing vs OSPF

Lab 4 used static routing.

In Lab 4, routes were manually configured:

    R1 → 192.168.2.0/24 via 10.0.0.2

    R2 → 192.168.1.0/24 via 10.0.0.1

In this lab, OSPF was used instead.

The routers automatically exchanged routing information:

    R1 ←──── OSPF ────→ R2

R1 dynamically learned:

    192.168.2.0/24

R2 dynamically learned:

    192.168.1.0/24

This demonstrates one of the main advantages of dynamic routing protocols: routes can be learned and updated automatically instead of being manually configured on every router.

---

# 🔎 Verification Commands Used

### Check router interfaces

    show ip interface brief

Used to verify interface IP addresses and operational status.

### Check OSPF neighbors

    show ip ospf neighbor

Used to verify the OSPF neighbor relationship and adjacency state.

### Check routing table

    show ip route

Used to verify OSPF-learned routes.

### Check OSPF process

    show ip ospf

Used to verify the OSPF routing process and Area 0.

### Check routing protocols

    show ip protocols

Used to verify OSPF configuration and advertised networks.

### Test connectivity

    ping

Used to test connectivity between routers and end devices.

---

# 📸 Screenshots

## 1. Network Topology

<img width="2868" height="1102" alt="topology" src="https://github.com/user-attachments/assets/5a9969c5-b2c5-459e-9036-4e3810589874" />

---

## 2. R1 — OSPF Neighbor Verification

<img width="2878" height="1376" alt="r1-ospf-neighbor" src="https://github.com/user-attachments/assets/ce1100f7-6b91-43ff-8a03-6d3443207f1b" />

---

## 3. R2 — OSPF Neighbor Verification

<img width="2874" height="1340" alt="r2-ospf-neighbor" src="https://github.com/user-attachments/assets/3dedbe0d-e61b-4f4f-a72f-7f44c1ae26f1" />

---

## 4. R1 — OSPF Process Verification

<img width="2872" height="1426" alt="r1-show-ip-ospf" src="https://github.com/user-attachments/assets/33148987-4848-4244-ab84-9875e860c2e5" />

---

## 5. Connectivity Test

<img width="2878" height="1406" alt="pc0-to-pc2-ping" src="https://github.com/user-attachments/assets/a5ee6b22-4a01-4758-b0d5-b93d3c98b5a9" />

---

# 📚 What I Learned

Through this lab, I learned and practiced:

- Dynamic routing
- OSPF configuration
- OSPF process IDs
- OSPF Area 0
- OSPF neighbor relationships
- OSPF adjacency states
- Router-to-router communication
- IPv4 addressing
- Wildcard masks
- Routing tables
- Dynamic route learning
- OSPF route identification
- End-to-end packet forwarding
- Cisco IOS configuration
- Cisco IOS verification commands
- Network troubleshooting
- Comparing static and dynamic routing
- Using Cisco Packet Tracer for network simulation

---

# 🛠️ Troubleshooting

During the lab, OSPF neighbor formation was monitored using:

    show ip ospf neighbor

The routers successfully reached the:

    FULL

neighbor state.

The router-to-router connectivity was also tested using:

    ping 10.0.0.2

and:

    ping 10.0.0.1

Both tests were successful.

After OSPF routes were learned, end-to-end connectivity was tested between the two LANs.

The final connectivity tests were successful with:

    0% packet loss

This confirmed that OSPF was correctly exchanging routing information and forwarding traffic between the two networks.

---

# ✅ Final Result

The OSPF dynamic routing lab was successfully completed.

Two separate LANs were connected through two routers using the `10.0.0.0/30` point-to-point network.

OSPF process 1 was configured on both routers using Area 0.

The routers successfully established an OSPF neighbor relationship and dynamically learned the remote LAN networks.

End-to-end communication between the two LANs was successfully verified.

Unlike Lab 4, where routes had to be manually configured, OSPF allowed the routers to dynamically exchange routing information.

This lab provided practical experience with dynamic routing and demonstrated an important step toward understanding enterprise network infrastructure.

---

# 📁 Project Structure

    05-ospf-dynamic-routing/
    │
    ├── README.md
    ├── ospf-lab.pkt
    ├── topology.png
    ├── r1-ospf-neighbor.png
    ├── r2-ospf-neighbor.png
    ├── r1-ospf-route.png
    ├── r2-ospf-route.png
    ├── pc0-to-pc2-ping.png
    └── pc2-to-pc0-ping.png
