# Day 01 - What is Networking
**Date:** 2026-08-16
**Focus Area:** Phase 1 - Networking
**Time Spent:** 3.5 Hours

## 1. Key Concepts Learned (1.5h Theory)
* **Concept 1:** ... IP vs. MAC Address: An IP address is your location on a network (like a mailing address/name), while a MAC address is a permanent physical identifier burned into your network card (like a fingerprint).

IPv4 & Octets: IPv4 uses 4 sets/sections of numbers (octets) separated by dots, ranging from 0.0.0.0 to 255.255.255.255. IPv6 exists because the world ran out of IPv4 addresses.

Public vs. Private IP: Private IPs (e.g., 192.168.x.x, 10.x.x.x) are used inside your local house/office network. Public IPs are assigned by your ISP and are visible to the global internet.

ICMP & Ping: ping is a command-line tool that uses the ICMP protocol to send an "Echo Request" packet to a server to see if it responds and check latency


* **Concept 2:** ... Topologies: Star (all devices connect to a central switch—most common), Bus (one main cable), Ring (token passed in a circle).
 
 Network vs. Host IP: Network ID identifies the subnet (the street); Host ID identifies the specific machine (the house number)
  
 .ARP (Address Resolution Protocol): Translates known IP addresses into physical MAC addresses using ARP Requests (broadcast) and ARP Replies (unicast), saved in the ARP cache.
  
 DHCP (Dynamic Host Configuration Protocol): Automatically assigns IPs using DORA (Discover > Offer > Request > Acknowledge).

## 2. Hands-on Lab & Commands (2.0h Lab)
```bash
# Tested connectivity and latency via ICMP
ping -c 4 8.8.8.8

# Viewed local network interface details (Mac/Linux)
ifconfig

# Checked local ARP table cache for IP-to-MAC mappings
arp -a
```

## 3. Key Takeaway / Blocker Solved
Key Takeaway: Understanding the street vs. house number analogy made the distinction between Network IP and Host IP click immediately.

Blocker Solved: Overcame initial terminal navigation friction by organizing the workspace into a clean 50/50 split between TryHackMe and VS Code.