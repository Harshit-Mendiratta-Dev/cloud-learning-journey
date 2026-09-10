# Day 26 - VLANs & Trunking: Built Through Active Correction
**Date:** 2026-09-10
**Focus Area:** Phase 1 - Networking
**Time Spent:** 3.0 Hours

## 1. Key Concepts Learned (Built Through Iterative Self-Correction, Not Passive Reading)

* **VLAN (Virtual Local Area Network):** A way to logically segment devices connected to one physical switch into separate groups (by department, function, etc.), without needing separate physical hardware for each group. Each VLAN is its own isolated broadcast domain — broadcasts from one VLAN's devices only reach other devices in the *same* VLAN, not the whole switch. Benefits: security isolation and reduced unnecessary broadcast traffic.

* **Switch vs. router vs. Mac's network interfaces — an important early correction:** A switch is a standalone networking device connecting *multiple different devices* together at Layer 2 (MAC addresses) — not a component living inside any single computer, and not the same thing as `en0`/`en1` (which are just interfaces on one device, connecting *out to* a switch). A router works at Layer 3 (IP addresses), connecting *different networks* together. Many home devices bundle router + switch + Wi-Fi access point into one physical box, which can blur the distinction, but conceptually they're separate functions.

* **Access port vs. trunk port:**
  | | Access Port | Trunk Port |
  |---|---|---|
  | VLANs carried | Exactly 1 | Multiple |
  | Frame type | Untagged | Tagged (802.1Q) |
  | Typical use | Connecting an end device (PC, printer, phone) | Connecting two switches together |
  - End devices are completely unaware VLANs exist — they always send normal, untagged frames.
  - Every switch in a multi-switch setup has *both* port types simultaneously: access ports facing its own local devices, trunk port(s) facing other switches.

* **Trunking (802.1Q) — the full corrected sequence:**
  1. Device sends an untagged frame → arrives at the local switch's **access port**
  2. If the destination is on a different switch, the frame is forwarded internally to that switch's **trunk port**
  3. The trunk port **adds the 802.1Q tag** at this exact moment (not earlier, not at the device) — tag is 4 extra bytes identifying the VLAN
  4. Tagged frame crosses the shared inter-switch link
  5. The receiving switch's trunk port **reads and removes the tag**
  6. The now-untagged frame is forwarded internally to the correct **access port**, delivered to the destination device — which never knows any tagging happened

* **Multi-switch broadcasts (VLAN spanning 3+ switches):** Same tag/strip cycle, just repeated in parallel — the originating switch sends a tagged copy out *every* trunk port leading toward another switch carrying that VLAN.

* **Switch flooding — how one frame reaches multiple ports:** A switch doesn't literally split one physical copy across ports — it holds the frame briefly in an internal buffer and generates an independent transmission out each relevant port from that buffered data. This "flooding" behavior (frame sent out every port except the one it arrived on) is exactly what happens with broadcasts — directly connects back to Day 12's DHCP Discover broadcast, which relied on this same mechanism without it being explicitly named at the time.

* **Bandwidth/tagging overhead — addressed and resolved:** The 802.1Q tag itself (4 bytes) adds negligible processing overhead — not a real bottleneck. The genuine engineering concern is **shared trunk-link capacity** (multiple VLANs' traffic sharing one physical link's total bandwidth) — a capacity-planning problem, not something caused specifically by tagging. Real networks size trunk links with extra headroom (e.g., 10 Gbps trunks aggregating multiple 1 Gbps access links) for exactly this reason. VLANs, if anything, *reduce* overall congestion by containing broadcasts within their own group rather than flooding the entire switch.

* **STP (Spanning Tree Protocol) — named but not covered in depth today:** In more complex, interconnected switch topologies (e.g., three switches wired in a triangle), broadcasts could loop forever without a protocol to prevent it. STP solves this — flagged as a topic for a future day, under "high availability/redundancy."

## 2. Hands-on Lab & Commands
No terminal commands today — pure theory, built entirely through a Socratic back-and-forth: proposing a mental model, testing it against a scenario, having it corrected, and rebuilding it, repeated across several rounds (switch identity, tagging location, port types, multi-switch broadcasts, and flooding mechanics).

## 3. Key Takeaway / Blocker Solved

Several real misconceptions surfaced and were corrected through iterative questioning rather than passive reading:
- Initially conflated a "switch" with a personal device's network interface — corrected to: switch is standalone shared infrastructure.
- Initially framed trunking as a monitoring "authority" — corrected to: trunking is a port configuration/mode on the switches themselves.
- Initially unsure whether tagging happened at the device or the switch — correctly reasoned toward "the switch, at the point of crossing to another switch" through an analogy (ship/crew/flag/seal) that ended up mapping almost perfectly onto the real mechanism.
- Initially paired trunk port with only one switch and access port with the other — corrected to: both switches have both port types, used symmetrically for their respective roles (access facing local devices, trunk facing the other switch).
- Proactively questioned whether switches "make copies" during broadcast flooding — correctly reasoned through to the right mechanism (buffered read, independent transmission per port) with only a minor clarification on buffer persistence.

This was a genuinely different kind of learning day — no terminal, no video, entirely built through proposing an understanding, testing it, and rebuilding it under correction. VLANs and trunking are now solidly understood, including the underlying "why" at each step, not just the vocabulary.