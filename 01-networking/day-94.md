# Day 94 - Review: Port Reference Table and OSI Layer Synthesis (Days 6-7)
**Date:** 2026-11-22
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Revisited the port reference table from Day 6 and the OSI synthesis from Day 7.**

**Common ports and their transport protocols:**

| Port | Protocol | Transport | Used for |
|---|---|---|---|
| 20, 21 | FTP | TCP | File transfer (data, control) |
| 22 | SSH | TCP | Secure remote login |
| 23 | Telnet | TCP | Unencrypted remote login |
| 25 | SMTP | TCP | Sending email |
| 53 | DNS | UDP (and TCP for large replies) | Name resolution |
| 67, 68 | DHCP | UDP | Address assignment |
| 80 | HTTP | TCP | Web traffic |
| 110, 143 | POP3, IMAP | TCP | Reading email |
| 123 | NTP | UDP | Time sync |
| 443 | HTTPS | TCP | Encrypted web traffic |

**Ports and transport:**
* TCP is connection-oriented and reliable, so most application traffic uses it.
* UDP is connectionless and lighter, used where speed matters more than guaranteed delivery (DNS queries, DHCP, NTP).
* A port number identifies the service on a host. The IP gets the packet to the host, and the port gets it to the right application.

**The OSI layers (Day 7 synthesis):**
* **Layer 7 (Application):** the protocols apps use: HTTP, DNS, DHCP.
* **Layer 6 (Presentation):** data format and encryption, such as TLS.
* **Layer 5 (Session):** managing sessions between applications.
* **Layer 4 (Transport):** TCP and UDP, ports, reliability.
* **Layer 3 (Network):** IP addresses, routing, ICMP.
* **Layer 2 (Data link):** MAC addresses, frames, ARP, switches.
* **Layer 1 (Physical):** signals on cable or radio.

**Tying the layers to the tools from Days 5 to 9:**
* `ifconfig` and `ipconfig` show Layer 3 addressing.
* `arp` shows Layer 2 MAC mappings.
* `netstat` shows Layer 4 connections and ports.
* `curl` and `dig` work at Layer 7.

**Still to firm up:**
* Naming the transport protocol and port for HTTPS, DNS, and DHCP without a pause.
* Explaining why DNS uses UDP but can fall back to TCP.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
netstat -an | grep LISTEN       # the ports this Mac is listening on (Day 6)
netstat -an | grep tcp | grep 80   # TCP connections involving port 80 (Day 6)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** the port table tells you which service is which. The OSI layers tell you which tool tests which part. Together they make it possible to pick the right tool for a port or a layer.
* **Blocker:** DNS sits on both UDP and TCP, depending on the reply size, so the port table alone doesn't show the full picture.