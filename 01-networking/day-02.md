# Day 2 - OSI Model, Transport Protocols & Terminal Diagnostics
**Date:** 2026-08-17
**Focus Area:** OSI 7-Layer Stack, TCP/UDP Mechanics, Common Ports, and Layer 4/7 CLI Diagnostics
**Time Spent:** 3.5 Hours

## 1. Key Concepts Learned (1.5h Theory)
* ### The OSI Model & PDUs
Data flows down the stack during **Encapsulation** (adding headers) and up the stack during **Decapsulation** (stripping headers).

| Layer | Layer Name | Protocol Data Unit (PDU) | Core Function / Identifiers |
| :--- | :--- | :--- | :--- |
| **7** | Application | Data | User-facing protocols (HTTP, SSH, DNS, Telnet) |
| **6** | Presentation | Data | Syntax, encryption, data formatting |
| **5** | Session | Data | Manages session state between applications |
| **4** | Transport | **Segment** (TCP) / **Datagram** (UDP) | Reliability, port numbers, flow control |
| **3** | Network | **Packet** | Logical addressing (**IP Addresses**), routing |
| **2** | Data Link | **Frame** | Physical addressing (**MAC Addresses**), switching |
| **1** | Physical | **Bits** | Electrical signals, optical pulses, cables |

---

### Layer 4: TCP vs. UDP
* **TCP (Transmission Control Protocol):** Connection-oriented and reliable. Guarantees ordered delivery using sequencing and error checking.
  * **TCP 3-Way Handshake:** `SYN` -> `SYN-ACK` -> `ACK`
* **UDP (User Datagram Protocol):** Connectionless and stateless. Low latency with no handshake or delivery guarantees (used for DNS, streaming, VoIP).

---

### Key Network Ports
* **Port 22:** SSH (Secure Shell)
* **Port 23:** Telnet (Unencrypted remote CLI access)
* **Port 53:** DNS (Domain Name System - UDP/TCP)
* **Port 80:** HTTP (Web traffic - Unencrypted)
* **Port 443:** HTTPS (Web traffic - Encrypted via TLS/SSL)

---


## 2. Hands-on Lab & Commands (2.0h Lab)
### Lab 1: Telnet Protocol Mechanics (TryHackMe)
* **Issue Observed:** Running `telnet <IP>` without a port explicitly declared defaults to **Port 23** (Ubuntu login prompt) and times out without OS credentials.
* **Resolution:** Declared explicit port targeting to interact with web services:
  ```bash
  telnet 10.48.154.187 80
  GET / HTTP/1.1
  [Hit Enter Twice]
  ```

### Lab 2: Local Mac Terminal Diagnostics

* **DNS Lookup:** `dig google.com +short`
* **HTTP/TLS Handshake:** `curl -vI https://google.com`
* **Listening Sockets:** `lsof -i -P -n | grep LISTEN`

## 3. Key Takeaway / Blocker Solved

**Port Explicit Connection:** Omitting port numbers causes utilities like telnet to assume default protocol handlers (Port 23). Specifying 80 forces standard HTTP socket interaction.

**Troubleshooting Scaffolding:** `dig` tests Layer 7-to-3 resolution, `curl -v` inspects Layer 4/7 handshake state, and `lsof` inspects internal host socket status.