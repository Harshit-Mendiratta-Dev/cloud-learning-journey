# Day 32 - Quick Quiz: Days 25-31 Review (Time-Constrained)
**Date:** 2026-09-16
**Focus Area:** Phase 1 - Networking (Rapid Review)
**Time Spent:** 1.0 hours

## 1. Key Concepts Learned (Review Format, No New Theory)

Ran a quick, unplanned 5-question quiz covering the recent theory-heavy stretch (Days 25–31: IPv6, VLANs/trunking, STP, WAN/MPLS), prompted by limited time available today. **Score: 4.3/5 (86%)**.

* **IPv6 secured vs. temporary addresses** — correctly recalled, including a precise use-case example (traceroute) matching Day 29's own live capture.
* **Switch flooding mechanics** — correctly recalled after a brief clarifying re-ask: floods to every port except the one the frame arrived on.
* **STP mechanism — the one real gap surfaced today:** correctly recalled the overall behavior (shortest-path-to-root election, redundant link blocking, works alongside the flooding-exclusion rule), but made three specific errors:
  - Full form incorrectly stated as "switch trunking protocol" instead of **Spanning Tree Protocol**
  - Bridge ID's components mislabeled — said "Port ID" when the correct term is **Priority** (paired with MAC address as tiebreaker)
  - **Mistakenly folded VLAN trunking's 802.1Q tagging mechanism into the STP explanation** — these are two separate mechanisms that happen to both involve switches/ports: STP decides *which links are allowed to forward traffic at all* (blocking/unblocking); trunking decides *how multiple VLANs share an already-active link* (tagging). Worth a deliberate mental re-separation, since teaching them back-to-back (Day 26 → Day 28) made them easy to blend.
* **LAN vs. WAN** — scale correctly explained (confined space vs. crossing large distances), with a good concrete example (Delhi/Mumbai). Missing the second half of the "why not wireless" reasoning — the **ownership** dimension (WAN crosses infrastructure owned by a carrier, not just infrastructure that's far away).
* **VLAN trunking vs. MPLS structural parallel** — correctly and fluently re-explained, extending Day 31's original insight with a well-articulated train-ticket analogy for MPLS labels.

## 2. Hands-on Lab & Commands
None today — pure recall-based quiz, no CLI work, due to limited available time.

## 3. Key Takeaway / Blocker Solved

No blocker — a deliberately efficient use of limited time, testing retention on the theory-heavy Days 25–31 stretch rather than skipping review entirely. One genuine gap surfaced and flagged for reinforcement: STP and VLAN trunking, while related and often deployed together on the same switches, are functionally separate mechanisms (blocking vs. tagging) that got blended together under quiz pressure. Also had a brief, productive side conversation today about CompTIA Network+ certification cost (~$358/₹30-36k) — decided to pursue the free AWS Certified Cloud Practitioner voucher available through Amazon's internal employee benefits instead, deferring any cert spending decision until closer to the Phase 1→2 transition window (Day 75-91).