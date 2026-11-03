# Day 78 - Reconsolidation: Re-reading Days 41-45 (Second Pass)
**Date:** 2026-11-03
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Second pass through the GitHub notes for Days 41-45, taking two days over the five-day block to make the concepts stick.**

**Reconsolidated:**
* **Days 41-44 (consolidation blocks):** subnetting, TCP handshake, DHCP and DORA, NAT and PAT, firewalls, VLANs and STP, WAN options, RIP and OSPF, BGP, IGP versus EGP, AAA, and SNMP.
* **Day 45 (troubleshooting method):** the seven CompTIA steps, with escalation as the branch at step 5, and the three approaches (top-down, bottom-up, divide-and-conquer).

**A pattern I noticed in my own study:**
* For the past week, each five-day block has taken about two days of re-reading.
* This is deliberate, not avoidance. Each pass fixes a different layer of understanding: the first pass shows what's there, and the second makes it hold.
* The notes support this. Every block so far has needed a second look before the concepts stayed put.

**Still to firm up:**
* Naming the seven troubleshooting steps in order without a pause.
* Saying which approach fits which situation in one line.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ping -c 2 127.0.0.1         # Day 46 baseline: own network stack
netstat -nr | grep default  # Day 46 baseline: gateway
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** the two-pass pace is working. The second pass fixes concepts that the first pass only showed.
* **Blocker:** the seven steps and the three approaches still blur, since they answer different questions. Steps are what to do, approaches are where to start.