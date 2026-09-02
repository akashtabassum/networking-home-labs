# DHCP & DNS Configuration with Inter-LAN Connectivity

## 📌 Lab Overview

This lab demonstrates the configuration and verification of DHCP and DNS services in Cisco Packet Tracer.

The network consists of two LANs connected through a router. DHCP automatically assigns IP configuration to clients on LAN2, while DNS resolves a hostname to the server's IP address.

The lab also verifies communication between LAN1 and LAN2.

---

## 🎯 Objectives

- Configure DHCP for LAN2.
- Automatically assign IP addresses to LAN2 clients.
- Configure the default gateway and DNS server through DHCP.
- Configure DNS services.
- Create a DNS hostname record.
- Verify DHCP address assignment.
- Verify client IP configuration.
- Test gateway connectivity.
- Test server connectivity.
- Verify DNS name resolution.
- Test communication between LAN1 and LAN2.

---

# 🗺️ Network Topology

The network contains:

- 1 Router
- 2 Switches
- LAN1 PCs
- LAN2 PCs
- 1 DHCP/DNS Server

### Network Structure

                 ┌───────────────┐
                 │    Router     │
                 │               │
                 │ LAN1: .1.1    │
                 │ LAN2: .2.1    │
                 └───────┬───────┘
                         │
             ┌───────────┴───────────┐
             │                       │
        ┌────▼─────┐            ┌────▼─────┐
        │  Switch  │            │  Switch  │
        │   LAN1   │            │   LAN2   │
        └────┬─────┘            └────┬─────┘
             │                       │
        ┌────┴────┐             ┌────┴────┐
        │ LAN1 PCs│             │ LAN2 PCs│
        └─────────┘             └────┬────┘
                                     │
                               ┌─────▼─────┐
                               │  Server   │
                               │ DHCP/DNS  │
                               │   .2.2    │
                               └───────────┘

### Topology Screenshot
<img width="2598" height="836" alt="topology" src="https://github.com/user-attachments/assets/beaba35d-3bdd-4e03-9080-c25d5e8efd4b" />


---

# 🌐 IP ADDRESSING

| Device / Network | IP Address | Subnet Mask | Default Gateway | DNS |
|---|---|---|---|---|
| LAN1 Network | 192.168.1.0/24 | 255.255.255.0 | 192.168.1.1 | — |
| LAN1 Router Gateway | 192.168.1.1 | 255.255.255.0 | — | — |
| LAN1 PC | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 | — |
| LAN2 Network | 192.168.2.0/24 | 255.255.255.0 | 192.168.2.1 | — |
| LAN2 Router Gateway | 192.168.2.1 | 255.255.255.0 | — | — |
| DHCP/DNS Server | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 | — |
| LAN2 PC3 | 192.168.2.10 | 255.255.255.0 | 192.168.2.1 | 192.168.2.2 |

---

# ⚙️ DHCP CONFIGURATION

DHCP was configured on the server to automatically provide network configuration to LAN2 clients.

### DHCP Pool Configuration

    Pool Name:       LAN2-DHCP
    Default Gateway: 192.168.2.1
    DNS Server:      192.168.2.2
    Start IP:        192.168.2.10
    Subnet Mask:     255.255.255.0
    Maximum Users:   50

### DHCP Configuration Screenshot

<img width="2870" height="1290" alt="dhcp-configuration" src="https://github.com/user-attachments/assets/f5386f03-36a3-45a5-981a-fcf231623e01" />

---

# 💻 DHCP CLIENT CONFIGURATION

PC3 was configured as a DHCP client so that it could automatically obtain its network configuration from the server.

### PC3 Obtained Configuration

    IPv4 Address:    192.168.2.10
    Subnet Mask:     255.255.255.0
    Default Gateway: 192.168.2.1
    DNS Server:      192.168.2.2

---

# 🌍 DNS CONFIGURATION

DNS service was enabled on the server.

A DNS record was created to map the hostname `server.local` to the server's IP address.

### DNS Record

    Hostname:     server.local
    IP Address:   192.168.2.2

This allows clients to use:

    server.local

instead of directly using:

    192.168.2.2

### DNS Configuration Screenshot

<img width="2874" height="1394" alt="dns-configuration" src="https://github.com/user-attachments/assets/f81e7f47-a607-4bb2-8ec9-1e0846a3311a" />


---

# 🧪 CONNECTIVITY TESTING

After configuring DHCP and DNS, connectivity tests were performed to verify the complete network.

---

## 1. PC3 → LAN2 Gateway

Command:

    ping 192.168.2.1

Result:

    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

✅ PC3 successfully communicates with the LAN2 gateway.

![PC3 Gateway Test](images/pc3-gateway-ping.png)

---

## 2. PC3 → DHCP/DNS Server

Command:

    ping 192.168.2.2

Result:

    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

✅ PC3 successfully communicates with the DHCP/DNS server.


---

# 🔎 3. DNS NAME RESOLUTION TEST

DNS functionality was tested using the hostname:

    ping server.local

The hostname successfully resolved to:

    192.168.2.2

Result:

    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

✅ DNS name resolution is working correctly.

---

# 🔀 4. LAN2 → LAN1 GATEWAY

Inter-LAN routing was tested from PC3 by pinging the LAN1 gateway.

Command:

    ping 192.168.1.1

Result:

    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

✅ LAN2 successfully reaches the LAN1 gateway.


---

# 🔄 5. LAN2 → LAN1 PC

PC3 was used to test communication with the LAN1 PC.

Command:

    ping 192.168.1.10

Result:

    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

✅ Communication from LAN2 to LAN1 PC is successful.


---

# 🔁 6. LAN1 → LAN2 PC

The reverse connectivity test was performed from the LAN1 PC.

Command:

    ping 192.168.2.10

Result:

    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

✅ Communication from LAN1 to LAN2 PC is successful.


---

# 📊 VERIFICATION SUMMARY

| Test | Result |
|---|---|
| DHCP address assignment | ✅ Successful |
| PC3 received 192.168.2.10 | ✅ Successful |
| PC3 → LAN2 Gateway | ✅ Successful |
| PC3 → DHCP/DNS Server | ✅ Successful |
| DNS server.local resolution | ✅ Successful |
| LAN2 → LAN1 Gateway | ✅ Successful |
| LAN2 → LAN1 PC | ✅ Successful |
| LAN1 → LAN2 PC | ✅ Successful |

---

# 📚 CONCEPTS PRACTICED

- DHCP
- DNS
- IPv4 Addressing
- Subnet Masks
- Default Gateways
- DHCP Client Configuration
- DNS Hostname Resolution
- Router-Based Inter-Network Communication
- LAN-to-LAN Connectivity
- ICMP
- Ping Testing
- Network Verification

---

# 🏁 CONCLUSION

The DHCP and DNS network was successfully configured and verified in Cisco Packet Tracer.

LAN2 clients successfully received their IP configuration automatically through DHCP.

The DHCP server provided:

- IP Address
- Subnet Mask
- Default Gateway
- DNS Server

The DNS service successfully resolved `server.local` to `192.168.2.2`.

Connectivity testing confirmed successful communication between:

- PC3 and its gateway
- PC3 and the DHCP/DNS server
- LAN2 and the LAN1 gateway
- LAN2 and the LAN1 PC
- LAN1 and the LAN2 PC

Therefore, DHCP, DNS, and inter-LAN connectivity were successfully implemented and verified.

---

# 🛠️ TOOLS USED

- Cisco Packet Tracer
- Cisco Router
- Cisco Switch
- PCs / End Devices
- Server
- DHCP
- DNS
- ICMP / Ping

---

# 📂 PROJECT STRUCTURE

06-DHCP-DNS-Lab/
│
├── README.md
├── DHCP-DNS-Lab.pkt
├── topology.png
├── dhcp-configuration.png
├── dns-configuration.png
├── pc3-dhcp.png
├── pc3-connectivity.png
├── server-ping-test.png


---

