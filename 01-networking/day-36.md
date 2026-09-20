# Day 36 - ASN Lookup & the IGP/EGP Distinction
**Date:** 2026-09-20
**Focus Area:** Phase 1 - Networking (Light Day / BGP Follow-Up)
**Time Spent:** ~1.5 Hours

## 1. Key Concepts Learned

* **ASN, confirmed:** the unique identifying number assigned to an Autonomous System, letting BGP routers unambiguously refer to a specific network ("AS55836" for Jio) across a global system of tens of thousands of independent networks — accurately self-described beforehand as "a badge/label for the AS."

* **Where BGP/ASN data actually lives — two genuinely separate things:**
  - **ASN registration/identity** (ASN ↔ organization mapping, IP block ownership) — centrally registered and publicly queryable through the RIR system (ARIN, APNIC, RIPE, etc.) — exactly what a `whois -h whois.cymru.com` query pulls from.
  - **Actual live routing configuration** (peering relationships, announced prefixes) — distributed, configured locally on each organization's own border routers, then dynamically propagated hop-by-hop via BGP itself — no single central directory holds this.
  - Phone-book analogy: the RIR registry is like a public phone book (who owns what), while actual routing/call connection happens dynamically through the network's own internal signaling, not by looking anything up centrally.

* **Router role vs. router hardware — an important correction worked through:** OSPF and BGP don't require different hardware — any router can run either or both, depending on configuration. What differs is **role/position**:
  - A router entirely internal to one AS typically only needs an **interior** routing protocol.
  - A router sitting at an AS's border, connecting to a different AS, needs to speak BGP specifically — no other protocol handles inter-AS communication.
  - A single physical border router commonly runs **both** simultaneously, for two different directions of traffic.

* **IGP vs. EGP — formal terminology, independently reasoned toward before being introduced:**
  - **IGP (Interior Gateway Protocol)** — routing protocols for *within* one AS (OSPF, RIP)
  - **EGP (Exterior Gateway Protocol)** — routing protocols for *between* ASes (BGP, effectively the only one used in practice today)
  - Correctly deduced through logic alone that a border router must run both an IGP (to talk to its own organization's internal routers) and BGP (to talk to neighboring organizations), before these formal terms were introduced.

## 2. Hands-on Lab & Commands
```bash
whois -h whois.cymru.com " -v 142.250.182.46"
```

Real output decoded field by field: AS 15169 | 142.250.182.46 | 142.250.182.0/24 | US | arin | 2012-05-24 | GOOGLE - Google LLC, US
- Confirmed Google's real-world ASN (15169)
- The `/24` BGP Prefix is literally the address block Google announces to the internet via BGP — direct, visible evidence of Day 34's "announcement" concept
- `arin` confirmed the Day 34 RIR hierarchy concretely — this specific allocation falls under ARIN's (North America) authority

## 3. Key Takeaway / Blocker Solved

No blocker — a deliberately light day, closing a loop deferred from Day 34. The ASN lookup gave direct, concrete evidence for two full days of prior theory (RIR hierarchy, BGP announcements) in a single query. The IGP/EGP distinction was genuinely self-derived through logical reasoning about router configuration before being told the formal terminology — a good sign the underlying mental model (not just vocabulary) is solid.