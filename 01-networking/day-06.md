# Day 06 - Network Troubleshooting & Ports
**Date:** 2026-08-21
**Focus Area:** Phase 1 - Networking
**Time Spent:** 3.5 Hours

## 1. Key Concepts Learned (1.5h Theory)
* **TCP Connection Lifecycle:** Explored how connection-oriented TCP sessions rely on the 3-Way Handshake ```(SYN, SYN-ACK, ACK)``` to establish reliability and the 4-Way Teardown ```(FIN, ACK)``` to close cleanly.
* 
* **Zero-Payload Packet Traces:** Learned that ```tcpdump``` captures showing ```length 0``` indicate connection management traffic (like probes or port checks) rather than actual application data payload transfer.
* 
*  **Professor Messer Core Ports Reference:** Covered standard exam ports including FTP (20/21), SSH (22), Telnet (23), SMTP (25), DNS (53), DHCP (67/68), HTTP (80), POP3 (110), NetBIOS (137-139), IMAP (143), SNMP (161/162), LDAP (389), HTTPS (443), SMB (445), and RDP (3389).
*  
## 2. Hands-on Lab & Commands (2.0h Lab)
```bash
# View the manual for netstat on macOS (since BSD netstat doesn't support -h)
man netstat

# Check active TCP connections and listening ports in numeric format
netstat -an | grep tcp

# Capture live traffic on the Wi-Fi interface (en1) for specific host and port
sudo tcpdump -n -i en1 host 1.1.1.1 and tcp port 80
```

## 3. Key Takeaway / Blocker Solved
Blocker Solved: macOS runs a BSD version of ```netstat``` which doesn't support the ```-h``` or ```--help``` flags. Resolved by using ```man netstat``` to check the manual. Also identified that ```en0``` is typically the physical Ethernet port on a Mac mini, whereas ```en1``` is the active Wi-Fi adapter used for capturing wireless traffic.