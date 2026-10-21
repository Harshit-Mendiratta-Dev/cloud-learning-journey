# Day 66 - Review: Re-reading Days 11-15
**Date:** 2026-10-21
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 11-15, line by line.**

**Revisited:**
* DHCP and the DORA exchange (Discover, Offer, Request, Acknowledge), and why DHCP is Layer 7 over UDP.
* DHCP lease timers and the difference between DNS TTL and IP packet TTL.
* CNAME as an alias, not a load balancer.
* TCP flags and the connection handshake.
* NAT and PAT: the difference between translating addresses and translating ports.
* The Day 15 toolkit: the commands used across Days 5-14 and what each one tests.

**Still to firm up:**
* Writing out the full forms of NAT and PAT without a pause.
* Keeping DNS TTL and IP packet TTL separate in your head.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ipconfig getpacket en1    # the DHCP lease details from Day 12
lsof -i -n -P             # which apps use which ports, from Day 14
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 11-15 cover the address-assignment and translation mechanics that the troubleshooting scenarios depend on.
* **Blocker:** the NAT/PAT naming slip has come up before, so it's the item to drill.