# Day 06 - Network Troubleshooting & Ports
**Date:** 2026-08-21
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2.5 Hours

## 1. Key Concepts Learned (1.5h Theory)

* **TCP Connection Lifecycle:** Connection-oriented TCP sessions rely on the 3-Way Handshake (`SYN`, `SYN-ACK`, `ACK`) to establish reliability, and the 4-Way Teardown (`FIN`, `ACK`) to close cleanly.

* **Zero-Payload Packet Traces:** `tcpdump` captures showing `length 0` indicate connection management traffic (handshakes, teardowns, probes/port checks) rather than actual application data payload transfer.

* **Professor Messer Core Ports Reference:**

| Port(s) | Protocol | What it's for |
|---|---|---|
| 20 / 21 | FTP | File transfer (data / control) |
| 22 | SSH | Secure remote shell |
| 23 | Telnet | Unencrypted remote shell (legacy) |
| 25 | SMTP | Sending email |
| 53 | DNS | Domain name resolution |
| 67 / 68 | DHCP | Dynamic IP address assignment (server / client) |
| 80 | HTTP | Unencrypted web traffic |
| 110 | POP3 | Retrieving email (downloads & removes from server) |
| 137–139 | NetBIOS | Legacy Windows name resolution/file sharing |
| 143 | IMAP | Retrieving email (syncs, keeps on server) |
| 161 / 162 | SNMP | Network device monitoring (queries / traps) |
| 389 | LDAP | Directory services lookup (e.g. Active Directory) |
| 443 | HTTPS | Encrypted web traffic |
| 445 | SMB | Windows file/printer sharing |
| 3389 | RDP | Windows remote desktop |

## 2. Hands-on Lab & Commands (1.0h Lab)
```bash
# View the manual for netstat on macOS (since BSD netstat doesn't support -h)
man netstat

# Check active TCP connections and listening ports in numeric format
netstat -an | grep tcp

# Capture live traffic on the Wi-Fi interface (en1) for specific host and port
sudo tcpdump -n -i en1 host 1.1.1.1 and tcp port 80
```

## 3. Key Takeaway / Blocker Solved
Blocker Solved: macOS runs a BSD version of `netstat` which doesn't support the `-h` or `--help` flags. Resolved by using `man netstat` to check the manual. Also identified that `en0` is typically the physical Ethernet port on a Mac mini, whereas `en1` is the active Wi-Fi adapter used for capturing wireless traffic.