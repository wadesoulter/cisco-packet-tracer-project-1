## Project Objective

Design and simulate a secure Wide Area Network (WAN) between two branch offices — the Ghana Office and the Cape Verde Office — using Cisco Packet Tracer. The goal is to ensure connectivity for essential services (HTTP and ICMP), while restricting all other traffic using proper IP addressing and Access Control Lists (ACLs).

---

## Network Topology Overview

* **Routers**: Ghana Router and Cape Verde Router
* **Switches**: One for each office
* **PCs**: Four in each office
* **Server**: Located in the Ghana Office
* **Connections**:

  * Routers connected via Serial (S0/0/0)
  * Routers connected to switches via GigabitEthernet0/0
  * PCs connected to switches

---

## IP Addressing Scheme

| Device/Interface           | IP Address     | Subnet Mask   | Gateway      |
| -------------------------- | -------------- | ------------- | ------------ |
| Ghana Router (G0/0)        | 192.168.10.1   | 255.255.255.0 | -            |
| Ghana Router (S0/0/0)      | 192.168.1.2    | 255.255.255.0 | -            |
| Cape Verde Router (G0/0)   | 192.168.20.1   | 255.255.255.0 | -            |
| Cape Verde Router (S0/0/0) | 192.168.1.1    | 255.255.255.0 | -            |
| Ghana PCs                  | 192.168.10.x   | 255.255.255.0 | 192.168.10.1 |
| Cape Verde PCs             | 192.168.20.x   | 255.255.255.0 | 192.168.20.1 |
| Server                     | 192.168.10.254 | 255.255.255.0 | 192.168.10.1 |

---

## Router Configuration Summary

### Basic Configuration Commands

```
enable
configure terminal
hostname GhanaRouter
interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
interface Serial0/0/0
 ip address 192.168.1.2 255.255.255.0
 clock rate 64000
 no shutdown
exit
ip route 192.168.20.0 255.255.255.0 192.168.1.1
```

Cape Verde Router:

```
hostname CapeVerdeRouter
interface GigabitEthernet0/0
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit
interface Serial0/0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
exit
ip route 192.168.10.0 255.255.255.0 192.168.1.2
```

---

## Access Control Lists (ACLs)

Permit only ICMP and HTTP traffic; deny all others.

```
access-list 100 permit icmp any any
access-list 100 permit tcp any any eq 80
access-list 100 deny ip any any

interface Serial0/0/0
 ip access-group 100 in
```

Repeat the ACL setup on both routers’ serial interfaces.

---

## Server Setup

* Place the server in the Ghana Office
* Assign IP: 192.168.10.254/24
* Set default gateway: 192.168.10.1
* Enable HTTP service under the Services tab

---

## Verification Steps

1. **Ping Test**: PCs from both offices should successfully ping the server and each other.
2. **Browser Test**: Cape Verde PCs should access the server’s webpage via HTTP.
3. **Blocked Services**: Any other service (e.g., FTP, Telnet) should be blocked.

---

## Final Notes

* Ensure all interfaces are up and not administratively down.
* Use proper cable types (crossover for router-router, straight-through for PC-switch/router-switch).
* Save configurations using:

```
copy running-config startup-config
```

---

## Attachments (for full submission)

* Screenshots:

  * IP configurations of all PCs and routers
  * Successful ping and browser tests
* `.pkt` File: Packet Tracer project file

---

**End of Documentation**
