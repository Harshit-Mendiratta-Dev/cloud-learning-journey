# Day 42 - Retention Check: Days 11-20 Review
**Date:** 2026-09-26
**Focus Area:** Phase 1 - Networking (Consolidation)
**Time Spent:** ~1.5 Hours

## 1. Key Concepts Learned (Review Format, No New Theory)

Self-directed re-read of Days 11-20 notes, followed by an 8-question no-notes quiz covering DORA/DHCP, TCP flags, NAT/PAT, `lsof`, Nmap scan types, and WHOIS/typosquatting.

**Score: 7/8** — strong retention overall, with one recurring gap worth flagging.

* **DORA process** — fully correct, all four steps accurately walked through.
* **DHCP/UDP reasoning** — fully correct, with a genuinely good added point beyond the original answer: TCP's handshake being tied to one specific endpoint would waste effort against a server unwilling/unable to respond, reinforcing the core "can't broadcast over TCP" reasoning.
* **`tcpdump` TCP flags** (`[S]`, `[S.]`, `[.]`, `[P.]`, `[F.]`) — fully correct, complete sequence including the asymmetric FIN/ACK teardown observed directly in the original Day 13 capture.
* **NAT/PAT — a recurring correction:** said "Network/Port Address *Protocol*" instead of **Translation**. This is the second time this specific naming slip has surfaced (first on Day 24, now again here) — the underlying mechanism and "why" (IPv4 exhaustion) are both solid both times, but the exact terminology hasn't fully locked in yet. Flagged for deliberate, repeated drilling until automatic.
* **`lsof -i -n -P`** — one flag correction: `-P` means "don't resolve port numbers to service names" (numeric ports), not "port" itself. The broader reasoning approach (compare addresses across entries to spot shared-IP usage) was already sound — pushed back appropriately when a correction was more about my phrasing than an actual gap, and that pushback was justified.
* **SYN scan vs. Connect scan** — fully correct, including privilege-level dependency and the "half-open/stealth" characterization.
* **Filtered vs. closed ports** — fully correct, matching the original Day 18 capture precisely.
* **WHOIS/typosquatting** — fully correct summary of defensive domain registration practices.

## 2. Hands-on Lab & Commands
None today — pure review and recall-testing, no CLI work.

## 3. Key Takeaway / Blocker Solved

One genuine, recurring gap identified and worth acting on: the NAT/PAT "Translation vs. Protocol" naming slip has now appeared twice across two separate reviews (Day 24, Day 42), despite the conceptual understanding being consistently solid both times — a clear signal this specific term needs deliberate repetition rather than passive re-exposure. Everything else from Days 11-20 held up well under cold recall. Plan: continue the consolidation pass (Days 21-40 next, if time allows) before starting the deferred troubleshooting methodology week on Monday.