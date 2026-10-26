# Day 71 - Review: Re-reading Days 26-30
**Date:** 2026-10-26
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 26-30, line by line.**

**Revisited:**
* VLANs: separating one switch into separate broadcast domains.
* Access ports (one VLAN) versus trunk ports (many VLANs), and why a trunk carries tags.
* 802.1Q tagging and untagging, and where the tag is added and removed.
* Why a switch floods unknown destinations, and how broadcasts cross VLANs only through a router.
* Spanning Tree Protocol: broadcast storms, root bridge election by lowest Bridge ID (priority, then MAC), and blocked ports as failover.
* Day 30: a light day, with a re-read of Days 1-10 notes.

**Still to firm up:**
* Stating the difference between an access port and a trunk port in one sentence.
* Keeping the Bridge ID components straight: priority first, then MAC.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ifconfig en1                  # interface and addressing, for comparing with VLAN config
netstat -nr | grep default    # routing table, for seeing which subnet a host sits on
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 26-30 cover how a switch keeps traffic apart (VLANs) and how it avoids loops (STP). Both come up in the Day 38 segmentation work and the Day 52 access reasoning.
* **Blocker:** the Bridge ID order (priority, then MAC) is the detail that's easy to blur.
* **Next:** Day 72, the re-read of Days 31-35 (WAN, dynamic routing, cabling).