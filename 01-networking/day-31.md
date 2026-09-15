# Day 31 - WAN Technologies: Leased Lines, MPLS & SD-WAN
**Date:** 2026-09-15
**Focus Area:** Phase 1 - Networking (WAN Technologies)
**Time Spent:** 2.0 Hours

## 1. Key Concepts Learned (Built Through Iterative Correction, Same Style as Days 26/28)

* **LAN vs. WAN — full forms and the real distinguishing factor:**
  - **LAN** = Local Area Network
  - **WAN** = Wide Area Network (corrected from an initial guess of "wireless" — the "W" refers to geographic *scale*, has nothing to do with wireless technology; a WAN can be entirely wired)
  - The real distinguishing factor isn't distance alone, it's **ownership**: a LAN is infrastructure one organization fully owns and controls; a WAN necessarily crosses infrastructure owned by someone else (an ISP or carrier), since you're bridging a distance too large to physically own end-to-end yourself.
  - Grounding realization: a home internet connection is technically a WAN link already — the "last mile" from router to ISP, and everything beyond, is WAN infrastructure not owned by the user.

* **Leased Line:** A dedicated, always-on, point-to-point connection rented from a carrier — exclusive to one organization's traffic, guaranteed bandwidth, historically the reliability "gold standard," but expensive.

* **MPLS (Multiprotocol Label Switching):**
  - Routers still have IP addresses — MPLS does not hide addresses from traceroute in the way it might seem.
  - The actual mechanism: an **ingress router** (WAN entry point) attaches a simple **label** to a packet, functioning like a reference ticket for a pre-determined path. Every router inside the MPLS backbone reads only that label (fast, simple lookup) instead of doing a full IP routing-table consultation at every hop. The **egress router** (WAN exit point) removes the label before the packet continues via normal IP routing — the destination network never knows a label was ever attached.
  - Sits conceptually at **"Layer 2.5"** — takes a Layer 3 IP packet and wraps it with a Layer-2-style fast-forwarding mechanism.
  - **Structural parallel to VLAN trunking (Day 26), self-identified and then refined:**

    | | VLAN Trunking | MPLS |
    |---|---|---|
    | Marker added | 802.1Q tag (VLAN ID) | MPLS label |
    | Added by | Sending switch's trunk port | Ingress router |
    | Removed by | Receiving switch's trunk port | Egress router |
    | What it represents | Group membership (which VLAN) | Pre-determined path selection |
    | Scope | Between two switches, local/campus network | Across a carrier's WAN backbone |

    Same underlying design pattern (tag at entry, strip at exit, fast-path everything in between, invisible outside that segment) — but trunking tags identify *group membership*, while MPLS labels identify a *specific pre-computed path*.

* **SD-WAN (Software-Defined WAN):** Smart software-driven routing and monitoring (correct initial instinct) **combined with** using ordinary, cost-effective internet connections — sometimes multiple simultaneously (e.g., wired broadband + 4G/5G backup) — instead of requiring expensive dedicated leased lines. The software intelligently load-balances and automatically fails over between links. This combination is why SD-WAN has become popular: much of leased-line-style reliability, at a fraction of the cost.

## 2. Hands-on Lab & Commands
No terminal commands today — WAN infrastructure is carrier-level, not directly demonstrable on a home Mac. Entire session was theory, built through the same iterative Socratic correction style as Days 26 and 28: proposing an understanding, testing it, refining it.

## 3. Key Takeaway / Blocker Solved

One real correction (WAN's full form, mistakenly guessed as "wireless"), one clarification (MPLS labels are about forwarding speed/path selection, not hiding IPs from traceroute), and one completed-but-partial answer (SD-WAN's software-routing instinct was right, but was missing the "uses cheap standard internet links instead of leased lines" half of the picture).

The genuine highlight of the day: independently drew a structural parallel between MPLS and Day 26's VLAN trunking — both use a "tag at entry, strip at exit" pattern for fast, simplified forwarding — then correctly refined the distinction between what each type of tag actually represents (group identity vs. path selection) once it was pointed out. This closes the loop from Day 28's Delhi/Mumbai cross-site question — real long-distance site-to-site connectivity typically uses exactly this kind of WAN technology (leased lines, MPLS, or SD-WAN) rather than simple switch trunking, which doesn't scale across large geographic distances.