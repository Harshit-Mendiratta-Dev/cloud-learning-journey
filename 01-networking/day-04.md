# Day 04 - Nmap Port Scanning, TCP Flags & Routing Boundaries
**Date:** 2026-08-19
**Focus Area:** Phase 1 - Networking
**Time Spent:** 3.5 Hours

## 1. Key Concepts Learned (1.5h Theory)

* **NAT vs. PAT (Network Address Translation):**
  * **Static NAT:** 1-to-1 mapping between a private IP and a public IP.
  * **Dynamic NAT:** Maps private IPs to a pool of available public IPs.
  * **PAT (Port Address Translation / NAT Overload):** Maps multiple private IPs to a single public IP by appending unique TCP/UDP **source port numbers** to track sessions. (Essential foundation for Cloud NAT Gateways).
* 
* **RFC Rules & Stealth Scans:** Internet RFCs (Request For Comments) (e.g., RFC 793) define TCP standard behavior. Stealth scans (`-sN` NULL, `-sF` FIN, `-sX` Xmas) abuse these rules by sending illegal flag combinations. Closed ports respond with `RST` per RFC specifications, while open ports stay silent (`Open|Filtered`).
* 
* **Firewall Evasion & NSE Scripts:** Firewalls inspect traffic headers; evasion methods include packet fragmentation, decoys, and timing delays. The Nmap Scripting Engine (NSE) automates vulnerability testing on exposed ports.
* 
* **RFC 1918 Private IP Routing:** Addresses in the `10.0.0.0/8` range are non-routable on the public internet. Scanning a remote cloud lab VM from a local Mac terminal without an active VPN tunnel drops packets.

## 2. Hands-on Lab & Commands (2.0h Lab)
```bash
# Installed Nmap on macOS via Homebrew package manager
brew install nmap

# TCP Connect Scan (Uses standard system calls; no root required)
nmap -sT 10.48.153.133

# Custom Xmas Scan (Manipulates raw TCP flags; requires root/sudo)
sudo nmap -sX --top-port 999 10.48.153.133

# Skipping host ping discovery (-Pn) when firewalls block ICMP
sudo nmap -sX -Pn --top-port 999 10.48.153.133

# Locating Nmap NSE scripts on macOS (Apple Silicon Homebrew path)
ls /opt/homebrew/share/nmap/scripts/ | grep smb

# Scanning official public test server (Validates public routing)
nmap -F scanme.nmap.org
```

## 3. Key Takeaway / Blocker Solved

   Privilege Escalation Solved: Discovered -sN, -sF, and -sX scans quit on macOS without sudo because non-root users are restricted from crafting raw Layer 3/4 packet headers.

   Routing Boundary Solved: Solved host timeout errors on 10.x.x.x targets by recognizing that private IPs require an active OpenVPN connection to route traffic outside local Wi-Fi.

   Path Resolution: Identified macOS Homebrew script path (/opt/homebrew/share/nmap/scripts/) replacing standard Linux paths (/usr/share/nmap/scripts/).