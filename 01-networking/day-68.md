# Day 68 - Review: Re-reading Days 21-25
**Date:** 2026-10-23
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 21-25, line by line.**

**Revisited:**
* Firewalls: software and network firewalls, and access control lists (ACLs).
* Stateful and stateless filtering, and why a stateful firewall can allow replies to connections it has seen.
* The macOS Application Firewall (`socketfilterfw`), its default state, and how to read its ACL.
* Wi-Fi standards and naming, and the 2.4, 5, and 6 GHz bands and their tradeoffs.
* Channels and interference: overlapping channels on 2.4 GHz, and what neighbouring networks do to your link.
* WPA, WPA2, and WPA3 security, and what changed between them.
* Reading signal, noise, and SNR from the Mac with `system_profiler SPAirPortDataType`.

**Still to firm up:**
* The difference between stateful and stateless in one sentence.
* Choosing a channel from the neighbouring networks you can see.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
sudo socketfilterfw --getglobalstate        # firewall on or off
sudo socketfilterfw --listapps              # apps in the firewall list
system_profiler SPAirPortDataType | grep -E "Channel|Signal|Noise"   # link quality, from Day 25
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 21-25 cover the two layers that sit between the network and the device: filtering (firewalls) and the wireless link. Both show up in the Day 59 and Day 61 scenarios.
* **Blocker:** the stateful/stateless distinction is the one to say cleanly, since it's easy to blur.
* **Next:** Day 69, the re-read of Days 26-30 (VLANs, trunking, STP).