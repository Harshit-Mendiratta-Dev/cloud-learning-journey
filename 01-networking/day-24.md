# Day 24 - Closing Day 23's Four Gaps
**Date:** 2026-09-08
**Focus Area:** Phase 1 - Networking (Gap Reinforcement)
**Time Spent:** ~2 Hours

## 1. Key Concepts Learned (Reinforcement — Re-explained, Not Just Re-read)

Closed all four gaps identified in Day 23's cumulative review, by re-explaining each concept back in my own words rather than just re-reading the correction:

* **DHCP uses UDP, not TCP — properly understood this time:** DHCP's very first packet (Discover) is broadcast to the entire local network since there's no single specific target yet (multiple DHCP servers could exist, and the client has no IP to begin with). TCP requires a known, fixed destination and a full 3-way handshake before any data moves — structurally impossible for a broadcast. UDP's "just send it, no handshake required" model is the only option that fits.

* **MAC address hop-locality — direction corrected from Day 23's flip:** A device's own MAC address only ever appears as the *source* on the very first hop of its outgoing packet. After that, at every subsequent hop, both source and destination MAC are completely replaced with that hop's specific pair of devices. MAC addresses are never persistent across a journey — they're always local to exactly one hop, in both directions (outgoing and the reply).

* **A record naming — corrected:** "A" stands for **Address** (an IPv4 address record). Also properly corrected: the IPv6 equivalent is **AAAA** (four A's, not "AAA") — named that way since IPv6 addresses are roughly 4x longer than IPv4.

* **NAT/PAT — full names and root cause, now complete:**
  - **NAT** = Network Address Translation
  - **PAT** = Port Address Translation (also known as "NAT overload" in some contexts, since it overloads one public IP across many simultaneous connections via port tracking)
  - **Why NAT exists:** IPv4 address exhaustion — there aren't enough public IPv4 addresses for every private device on Earth to have its own, so NAT/PAT lets many private devices share a small number of public IPs.

## 2. Discussion: AI Nudging Toward Phase 2 

Raised a direct question about why Claude/Gemini both tended to nudge toward Phase 2 material early, despite the deliberate 90-day Phase 1 pacing plan. Identified as a structural "novelty bias" in AI tutoring — suggesting new material reads as more like progress than suggesting depth/reinforcement, even when reinforcement is what's actually needed.

Confirmed Phase 1's curriculum is far from exhausted — untouched topics remain for the rest of the 90-day window, including: VLANs & trunking, wireless networking, WAN technologies, dynamic routing protocols (RIP/OSPF/BGP), high availability/redundancy, physical media/cabling, network hardening & AAA, formal troubleshooting methodology, IPv6 in depth, and network monitoring/management.

## 3. Hands-on Lab & Commands
No terminal commands today — pure conceptual reinforcement and career-path discussion, no CLI work.

## 4. Key Takeaway / Blocker Solved

All four gaps from Day 23's review are now closed and re-verified through active re-explanation rather than passive correction. No blocker today — a deliberate pause to consolidate before moving into new, untouched Phase 1 territory starting tomorrow. Reconfirmed the 90-day Phase 1 pacing decision is sound — plenty of real material remains, and prioritizing mastery over speed is the right call, not a sign of falling behind. 