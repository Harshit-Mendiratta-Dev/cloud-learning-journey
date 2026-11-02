# Day 77 - Review: Re-reading Days 41-45
**Date:** 2026-11-02
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 41-45, line by line.**

**Revisited:**
* **Days 41-44 (the four consolidation blocks):**
  * Days 1-10: subnetting, TCP handshake, MAC and router mechanics, TTL, DNS hierarchy, A versus CNAME records.
  * Days 11-20: DORA and DHCP, TCP flags, NAT and PAT, `lsof`, Nmap scan types, WHOIS.
  * Days 21-30: firewalls, VLANs and trunking, STP, WAN and MPLS, the subnetting arc.
  * Days 31-40: WAN options, RIP and OSPF, AS and BGP, IGP versus EGP, 802.1X, MFA, SNMP and syslog.
* **Day 45 (troubleshooting method):** the seven CompTIA steps, from identifying the problem through to documenting it, with escalation as the branch at step 5.
* **Approaches:** top-down (start at the application), bottom-up (start at the physical layer), and divide-and-conquer (start at Layer 3 and move up or down based on the result).

**Still to firm up:**
* Naming the seven troubleshooting steps in order without a pause.
* Matching each approach to the situation it suits, in one line.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ping -c 2 127.0.0.1         # Day 46 baseline: own network stack
netstat -nr | grep default  # Day 46 baseline: gateway
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 41-45 cover the consolidation of the first 40 days and the start of the troubleshooting method. The method is what the scenarios from Days 48-62 are built on.
* **Blocker:** the seven steps and the three approaches are easy to mix up, since they're separate ideas (steps are what to do, approaches are where to start).