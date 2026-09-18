# Day 34 - Dynamic Routing: RIP, OSPF, and BGP
**Date:** 2026-09-18
**Focus Area:** Phase 1 - Networking (Dynamic Routing Protocols)
**Time Spent:** 3.5 Hours

## 1. Key Concepts Learned (Built Through Iterative Reasoning and Synthesis)

* **Why dynamic routing exists:** Static routing (Day 9's `netstat -nr`) is fixed, manually/one-time set, with no ongoing adaptation. Real networks with many routers and multiple possible paths need routers to actively communicate and continuously update their own routing tables — this is what dynamic routing protocols provide.

* **RIP (Routing Information Protocol):** Oldest, simplest — uses only **hop count** as its metric, ignoring link speed/quality entirely. Can pick a technically worse path (self-derived metro analogy: fewer stops doesn't guarantee a faster trip if those segments are slow).

* **OSPF (Open Shortest Path First):** Uses **cost**, derived from link bandwidth (faster links = lower cost = preferred), converges faster than RIP, widely used in real enterprise networks today. Runs *within* one organization's network.

* **Static vs. dynamic routing — where each actually lives, synthesized correctly through discussion:**
  - **Host devices** (a laptop, a phone) always use **static routing** — there's only ever one realistic exit (the default gateway), so nothing needs dynamic optimization. True even within large, complex enterprise networks — the host's own behavior never changes.
  - **Routers with genuine multiple-path choices** — run dynamic routing (OSPF internally, BGP at organizational borders).
  - Local/home networks generally never need dynamic routing at all: switches (+ VLANs + STP) handle Layer 2 organization, and static routing handles the trivial "send unknown traffic to the one gateway" decision.
  - Multiple simultaneous default gateways on a host are technically configurable but rare/problematic (asymmetric routing risk); real host-level redundancy is typically handled via interface failover (e.g., Ethernet-to-Wi-Fi), not two live gateways splitting traffic.

* **BGP (Border Gateway Protocol) — "the protocol that runs the internet":**
  - **Autonomous System (AS):** a network under one organization's independent control (an ISP, a cloud provider, a university), each with a unique ASN. The internet is thousands of independent ASes, not one unified network.
  - BGP operates specifically at the **borders** where one AS connects to another — deciding how traffic hands off between organizations.
  - Unlike OSPF (pure technical efficiency), **BGP decisions are heavily driven by business relationships and policy** — peering agreements, paid transit contracts — not just shortest/fastest path. A technically shorter path may be deliberately avoided due to commercial arrangements.
  - **The real-world foundation underneath BGP, confirmed through discussion:** actual physical interconnection between networks (often at neutral Internet Exchange Points), backed by genuine business/legal peering or paid-transit agreements negotiated between companies — BGP's technical route announcements are configured *on top of* these pre-existing agreements, not independent of them.
  - Route announcement/selection happens **in advance and continuously**, not per-packet — by the time an actual packet arrives at a border, the preferred handoff path is already sitting in a routing table, similar in spirit to how static/OSPF tables work.

* **Full routing sequence across an internet-scale journey, correctly synthesized and anchored to real traceroute evidence (Days 7, 9, 29):**
  Host (static) → Router (OSPF within local ISP's AS) →Border of that AS (BGP hands off to a different AS, e.g. one ISP to another) →OSPF within the new AS → repeat as needed until destination
  Directly connects to the private-IP-then-public-IP-then-latency-jump pattern observed in earlier traceroute captures.

## 2. Hands-on Lab & Commands
No terminal commands today — dynamic routing protocols run on carrier/enterprise router hardware, not directly demonstrable on a home Mac. Entire session was theory, built through the same iterative Socratic reasoning style as Days 26, 28, and 31 — including several self-generated analogies (metro system for RIP vs. OSPF, later refined map-exchange framing for BGP) that were tested, corrected, and rebuilt collaboratively.

## 3. Key Takeaway / Blocker Solved

Worked through a genuinely deep synthesis today, correcting a couple of real misunderstandings along the way:
- Initial confusion about OS layers ("host is on Layer 1") — corrected to: every device operates across multiple layers simultaneously; the static-vs-dynamic comparison is about two different *approaches* to the same Layer 3 problem, not a cross-layer comparison.
- Initial BGP analogy ("police checkpost," inspecting traffic in real time) — corrected to: BGP is closer to networks exchanging reachability announcements *in advance*, with routing decisions pre-computed and sitting in tables by the time actual traffic arrives, not real-time inspection.

The session's strongest moment was independently and correctly sequencing the full static → OSPF → BGP → OSPF flow across an AS border crossing, then pushing further to correctly infer that BGP's policy layer is underpinned by genuine real-world business/legal peering agreements — arriving at this conclusion through reasoning rather than being told directly. This closes out dynamic routing conceptually, completing the last major "how does a packet actually get routed" topic in Phase 1's remaining list.