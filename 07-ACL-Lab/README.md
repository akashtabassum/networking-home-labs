# 🔐 Lab 7 — Access Control List (ACL) Configuration

## 📌 Overview

This is the seventh networking home lab in my Cisco Packet Tracer networking series.

In this lab, I configured an **Extended Access Control List (ACL)** to control traffic between two different LAN networks.

The main objective was to allow normal communication between the networks while specifically blocking **PC2 (192.168.1.11)** from accessing **LAN 2 (192.168.2.0/24)**.

The lab also demonstrates how ACL rules can be verified using Cisco IOS commands and connectivity tests.

---

## 🎯 Objectives

* Build a network containing two separate LANs
* Configure IPv4 addressing
* Configure router interfaces
* Configure inter-LAN connectivity
* Create an Extended ACL
* Block traffic from a specific host
* Allow other network traffic
* Apply an ACL to a router interface
* Verify ACL configuration
* Test allowed traffic
* Test blocked traffic
* Use Cisco IOS verification commands
* Understand basic network traffic filtering

---

## 🛠️ Tools Used

* Cisco Packet Tracer
* Cisco 2911 Router × 1
* Cisco 2960 Switch × 2
* PCs × 4
* Copper Straight-Through cables

---

# 🏗️ Network Topology

The network consists of two separate LANs connected through one router.

```
PC1                 PC3
 |                   |
PC2                 PC4
 \                   /
  SW1             SW2
   |               |
   |               |
  G0/0           G0/1
     \           /
      \         /
       Router
```

The router provides communication between:

```
LAN 1 → 192.168.1.0/24
```

and:

```
LAN 2 → 192.168.2.0/24
```

The ACL is applied to traffic entering the router from LAN 1.

---

# 🌐 Network Design

The network was divided into two IP networks.

### LAN 1

```
Network: 192.168.1.0/24
Gateway: 192.168.1.1
```

### LAN 2

```
Network: 192.168.2.0/24
Gateway: 192.168.2.1
```

The ACL specifically blocks:

```
PC2: 192.168.1.11
```

from accessing:

```
LAN 2: 192.168.2.0/24
```

---

# 🔌 Physical Connections

The following physical connections were used.

| From   | Port          | To     | Port          | Cable                   |
| ------ | ------------- | ------ | ------------- | ----------------------- |
| PC1    | FastEthernet0 | SW1    | Fa0/1         | Copper Straight-Through |
| PC2    | FastEthernet0 | SW1    | Fa0/2         | Copper Straight-Through |
| SW1    | Fa0/24        | Router | Gig0/0        | Copper Straight-Through |
| Router | Gig0/1        | SW2    | Fa0/24        | Copper Straight-Through |
| SW2    | Fa0/1         | PC3    | FastEthernet0 | Copper Straight-Through |
| SW2    | Fa0/2         | PC4    | FastEthernet0 | Copper Straight-Through |

---

# 💻 IP Addressing

The PCs were manually configured with static IPv4 addresses.

### PC1

```
IP Address:       192.168.1.10
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.1.1
```

### PC2

```
IP Address:       192.168.1.11
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.1.1
```

### PC3

```
IP Address:       192.168.2.10
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.2.1
```

### PC4

```
IP Address:       192.168.2.11
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.2.1
```

---

# 📋 IP Addressing Table

| Device | Interface     | IP Address   | Subnet Mask   | Default Gateway |
| ------ | ------------- | ------------ | ------------- | --------------- |
| Router | G0/0          | 192.168.1.1  | 255.255.255.0 | —               |
| PC1    | FastEthernet0 | 192.168.1.10 | 255.255.255.0 | 192.168.1.1     |
| PC2    | FastEthernet0 | 192.168.1.11 | 255.255.255.0 | 192.168.1.1     |
| Router | G0/1          | 192.168.2.1  | 255.255.255.0 | —               |
| PC3    | FastEthernet0 | 192.168.2.10 | 255.255.255.0 | 192.168.2.1     |
| PC4    | FastEthernet0 | 192.168.2.11 | 255.255.255.0 | 192.168.2.1     |

---

# ⚙️ Router Interface Configuration

The router was configured with two GigabitEthernet interfaces.

### GigabitEthernet0/0

```
IP Address: 192.168.1.1
Subnet Mask: 255.255.255.0
```

### GigabitEthernet0/1

```
IP Address: 192.168.2.1
Subnet Mask: 255.255.255.0
```

The following commands were used:

```
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
exit

end
```

The router interfaces were verified using:

```
show ip interface brief
```

The interfaces showed:

```
GigabitEthernet0/0    192.168.1.1    up    up
GigabitEthernet0/1    192.168.2.1    up    up
```

---

# 🔄 Inter-LAN Connectivity

Before applying the ACL, communication between the two LANs was verified.

The router provided routing between:

```
192.168.1.0/24
```

and:

```
192.168.2.0/24
```

PC1 was able to communicate with PC3.

PC3 was also able to communicate with PC1.

This confirmed that the basic network connectivity was working before applying traffic restrictions.

---

# 🔒 Extended ACL Configuration

An Extended ACL was configured to block PC2 from accessing LAN 2.

The ACL uses:

```
ACL Number: 100
```

The following configuration was used:

```
enable
configure terminal

access-list 100 deny ip host 192.168.1.11 192.168.2.0 0.0.0.255
access-list 100 permit ip any any

interface gigabitEthernet 0/0
ip access-group 100 in

end
```

---

# 📜 ACL Rules

The ACL contains two rules.

### Rule 10 — Deny PC2

```
deny ip host 192.168.1.11 192.168.2.0 0.0.0.255
```

This rule blocks traffic from:

```
192.168.1.11
```

to the entire:

```
192.168.2.0/24
```

network.

### Rule 20 — Permit Other Traffic

```
permit ip any any
```

This rule allows other IP traffic that is not blocked by the first rule.

---

# 📍 ACL Application

The ACL was applied inbound on:

```
GigabitEthernet0/0
```

using:

```
ip access-group 100 in
```

This means traffic entering the router from LAN 1 is checked against ACL 100 before being forwarded.

---

# 🧪 Connectivity Testing

After configuring the ACL, connectivity tests were performed to verify both allowed and blocked traffic.

---

# ✅ Test 1 — PC1 to PC3

From PC1:

```
ping 192.168.2.10
```

The ping was successful.

The result showed:

```
Packets: Sent = 4
Received = 4
Lost = 0 (0% loss)
```

This confirmed that PC1 was allowed to communicate with PC3.

---

# ❌ Test 2 — PC2 to PC3

From PC2:

```
ping 192.168.2.10
```

The ping was blocked.

The result showed:

```
Packets: Sent = 4
Received = 0
Lost = 4 (100% loss)
```

The router returned:

```
Reply from 192.168.1.1: Destination host unreachable.
```

This confirmed that the ACL successfully blocked PC2 from accessing LAN 2.

---

# 🔄 Test 3 — PC3 to PC1

From PC3:

```
ping 192.168.1.10
```

The ping was successful.

The result showed:

```
Packets: Sent = 4
Received = 4
Lost = 0 (0% loss)
```

This confirmed that PC3 could communicate with PC1.

---

# 🔎 ACL Verification

The configured ACL was verified using:

```
show access-lists
```

The output showed:

```
Extended IP access list 100
    10 deny ip host 192.168.1.11 192.168.2.0 0.0.0.255 (4 match(es))
    20 permit ip any any (16 match(es))
```

The match counter on the deny rule confirmed that traffic from PC2 matched the ACL rule.

---

# 📊 Test Results

| Source             | Destination        | Result    |
| ------------------ | ------------------ | --------- |
| PC1 (192.168.1.10) | PC3 (192.168.2.10) | ✅ Allowed |
| PC2 (192.168.1.11) | PC3 (192.168.2.10) | ❌ Blocked |
| PC3 (192.168.2.10) | PC1 (192.168.1.10) | ✅ Allowed |
| ACL deny rule      | PC2 → LAN 2        | ✅ Matched |

---

# 🧠 Understanding the ACL

The ACL evaluates traffic based on its configured rules.

When PC2 sends traffic toward LAN 2:

```
PC2
192.168.1.11
   ↓
SW1
   ↓
Router G0/0
   ↓
ACL 100
   ↓
DENY
   ↓
LAN 2
```

The traffic is denied because the source IP address matches:

```
192.168.1.11
```

and the destination belongs to:

```
192.168.2.0/24
```

When PC1 sends traffic toward LAN 2, its source address does not match the deny rule, so the traffic is permitted by:

```
permit ip any any
```

---

# 🛡️ Network Security Concept

Access Control Lists are an important network security mechanism used to control traffic entering or leaving network interfaces.

In this lab, the ACL was used to implement a simple security policy:

```
PC2 → LAN 2 = BLOCKED
```

while:

```
PC1 → LAN 2 = ALLOWED
```

This demonstrates how ACLs can be used to restrict access to specific network resources.

---

# 🔍 Verification Commands Used

### Check Router Interfaces

```
show ip interface brief
```

Used to verify interface IP addresses and operational status.

### Check ACL Configuration

```
show access-lists
```

Used to display the configured ACL rules and match counters.

### Test Network Connectivity

```
ping
```

Used to verify allowed and blocked communication between devices.

---

# 📸 Screenshots

## 1. Network Topology

<img width="2860" height="1040" alt="topology" src="https://github.com/user-attachments/assets/74d6572b-443b-4d04-b6e1-8d58ca7ac3b7" />

---

## 2. IP Configuration

<img width="2878" height="1418" alt="ip-configuration" src="https://github.com/user-attachments/assets/44492be6-6c0f-4619-bc29-e2af3b80e265" />

---

## 3. ACL Configuration

<img width="2878" height="1418" alt="ip-configuration" src="https://github.com/user-attachments/assets/f07d1002-de35-47b5-aa35-1d3957195a3b" />

---

## 4. Allowed Traffic

<img width="2872" height="1412" alt="allowed-traffic" src="https://github.com/user-attachments/assets/64af0d64-b7b9-4c5c-b965-c8b64a1daec9" />

---

## 5. Blocked Traffic
<img width="2878" height="1412" alt="blocked-traffic" src="https://github.com/user-attachments/assets/12b29daf-5614-4381-bffe-f82d6b16e82b" />

---

## 6. ACL Verification

<img width="2878" height="1408" alt="acl-verification" src="https://github.com/user-attachments/assets/0e1b7478-ae8c-4afb-9e39-55e25676ceba" />

---

# 📚 What I Learned

Through this lab, I learned and practiced:

* Access Control Lists
* Extended ACLs
* IP address filtering
* Source and destination matching
* Traffic filtering
* Router interface configuration
* Inter-LAN communication
* ACL application to interfaces
* Inbound ACL processing
* ACL verification
* Network security fundamentals
* Cisco IOS configuration
* Cisco IOS verification commands
* ICMP/Ping testing
* Basic network access control

---

# 🏆 Final Result

The ACL configuration lab was successfully completed.

An Extended ACL was configured on the router to block **PC2 (192.168.1.11)** from accessing **LAN 2 (192.168.2.0/24)**.

PC1 was successfully able to communicate with PC3, while PC2 was blocked from accessing PC3.

The ACL configuration was verified using `show access-lists`, and the match counter confirmed that the deny rule was applied to traffic from PC2.

This lab provided practical experience with Cisco ACL configuration and demonstrated how network traffic can be controlled using source and destination IP addresses.

---

# 📁 Project Structure

```
07-ACL-Lab/
│
├── ACL-Lab.pkt
├── README.md
├── topology.png
├── ip-configuration.png
├── acl-configuration.png
├── allowed-traffic.png
├── blocked-traffic.png
└── acl-verification.png
```

---
