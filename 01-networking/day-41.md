# Day 41 - Retention Check: Days 1-10 Review
**Date:** 2026-09-25
**Focus Area:** Phase 1 - Networking (Consolidation)
**Time Spent:** ~1.5 Hours

## 1. Key Concepts Learned (Review Format, No New Theory)

Self-directed re-read of Days 1-10 notes, followed by an 8-question no-notes quiz covering subnetting, TCP handshake, grep behavior, MAC/router mechanics, address types, TTL, DNS hierarchy, and record types.

**Score: 7.5/8** — genuinely strong retention for the earliest material, 40 days later.

* **Subnetting (`/28` problem)** — solved cleanly: correct host bits, block width, block identification, network/broadcast addresses, and full usable range.
* **TCP 3-way handshake** — fully correct, including the nuance that SYN-ACK performs double duty (acknowledgment + its own connection request).
* **`grep` behavior** — correctly reaffirmed: grep is a literal substring match, not a smart filter; imprecision comes from the query, not the tool.
* **MAC/router hop mechanics** — correctly explained why the destination MAC on the first hop is the router's, not the remote server's (no direct physical connection to the destination exists).
* **Network/host/broadcast addresses — one correction made:** initially described the network address as "the first *usable* address" — corrected to: the network address is the first address in a block, but is specifically **not usable** (reserved to identify the subnet); the first usable host address is one number after it.
* **TTL (both kinds)** — correctly and clearly distinguished IP packet TTL (hop-counter, prevents infinite loops) from DNS TTL (cache expiration in seconds) — confirms Day 11's original correction has fully stuck.
* **DNS resolution hierarchy** — correctly walked root → TLD (.com) → authoritative (google.com's own servers) → final answer.
* **A vs. CNAME records** — both correctly explained, including the CNAME indirection/flexibility reasoning from Day 11's correction, still intact.

## 2. Hands-on Lab & Commands
None today — pure review and recall-testing, no CLI work.

## 3. Key Takeaway / Blocker Solved

No real blocker — one minor precision correction (network address being unusable rather than "first usable") out of eight questions. This confirms the Days 1-10 foundation remains solid well after the fact, not just freshly memorized at the time. Part of a planned multi-day consolidation pass (Days 1-10 today, 11-20 next, possibly 21-40 after that) before starting the deferred formal troubleshooting methodology week on Monday.