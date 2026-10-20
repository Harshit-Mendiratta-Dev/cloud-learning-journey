# Day 65 - Review: Re-reading Days 1-10
**Date:** 2026-10-20
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 1-10, line by line.**

**Revisited:**
* The OSI layers and the tools that test them.
* Ports and protocols, including HTTPS on TCP 443.
* Subnet arithmetic: a `/24` has 254 usable hosts.
* MAC addresses in a frame: rewritten at each hop, while the destination IP stays the same.
* CNAME: a name-to-name alias that resolves to an IP at the end of the chain.

**Still to firm up:**
* The `/24` host count, worked without a pause.
* Following a CNAME chain to the final A record.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
dig www.example.com     # look for a CNAME line, then the A record after it
ipcalc 192.168.10.0/24  # check host count and range (if installed)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** the Days 1-10 foundations hold up on re-reading. The two open items are arithmetic and chain-following, so they're drills, not new topics.
* **Blocker:** brain fog after the quiz. Re-reading at a slower pace helped.