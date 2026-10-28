# Day 73 - Review (Second Pass): Re-reading Days 31-35
**Date:** 2026-10-28
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Second pass through the GitHub notes for Days 31-35, tightening the concepts.**

**Tightened:**
* **WAN choice by need:**
  * Leased line: a dedicated, private circuit. Reliable and predictable, but expensive.
  * MPLS: a carrier's private backbone that forwards by label rather than by full address lookup. Good for connecting multiple sites with service guarantees.
  * SD-WAN: software picks the best path across ordinary internet links. Cheaper, and it adapts to link quality.
* **MPLS and VLAN tags:** both add a label at the entry point and remove it at the exit. MPLS does this across a carrier's network, and VLAN trunking does it on a switch link.
* **Cabling:** copper (twisted pair) versus fiber (light, longer distance, no electrical interference). Cat5e through Cat8 are copper categories with rising speed and shielding. RJ45 is the copper connector. LC and SC are fiber connectors.
* **Straight-through versus crossover:** a straight-through cable connects unlike devices (for example, a PC to a switch). A crossover connects like devices (for example, switch to switch). Modern ports auto-detect, so this matters less in practice.
* **Cat6 label:** 24 AWG is the wire gauge, 4 pairs is the number of twisted pairs, and ANSI/TIA-568 is the standard it's built to.

**Still to firm up:**
* Matching a need to a WAN choice in one sentence (for example, "cheap, multi-site, adapts to link quality" points to SD-WAN).
* Which cable type connects like devices and which connects unlike devices, without a pause.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ifconfig en1 | grep -E "media|status"    # link type and status for the physical layer
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** WAN and cabling sit at the physical and link layers, where bottom-up troubleshooting starts. Knowing what each option costs and where it's used makes the layer question easier to answer.
* **Blocker:** crossover versus straight-through is the detail that still blurs, so this pass added the "like versus unlike devices" rule.
* **Next:** Day 74, the re-read of Days 36-40 (ASN and BGP, AAA, monitoring).