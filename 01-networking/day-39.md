# Day 39 - Light Day: Quick Quiz on AAA, WAN & Dynamic Routing
**Date:** 2026-09-23
**Focus Area:** Phase 1 - Networking (Rapid Review)
**Time Spent:** ~1 Hour

## 1. Key Concepts Learned (Review Format, No New Theory)

Deliberately light day — ran a 5-question quiz covering recent material (AAA/802.1X, WAN, dynamic routing/BGP) rather than starting new content, since **formal troubleshooting methodology** is being deliberately saved for a full dedicated week later, given its importance to the actual target role.

**Score: essentially 5/5** — strong retention across the board:

* **802.1X vs. firewall/ACL timing** — initial answer had a wording ambiguity ("verifies identity every time a request is made") that read as continuous re-checking, but clarified to mean "every fresh login/connection attempt" — which is actually correct. Confirmed understanding: 802.1X authenticates once, at first connection, gating network access entirely; firewalls filter an already-connected device's ongoing traffic.
* **MFA's three categories** (something you know/have/are) and why combining across categories (not within one) genuinely strengthens security — fully correct.
* **LAN vs. WAN** — correctly identified both halves: scale (confined space vs. spanning large/global distances) and ownership (WAN as the connective infrastructure between independently-owned Autonomous Systems like Google, Jio, Airtel, Amazon).
* **RIP vs. OSPF** — correctly described RIP's pure hop-count metric versus OSPF's bandwidth-derived cost metric (faster = lower cost = preferred), including that OSPF may choose a longer-hop-count path if it's genuinely faster overall.
* **Why BGP prioritizes business relationships/policy over pure shortest path** — gave a genuinely expanded, multi-part answer beyond what was asked: respecting the cooperative/legal foundation WAN infrastructure is built on, security/loop-prevention benefits of routing only through agreed channels, and protecting each network's own finite bandwidth capacity from being overburdened by uncontrolled traffic.

## 2. Hands-on Lab & Commands
None today — pure recall-based quiz, no CLI work, deliberately light session.

## 3. Key Takeaway / Blocker Solved

No real blocker — essentially a clean sweep across AAA, WAN, and dynamic routing material, with only one wording-based ambiguity (resolved immediately on clarification, not an actual knowledge gap). Confirms the recent theory-heavy stretch (Days 31, 34, 37-38) has genuinely stuck, not just been covered. Troubleshooting methodology remains intentionally deferred to its own dedicated week; network monitoring/management is the only other topic left before that.