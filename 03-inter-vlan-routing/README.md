# 🧪 Lab 3 — Inter-VLAN Routing Using Router-on-a-Stick

## 📌 Objective

The objective of this lab was to configure **Inter-VLAN Routing** using the **Router-on-a-Stick** method in Cisco Packet Tracer.

In Lab 2, three VLANs were created and successfully isolated from one another. In this lab, a Cisco router was introduced to enable communication between these VLANs.

The lab demonstrates:

* Inter-VLAN routing
* Router-on-a-Stick
* 802.1Q trunking
* Router subinterfaces
* Default gateways
* VLAN-to-VLAN connectivity testing

---

## 🛠️ Tools Used

* Cisco Packet Tracer
* Cisco 2960 Switch
* Cisco 2911 Router
* 6 × PCs
* Copper Straight-Through Ethernet cables

---

# 🏗️ Network Topology

The network consists of:

* One Cisco 2960 switch (`SW1`)
* One Cisco 2911 router (`R1`)
* Six PCs
* Three VLANs
* One trunk link between the switch and router

```text
                         R1
                  Cisco 2911 Router
                         |
                    G0/0 | Fa0/24
                         |
                       TRUNK
                         |
                        SW1
                   Cisco 2960
             ┌───────────┼───────────┐
             │           │           │
          VLAN 10     VLAN 20     VLAN 30
           ADMIN       USERS      SERVERS
             │           │           │
           PC1/PC2     PC3/PC4     PC5/PC6
```
<img width="2868" height="1022" alt="topology" src="https://github.com/user-attachments/assets/cb70c171-1552-4a6b-811b-f01b5c3f0256" />

---

# 1. 🔌 Physical Connections

The router was connected to the switch using a Copper Straight-Through cable.

| Device | Interface          | Connected To |
| ------ | ------------------ | ------------ |
| R1     | GigabitEthernet0/0 | SW1 Fa0/24   |
| SW1    | FastEthernet0/24   | R1 Gig0/0    |

The remaining switch ports were connected to the PCs:

| PC  | Switch Port | VLAN |
| --- | ----------- | ---: |
| PC1 | Fa0/1       |   10 |
| PC2 | Fa0/2       |   10 |
| PC3 | Fa0/3       |   20 |
| PC4 | Fa0/4       |   20 |
| PC5 | Fa0/5       |   30 |
| PC6 | Fa0/6       |   30 |

---

# 2. 🏷️ VLAN Configuration

The VLANs created in Lab 2 were used in this lab.

| VLAN ID | Name    | Network         |
| ------: | ------- | --------------- |
|      10 | ADMIN   | 192.168.10.0/24 |
|      20 | USERS   | 192.168.20.0/24 |
|      30 | SERVERS | 192.168.30.0/24 |

The switch configuration was verified using:

```text
show vlan brief
```
<img width="2878" height="1422" alt="vlan-configuration" src="https://github.com/user-attachments/assets/fe231632-9df3-496d-8b4f-b78745aebf7d" />

---

# 3. 🔗 Configuring the Trunk

The connection between SW1 and R1 needs to carry traffic from all three VLANs.

The switch port `Fa0/24` was configured as a trunk.

```text
enable
configure terminal
interface fastethernet 0/24
switchport mode trunk
exit
end
```

The trunk was verified using:

```text
show interfaces trunk
```

The final result showed:

```text
Fa0/24   on   802.1q   trunking
```

The active VLANs allowed across the trunk were:

```text
1,10,20,30
```

<img width="2870" height="1358" alt="trunk-configuration" src="https://github.com/user-attachments/assets/c0f7d955-f9b5-4f99-82d6-6531de326243" />


---

# 4. 🌐 Enabling the Router Interface

The router interface connected to the switch was enabled using:

```text
enable
configure terminal
interface gigabitEthernet 0/0
no shutdown
exit
end
```

The interface was verified using:

```text
show ip interface brief
```

The physical interface showed:

```text
GigabitEthernet0/0    unassigned    up    up
```
<img width="2878" height="1358" alt="router-subinterfaces" src="https://github.com/user-attachments/assets/2fa170be-bf45-4317-a505-4dada56cacc2" />

---

# 5. 🔀 Router-on-a-Stick Configuration

Router-on-a-Stick allows a single physical router interface to handle traffic for multiple VLANs using logical **subinterfaces**.

Three subinterfaces were created.

## VLAN 10

```text
configure terminal
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit
```

## VLAN 20

```text
interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit
```

## VLAN 30

```text
interface gigabitEthernet 0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit
```

The configuration was verified using:

```text
show ip interface brief
```

The final configuration showed:

```text
GigabitEthernet0/0.10    192.168.10.1    up    up
GigabitEthernet0/0.20    192.168.20.1    up    up
GigabitEthernet0/0.30    192.168.30.1    up    up
```
---
# 6. 🚪 Default Gateway Configuration

Each PC was configured with the router subinterface corresponding to its VLAN as its default gateway.

| VLAN | Network         | Default Gateway |
| ---: | --------------- | --------------- |
|   10 | 192.168.10.0/24 | 192.168.10.1    |
|   20 | 192.168.20.0/24 | 192.168.20.1    |
|   30 | 192.168.30.0/24 | 192.168.30.1    |

### Complete IP Addressing Table

| Device | VLAN | IP Address    | Subnet Mask   | Gateway      |
| ------ | ---: | ------------- | ------------- | ------------ |
| PC1    |   10 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC2    |   10 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC3    |   20 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| PC4    |   20 | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |
| PC5    |   30 | 192.168.30.10 | 255.255.255.0 | 192.168.30.1 |
| PC6    |   30 | 192.168.30.11 | 255.255.255.0 | 192.168.30.1 |

---

# 7. 🧪 Connectivity Testing

## Test 1 — PC1 → VLAN 10 Gateway

Command:

```text
ping 192.168.10.1
```

Result:

```text
Packets: Sent = 4
Received = 4
Lost = 0 (0% loss)
```

✅ **Successful**

This confirms that PC1 can reach its default gateway.

---

## Test 2 — VLAN 10 → VLAN 20

From PC1:

```text
ping 192.168.20.10
```

Result:

```text
Packets: Sent = 4
Received = 4
Lost = 0 (0% loss)
```

✅ **Successful**

This confirms that the router is successfully routing traffic between VLAN 10 and VLAN 20.

<img width="2876" height="1392" alt="inter-vlan-ping" src="https://github.com/user-attachments/assets/1e5e4786-8425-49e2-92c8-f233fa9324a7" />


---

## Test 3 — VLAN 10 → VLAN 30

From PC1:

```text
ping 192.168.30.10
```

Result:

```text
Packets: Sent = 4
Received = 4
Lost = 0 (0% loss)
```

✅ **Successful**

This confirms communication between VLAN 10 and VLAN 30.
<img width="2862" height="1422" alt="vlan10-to-vlan30" src="https://github.com/user-attachments/assets/79f839c0-66f1-4385-b841-f179f728a36a" />

---

## Test 4 — VLAN 20 → VLAN 30

From PC3:

```text
ping 192.168.30.10
```

Result:

```text
Packets: Sent = 4
Received = 4
Lost = 0 (0% loss)
```

✅ **Successful**

This confirms communication between VLAN 20 and VLAN 30.

<img width="2874" height="1414" alt="vlan20-to-vlan30" src="https://github.com/user-attachments/assets/c9bfd5d4-5b18-4e5d-b256-ede8c8efd5dd" />


---

# 8. 📊 Final Test Results

| Source        | Destination            | Result    |
| ------------- | ---------------------- | --------- |
| PC1 — VLAN 10 | Gateway `192.168.10.1` | ✅ 0% loss |
| PC1 — VLAN 10 | PC3 — VLAN 20          | ✅ 0% loss |
| PC1 — VLAN 10 | PC5 — VLAN 30          | ✅ 0% loss |
| PC3 — VLAN 20 | PC5 — VLAN 30          | ✅ 0% loss |

All required inter-VLAN connectivity tests were successful.

---

# 9. 🔄 How Router-on-a-Stick Works

When PC1 sends traffic to PC3:

```text
PC1
192.168.10.10
     │
     ▼
SW1
VLAN 10
     │
     ▼
Fa0/24 TRUNK
     │
     ▼
R1 G0/0.10
192.168.10.1
     │
     │ Routing
     ▼
R1 G0/0.20
192.168.20.1
     │
     ▼
TRUNK
     │
     ▼
SW1
VLAN 20
     │
     ▼
PC3
192.168.20.10
```

The router receives the packet through the VLAN 10 subinterface, routes it to the VLAN 20 network, and forwards it through the VLAN 20 subinterface.

---

# 10. 🔍 Verification Commands

The following Cisco IOS commands were used during the lab:

### View VLANs

```text
show vlan brief
```

### View trunk interfaces

```text
show interfaces trunk
```

### View interface status and IP addresses

```text
show ip interface brief
```

### Test connectivity

```text
ping <destination-ip>
```

---

# 11. 🧠 What I Learned

This lab helped me understand how different VLANs can communicate using a router.

I learned:

* How Inter-VLAN Routing works
* How Router-on-a-Stick works
* How to configure router subinterfaces
* How 802.1Q VLAN tagging is used on trunk links
* How to configure default gateways
* How routers route traffic between different IP networks
* How to verify trunk and interface status
* How to troubleshoot physical and logical connectivity issues
* How to test inter-VLAN communication using `ping`

---

# 12. 🛠️ Troubleshooting

During the lab, the switch trunk initially showed:

```text
Operational Mode: down
Status: notconnect
```

The switch configuration itself was correct, but the router interface was not operational.

The router interface was enabled using:

```text
interface gigabitEthernet 0/0
no shutdown
```

After enabling the interface, the switch port changed to:

```text
Status: connected
VLAN: trunk
```

The trunk then successfully carried VLANs 10, 20, and 30.

This troubleshooting process demonstrated the importance of checking both **administrative configuration and operational interface status**.

---

# 13. ✅ Conclusion

The Inter-VLAN Routing lab was successfully completed using **Router-on-a-Stick**.

Three VLANs were configured on the switch, a trunk link was established between the switch and router, and three router subinterfaces were configured as default gateways.

Connectivity testing confirmed successful communication between VLAN 10, VLAN 20, and VLAN 30 with **0% packet loss**.

This lab builds on the VLAN segmentation concepts from Lab 2 and provides the foundation for more advanced networking concepts such as **static routing, OSPF, ACLs, NAT, and network security**.

---

## 📁 Project Structure

```text
03-inter-vlan-routing/
│
├── README.md
├── inter-vlan-routing.pkt
├── topology.png
├── vlan-configuration.png
├── trunk-configuration.png
├── router-subinterfaces.png
├── inter-vlan-ping.png
├── vlan10-to-vlan30.png
└── vlan20-to-vlan30.png
```

