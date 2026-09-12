# Day 28 - STP (Spanning Tree Protocol): Preventing Broadcast Storms
**Date:** 2026-09-12
**Focus Area:** Phase 1 - Networking (High Availability & Redundancy)
**Time Spent:** 3.0 Hours

## 1. Key Concepts Learned (Built Through Iterative Tracing, Same Socratic Style as Day 26)

* **The problem STP solves — broadcast storms:** In a network with a physical loop (e.g., three switches wired in a triangle), a single broadcast frame floods endlessly. Switches have **no memory** of having seen a frame before — each switch only ever excludes the *one specific port* a frame just arrived on for *that* instance, then floods out everywhere else. This means the same broadcast keeps circulating and multiplying between switches forever, exponentially consuming bandwidth until the network is saturated — a genuine "broadcast storm."

* **STP full form: Spanning Tree Protocol** — named for graph theory's "spanning tree": a loop-free subset of connections that still reaches every point in a network.

* **How STP prevents the storm — three-step process:**
  1. **Root Bridge election:** All switches compare their **Bridge ID** (Priority + MAC address) — lowest wins. Priority is admin-configurable (default 32768) specifically so an admin can deliberately choose which switch becomes root; MAC address serves purely as a tiebreaker when priorities match.
  2. **Shortest-path calculation:** every other switch calculates its shortest path (fewest hops) to the elected root.
  3. **Blocking redundant links:** any link that isn't part of a switch's shortest path to root gets put into a permanent **Blocking state** — the port stays physically connected but will not forward *any* traffic, in *either* direction, until a failure elsewhere triggers it to automatically unblock (the actual "high availability" payoff — redundant links exist as instant failover, not everyday extra capacity).

* **Two loop-prevention mechanisms work together, not just one:**
  - **Basic flooding rule (from Day 26):** never re-flood a frame back out the exact port it arrived on — universal behavior, with or without STP.
  - **STP's permanent blocking:** closes the gap the basic flooding rule can't solve alone — a frame reaching the same switch again via a genuinely *different* physical path (not just bouncing straight back). Both mechanisms operate simultaneously; STP's blocking handles redundant-path duplication, basic flooding handles immediate backward bouncing.

* **Ties in shortest-path distance are resolved via Bridge ID comparison between the competing intermediate switches** — not the switch calculating the tie itself.

* **Worked a full 7-switch topology by hand** (root + 6 others, several with genuine ties and some with clear-cut shortest paths), correctly identifying which links stay active vs. blocked, and traced a full broadcast from a leaf switch (7) through the resulting loop-free tree, confirming every switch receives exactly one copy, zero duplicates, zero infinite loop.

* **Cross-site connectivity (e.g., two office topologies in different cities):** STP doesn't inherently know or care about "site A" vs "site B" as concepts — it treats the entire combined network as one topology once any link connects them, re-running root election and path calculation across everything. A root-to-root link would likely end up active anyway if it's genuinely the shortest path, but STP doesn't require it to be root-to-root specifically. Real long-distance site-to-site connections in practice use routers/WAN technology (Layer 3) rather than simple Layer 2 switch trunking, since STP doesn't scale well across large geographic distances — flagged as a natural lead-in to a future WAN technologies topic.

## 2. Hands-on Lab & Commands
No terminal commands today — STP is switch-hardware behavior, not directly demonstrable on a home Mac. Entire session was theory built through iterative topology-tracing and self-correction, same style as Day 26's VLAN/trunking session.

Illustrative Bridge ID format covered conceptually (not run on this machine):
Priority: 32768
MAC: 5a:70:9a:88:c0:0b
Combined: 32768.5a70.9a88.c00b


## 3. Key Takeaway / Blocker Solved

Worked through several real misconceptions via active back-and-forth, correcting them collaboratively rather than being told the answer outright:
- Initially underestimated how a triangle loop actually multiplies (assumed it would "naturally" avoid sending back to the origin) — corrected by tracing the exact per-hop port-exclusion logic frame by frame.
- A genuinely sharp catch on my end being wrong: correctly identified that Switch 4 (with a direct 1-hop link to root) would have *both* its redundant 2-hop links blocked outright, resolving an ambiguity I'd left unclear in an earlier topology trace.
- Correctly pushed back when I dismissed the "don't flood back out the receiving port" mechanism as irrelevant to STP — it's real and works *alongside* STP's blocking, not replaced by it; both mechanisms needed correcting into one accurate combined picture.

This was a genuinely collaborative, iterative session — the final 7-switch topology trace and the cross-site architectural question at the end both reflect real synthesis of the concept, not just definitions memorized. STP is now understood at the mechanism level: why storms happen, how root election and blocking prevent them, and how the redundant "wasted" paths function as automatic failover insurance.