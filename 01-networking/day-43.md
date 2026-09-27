# Day 43 - Retention Check: Days 21-30 Review
**Date:** 2026-09-27
**Focus Area:** Phase 1 - Networking (Consolidation)
**Time Spent:** ~1.5 Hours

## 1. Key Concepts Learned (Review Format, No New Theory)

Self-directed re-read of Days 21-30 notes, followed by a 7-question no-notes quiz covering firewalls, VLANs/trunking, STP, WAN/MPLS, and the Day 16→17 subnetting recovery arc.

**Score: 7.5/8** — the strongest of the three consolidation reviews so far, with several answers matching or exceeding original session depth.

* **Stateful vs. stateless firewalls** — fully correct.
* **Access port vs. trunk port, 802.1Q tagging cycle** — fully correct, complete walkthrough of the tag/strip sequence between switches.
* **STP mechanism** — fully correct: broadcast storm cause, the two-mechanism relationship (receiving-port exclusion + STP blocking working together), and Bridge ID election (priority + MAC, lower wins, admin-configurable). Full form (Spanning Tree Protocol) correctly recalled after a brief hesitation.
* **macOS Application Firewall — one correction:** initially described the discovered default state as "allow," when it was actually fully **disabled** (no filtering happening at all). Corrected and clearly re-articulated the distinction: disabled = no monitoring whatsoever; enabled-with-allow = actively evaluating traffic and choosing to permit it.
* **LAN vs. WAN** — fully correct, both scale and ownership halves present.
* **VLAN trunking / MPLS structural parallel** — fully correct, accurately drawn across both mechanisms (tag/label add-and-strip cycle, roles, invisibility to endpoints).
* **Subnetting (`/28`) + reflective question on Day 16/17** — subnetting math fully correct (block 5, network/broadcast/usable range). Reflective answer accurately and honestly identified that Day 16's actual blocker was time pressure, not a conceptual gap, and that Day 17's fix worked specifically because there was room to slow down and rebuild from first principles — consistent with what was logged at the time.

## 2. Hands-on Lab & Commands
None today — pure review and recall-testing, no CLI work.

## 3. Key Takeaway / Blocker Solved

One clean correction (macOS firewall's disabled state vs. an "allow" policy state) resolved immediately with clear understanding afterward — no lingering confusion. This review confirms the Days 21-30 material, despite being entirely theory-heavy and Socratically-built rather than hands-on, has held up under cold recall just as well as — arguably better than — the earlier hands-on-heavy blocks. Three-part consolidation pass (Days 1-10, 11-20, 21-30) now complete; Days 31-40 remain optional before starting the deferred troubleshooting methodology week on Monday.