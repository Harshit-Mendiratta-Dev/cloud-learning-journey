# Day 72 - Review: Re-reading Days 31-35
**Date:** 2026-10-27
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 31-35, line by line.**

**Revisited:**
* WAN technologies: leased lines (dedicated, reliable, expensive), MPLS (label-based forwarding across a carrier's backbone), and SD-WAN (software-based routing over ordinary internet links).
* MPLS labels and VLAN trunking tags: the same add-at-entry, strip-at-exit idea, at different scales.
* Cabling: copper versus fiber, cable categories (Cat5e to Cat8), and connectors (RJ45, LC, SC).
* Straight-through versus crossover cables, and when each is used.
* Reading a Cat6 cable's printed label: wire gauge (24 AWG), number of pairs (4), and the standard it meets (ANSI/TIA-568).

**Still to firm up:**
* Naming the WAN technology that fits a given need (leased line, MPLS, or SD-WAN) in one sentence.
* Straight-through versus crossover, without a pause.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ifconfig en1 | grep -E "media|status"    # link type and status, for the physical layer
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 31-35 cover how sites connect over distance (WAN) and what the physical link is made of (cabling). Both sit at the bottom of the OSI model, where bottom-up troubleshooting starts.
* **Blocker:** straight-through versus crossover is the detail that's easy to blur.
* **Next:** Day 73, the re-read of Days 36-40 (BGP, AAA, monitoring).