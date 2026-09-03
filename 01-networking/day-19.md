# Day 19 - Light Revision Day
**Date:** 2026-09-03
**Focus Area:** Phase 1 - Networking (Revision)
**Time Spent:** ~1 Hour

## 1. Key Concepts Learned (Revision Only)

Reread Days 16–18 in full: the subnetting struggle-and-recovery arc (block boundaries, network/broadcast address derivation from first principles) and the Nmap revisit (SYN vs. Connect scans, open/closed/filtered port states, and how they tie back to TCP handshake and NAT/firewall behavior from earlier days).

Solved one subnetting problem from memory, no notes, to check retention:
* `172.20.9.55/27` — correctly identified host bits (5), block width (32), block number, network address (`172.20.9.32`), and broadcast address (`172.20.9.63`) on the first pass, no struggle. One minor clarification needed: gave the usable host *count* (30) instead of the actual *range* (`172.20.9.33`–`172.20.9.62`) — small distinction between "how many" and "which ones," corrected immediately.

## 2. Hands-on Lab & Commands
No terminal commands today — pure revision and one manual subnetting problem, no CLI work.

## 3. Key Takeaway / Blocker Solved
No blocker — a deliberate light day by design, after 18 days including a dense subnetting recovery arc and Nmap deep-dive. Confirmed Day 17's subnetting fix has held up under a cold retention test (no notes, no hints) — clean solve on host bits, block width, and address derivation, with only a minor slip between "count" and "range" that self-corrected instantly. Good sign the concept is genuinely retained, not just freshly memorized. Firewalls (the next major topic) intentionally deferred to Saturday for a proper, unhurried session.