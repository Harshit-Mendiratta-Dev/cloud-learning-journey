# Day 69 - Review (Second Pass): Re-reading Days 21-25
**Date:** 2026-10-24
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Second pass through the GitHub notes for Days 21-25, after slip-ups on Day 68.**

**Slip-ups from Day 68 and what I configured to fix them:**
* [Fill in the specific slips and what you changed, in your own words]

**Revisited:**
* Firewalls: software and network firewalls, and access control lists (ACLs).
* Stateful versus stateless filtering, and why a stateful firewall allows replies to connections it has already seen.
* The macOS Application Firewall (`socketfilterfw`), its default state, and how to read its ACL.
* Wi-Fi standards and naming, and the 2.4, 5, and 6 GHz bands and their tradeoffs.
* Channels and interference: overlapping channels on 2.4 GHz, and how neighbouring networks affect your link.
* WPA, WPA2, and WPA3, and what changed between them.
* Reading signal, noise, and SNR with `system_profiler SPAirPortDataType`.

**Still to firm up:**
* Stating stateful versus stateless in one sentence, without a pause.
* Choosing a channel from the neighbouring networks you can see.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
sudo socketfilterfw --getglobalstate        # firewall on or off
sudo socketfilterfw --listapps              # apps in the firewall list
system_profiler SPAirPortDataType | grep -E "Channel|Signal|Noise"   # link quality, from Day 25
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 21-25 cover filtering and the wireless link. The first pass needed a second look, and the slip-ups are now configured and reviewed.
* **Correction to Day 68:** the firewall scenario that matches this material is Day 55 (ping works, port 443 blocked). Day 61 was a TLS certificate interception case, not a firewall.
* **Blocker:** stateful versus stateless is the one to say cleanly, since it blurs easily.