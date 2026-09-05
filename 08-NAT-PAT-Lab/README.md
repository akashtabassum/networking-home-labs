# Lab 8 — NAT/PAT Configuration and Verification

## 📌 Objective

Configure and verify **NAT/PAT (Port Address Translation)** on a Cisco router to allow multiple private LAN devices to access an external network using a single translated IP address.

---

## 🖧 Network Topology

<img width="2864" height="1010" alt="topology" src="https://github.com/user-attachments/assets/f2264f4d-e92a-46c5-b842-0e8dc741ec96" />


### Devices Used

| Device            | Quantity |
| ----------------- | -------: |
| Cisco 2911 Router |        2 |
| Cisco 2960 Switch |        1 |
| PCs               |        3 |
| Server            |        1 |

### Topology

```
PC0 ─┐
PC1 ─┼── Switch ─── R1 ─── R2 ─── Server
PC2 ─┘
```

---



## 🌐 IP Addressing Scheme

| Device | Interface | IP Address   | Subnet Mask     | Gateway     |
| ------ | --------- | ------------ | --------------- | ----------- |
| PC0    | Fa0       | 192.168.1.10 | 255.255.255.0   | 192.168.1.1 |
| PC1    | Fa0       | 192.168.1.11 | 255.255.255.0   | 192.168.1.1 |
| PC2    | Fa0       | 192.168.1.12 | 255.255.255.0   | 192.168.1.1 |
| R1     | Gi0/0     | 192.168.1.1  | 255.255.255.0   | —           |
| R1     | Gi0/1     | 10.0.0.1     | 255.255.255.252 | —           |
| R2     | Gi0/0     | 10.0.0.2     | 255.255.255.252 | —           |
| R2     | Gi0/1     | 203.0.113.1  | 255.255.255.0   | —           |
| Server | Fa0       | 203.0.113.10 | 255.255.255.0   | 203.0.113.1 |

---

## ⚙️ R1 Interface Configuration

<img width="2874" height="1364" alt="r1-interface-configuration" src="https://github.com/user-attachments/assets/a9a8defd-4651-4974-8fb5-b86ee1e07628" />

```
enable
configure terminal
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
interface gigabitEthernet 0/1
ip address 10.0.0.1 255.255.255.252
no shutdown
exit
end
```

---

## ⚙️ R2 Interface Configuration

<img width="2878" height="1352" alt="r2-interface-configuration" src="https://github.com/user-attachments/assets/a0593aa2-7248-47e4-b48d-b45b99d23caf" />

```
enable
configure terminal
interface gigabitEthernet 0/0
ip address 10.0.0.2 255.255.255.252
no shutdown
exit
interface gigabitEthernet 0/1
ip address 203.0.113.1 255.255.255.0
no shutdown
exit
end
```

---

## 🛣️ Static Routing

### R1 Default Route

```
configure terminal
ip route 0.0.0.0 0.0.0.0 10.0.0.2
end
```

### R2 Return Route

```
configure terminal
ip route 192.168.1.0 255.255.255.0 10.0.0.1
end
```

These routes allow traffic to travel between the internal LAN and the external server network.

---

## 🔄 NAT/PAT Configuration

NAT was configured on R1 with:

* `Gi0/0` as the **NAT inside** interface
* `Gi0/1` as the **NAT outside** interface
* ACL 1 identifying the internal private network
* PAT overload allowing multiple devices to share the same outside interface address

### R1 NAT/PAT Configuration

```
configure terminal
interface gigabitEthernet 0/0
ip nat inside
exit
interface gigabitEthernet 0/1
ip nat outside
exit
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface gigabitEthernet 0/1 overload
end
```

---

## 🧪 Connectivity Testing

<img width="2866" height="1408" alt="successful-ping-test" src="https://github.com/user-attachments/assets/71d2dc38-45dc-4750-bf5a-82f9663e6b67" />

Connectivity was tested from the internal PCs to the external server:

```
ping 203.0.113.10
```

The ping tests from **PC0** and **PC1** were successful with **0% packet loss**.

---

## 🔍 NAT Translation Verification

<img width="2876" height="1388" alt="nat-translations" src="https://github.com/user-attachments/assets/7756de65-005b-4b89-a61f-0f2d584cba41" />

The NAT table was verified on R1 using:

```
show ip nat translations
```

The translations demonstrated that private addresses such as:

* `192.168.1.10`
* `192.168.1.11`

were translated to the R1 outside interface address:

* `10.0.0.1`

This confirms that **PAT overload** allows multiple internal hosts to use the same translated address.

---

## 📊 NAT Statistics

NAT operation was also verified using:

```
show ip nat statistics
```

The output confirmed:

* NAT inside interface: `GigabitEthernet0/0`
* NAT outside interface: `GigabitEthernet0/1`
* NAT hits recorded for translated traffic

---

## 💾 Configuration Backup

The router configuration was saved using:

```
copy running-config startup-config
```

---

## ✅ Verification Summary

| Test                | Result       |
| ------------------- | ------------ |
| PC0 → R1            | ✅ Successful |
| PC0 → R2            | ✅ Successful |
| PC0 → Server        | ✅ Successful |
| PC1 → Server        | ✅ Successful |
| PAT translations    | ✅ Verified   |
| NAT statistics      | ✅ Verified   |
| Configuration saved | ✅ Completed  |

---

## 🎯 Key Concepts Learned

* NAT inside and outside interfaces
* Static routing
* Access Control Lists for NAT
* PAT / NAT overload
* Private-to-translated IP addressing
* NAT translation table verification
* NAT statistics
* Connectivity testing using `ping`
* Saving Cisco router configurations
---

## 📁 Project Structure

```
Lab-8-NAT-PAT/
│
├── README.md
│
├── topology.png
├── NAT-PAT.pkt
├── r1-interface-configuration.png
├── r2-interface-configuration.png
├── successful-ping-test.png
└── nat-translations.png
```
