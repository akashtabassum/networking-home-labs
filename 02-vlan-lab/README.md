# 🧪 Lab 2 — VLAN Configuration and Network Segmentation

## 📌 Objective

The objective of this lab was to configure and test **Virtual Local Area Networks (VLANs)** using Cisco Packet Tracer.

The lab demonstrates how a single Layer 2 switch can logically divide a network into multiple isolated broadcast domains.

Three VLANs were created:

* **VLAN 10 — ADMIN**
* **VLAN 20 — USERS**
* **VLAN 30 — SERVERS**

The lab also verifies communication within the same VLAN and isolation between different VLANs.

---

## 🛠️ Tools Used

* Cisco Packet Tracer
* Cisco 2960 Switch
* 6 × PCs
* Copper Straight-Through Ethernet cables

---

## 🏗️ Network Topology

The topology consists of one Cisco 2960 switch and six PCs.

Each VLAN contains two PCs.

```text
                         ┌─────────────────┐
                         │      SW1        │
                         │  Cisco 2960     │
                         └─────────────────┘
                           │ │ │ │ │ │
              ┌────────────┘ │ │ │ │ └────────────┐
              │              │ │ │ │              │
             PC1            PC2 PC3 PC4           PC5 PC6
```

### VLAN Structure

```text
VLAN 10 — ADMIN
    ├── PC1
    └── PC2

VLAN 20 — USERS
    ├── PC3
    └── PC4

VLAN 30 — SERVERS
    ├── PC5
    └── PC6
```

<img width="2876" height="1086" alt="topology" src="https://github.com/user-attachments/assets/a96f091a-b674-4b39-a307-abebc87826cb" />


---

# 1. 🔌 Connecting the Devices

A Cisco 2960 switch and six PCs were added to the Packet Tracer workspace.

Each PC was connected to the switch using a **Copper Straight-Through** cable.

| Device | Switch Port | VLAN |
| ------ | ----------- | ---: |
| PC1    | Fa0/1       |   10 |
| PC2    | Fa0/2       |   10 |
| PC3    | Fa0/3       |   20 |
| PC4    | Fa0/4       |   20 |
| PC5    | Fa0/5       |   30 |
| PC6    | Fa0/6       |   30 |

---

# 2. 🏷️ Creating the VLANs

The switch CLI was opened and privileged configuration mode was entered.

```text
enable
configure terminal
```

### VLAN 10 — ADMIN

```text
vlan 10
name ADMIN
exit
```

### VLAN 20 — USERS

```text
vlan 20
name USERS
exit
```

### VLAN 30 — SERVERS

```text
vlan 30
name SERVERS
exit
```

---

# 3. 🔧 Assigning Switch Ports to VLANs

The connected switch ports were configured as **access ports** and assigned to their respective VLANs.

### VLAN 10 — PC1 and PC2

```text
interface range fastethernet 0/1 - 2
switchport mode access
switchport access vlan 10
exit
```

### VLAN 20 — PC3 and PC4

```text
interface range fastethernet 0/3 - 4
switchport mode access
switchport access vlan 20
exit
```

### VLAN 30 — PC5 and PC6

```text
interface range fastethernet 0/5 - 6
switchport mode access
switchport access vlan 30
exit
```

---

# 4. 🌐 IP Address Configuration

Static IP addresses were assigned to each PC.

| Device | VLAN | IP Address    | Subnet Mask   |
| ------ | ---: | ------------- | ------------- |
| PC1    |   10 | 192.168.10.10 | 255.255.255.0 |
| PC2    |   10 | 192.168.10.11 | 255.255.255.0 |
| PC3    |   20 | 192.168.20.10 | 255.255.255.0 |
| PC4    |   20 | 192.168.20.11 | 255.255.255.0 |
| PC5    |   30 | 192.168.30.10 | 255.255.255.0 |
| PC6    |   30 | 192.168.30.11 | 255.255.255.0 |

No default gateway was configured because **inter-VLAN routing was not part of this lab**.

---

# 5. 🔍 VLAN Verification

The VLAN configuration was verified using:

```text
show vlan brief
```

The resulting configuration confirmed that the correct switch ports were assigned to each VLAN.

```text
10   ADMIN       active    Fa0/1, Fa0/2
20   USERS       active    Fa0/3, Fa0/4
30   SERVERS     active    Fa0/5, Fa0/6
```

<img width="2872" height="1418" alt="vlan-configuration" src="https://github.com/user-attachments/assets/2bbc0d31-1fb1-4350-ada3-e8d4ed074236" />


---

# 6. 🧪 Connectivity Testing

Connectivity was tested using the `ping` command.

## Test 1 — VLAN 10

PC1 was used to ping PC2:

```text
ping 192.168.10.11
```

### Result

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

✅ **Successful**

PC1 and PC2 can communicate because they belong to the same VLAN.

<img width="2880" height="1430" alt="vlan10-ping" src="https://github.com/user-attachments/assets/3fd50314-39b9-4c69-92b4-9ca2945a732a" />


---

## Test 2 — VLAN 20

PC3 was used to ping PC4:

```text
ping 192.168.20.11
```

### Result

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

✅ **Successful**

PC3 and PC4 can communicate because they belong to the same VLAN.

<img width="2874" height="1434" alt="vlan20-ping" src="https://github.com/user-attachments/assets/7be7858f-8ab8-49ea-a85a-11f739704e79" />


---

## Test 3 — VLAN 30

PC5 was used to ping PC6:

```text
ping 192.168.30.11
```

### Result

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

✅ **Successful**

PC5 and PC6 can communicate because they belong to the same VLAN.

<img width="2874" height="1410" alt="vlan30-ping" src="https://github.com/user-attachments/assets/92648066-d622-4c28-9351-4b374ee01ecb" />


---

# 7. 🚫 Inter-VLAN Communication Test

To verify VLAN isolation, PC1 attempted to communicate with PC3.

From PC1:

```text
ping 192.168.20.10
```

### Result

```text
Request timed out.

Packets: Sent = 2, Received = 0, Lost = 2 (100% loss)
```

❌ **Communication failed as expected.**

PC1 belongs to VLAN 10, while PC3 belongs to VLAN 20.

Because no Layer 3 device or inter-VLAN routing has been configured, the switch cannot route traffic between these VLANs.

<img width="2874" height="1428" alt="inter-vlan-failed" src="https://github.com/user-attachments/assets/e384c4ab-a8dc-4107-bdc8-5313b7e8c1d4" />


---

# 8. 📊 Final Configuration

| VLAN | Name    | Ports       | Network         |
| ---: | ------- | ----------- | --------------- |
|   10 | ADMIN   | Fa0/1–Fa0/2 | 192.168.10.0/24 |
|   20 | USERS   | Fa0/3–Fa0/4 | 192.168.20.0/24 |
|   30 | SERVERS | Fa0/5–Fa0/6 | 192.168.30.0/24 |

---

# 9. ✅ Results

The lab successfully demonstrated:

* VLAN creation and configuration
* VLAN naming
* Access port configuration
* Assigning switch ports to VLANs
* Static IP addressing
* Same-VLAN connectivity
* VLAN segmentation
* Inter-VLAN isolation
* VLAN verification using Cisco IOS commands

All three VLANs successfully communicated internally, while communication between different VLANs was blocked.

---

# 10. 🧠 What I Learned

Through this lab, I learned how VLANs can be used to logically divide a physical network into separate networks.

I also learned how to:

* Create VLANs on a Cisco switch
* Configure access ports
* Assign devices to VLANs
* Configure IP addresses for different VLAN networks
* Verify VLAN configuration using `show vlan brief`
* Test network connectivity using `ping`
* Understand why inter-VLAN communication requires Layer 3 routing

---

## 📁 Project Files

```text
02-vlan-lab/
│
├── README.md
├── vlan-lab.pkt
├── topology.png
├── vlan-configuration.png
├── vlan10-ping.png
├── vlan20-ping.png
├── vlan30-ping.png
└── inter-vlan-failed.png
```
