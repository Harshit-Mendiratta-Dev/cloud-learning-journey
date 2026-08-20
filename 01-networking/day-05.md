# Day 05 - IP Math, CIDR Notation & Logical Subnets
**Date:** 2026-08-20
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2.0 Hours

## 1. Key Concepts Learned (1.5h Theory)

* **IPv4 Structure:** An IP address is 32 bits long, divided into 4 octets (8 bits each).
* 
* **CIDR Notation (Classless Inter-Domain Routing)(The Slash):** The slash (e.g., `/8`, `/16`, `/24`) is simply a shortcut representing how many binary `1`s exist in the subnet mask from left to right. 
  * Example: `/24` = 24 `1`s (`11111111.11111111.11111111.00000000`) = `255.255.255.0`.
* 
* **Network Boundaries:**
  * **Network Address:** The very first IP in the range (e.g., `172.16.0.0`) representing the network itself.
  * **Broadcast Address:** The very last IP in the range (e.g., `172.16.255.255`) used to message all hosts.
  * **Default Gateway:** The "exit door" to the internet (the local router). By standard convention, it is usually assigned the first usable host IP (e.g., `172.16.0.1`).
* 
* **Binary to Decimal Conversion:** Mastered how to convert 8-bit binary octets into decimal format (using the `128 | 64 | 32 | 16 | 8 | 4 | 2 | 1` positional values) to understand how IP addresses and subnet masks are actually calculated by the computer.
* 
* **Subnets vs. Switches:** A subnet is a **logical, mathematical boundary** (like a Zip Code), not a piece of physical hardware. Switches are the physical hardware (like paved roads) connecting devices inside that logical boundary.

## 2. Hands-on Lab & Commands (0.5h Lab)
```bash
# Display local network interfaces, IP addresses, and subnet masks on macOS
ifconfig en0

# (Example output context to look for):
# inet 192.168.1.50 netmask 0xffffff00 broadcast 192.168.1.255
# Note: 0xffffff00 in hexadecimal equals 255.255.255.0 or /24 in CIDR.
```

## 3. Key Takeaway / Blocker Solved
Conceptual Blocker Solved: Cleared the misconception that a subnet is a physical switch sitting between hosts and a router.

CIDR Clarity: Understood that binary subnet masks simply draw the line between the "Network ID" bits and the "Host ID" bits.