# Day 27 - Light Day: Terminal Reading Practice & Subnetting
**Date:** 2026-09-11
**Focus Area:** Phase 1 - Networking (Light Day / Comprehension Check)
**Time Spent:** ~1.5 Hours

## 1. Key Concepts Learned (Comprehension-Focused, Minimal New Theory)

* **`curl -I` (HEAD request), corrected understanding:** `-I` does NOT stand for "interface" — it triggers an HTTP **HEAD** request instead of the default **GET**. HEAD asks the server for headers only, skipping the actual content body entirely — this is why no HTML appears in the output. Distinct from `-v` (verbose), which keeps the default GET request but adds extra diagnostic detail around it (DNS resolution, TCP connect, full headers) — `-I` changes *what* is requested, `-v` changes *how much curl reports* about the same request.

* **`301 Moved Permanently` vs `200 OK`, applied practically:** `google.com` redirects to `www.google.com` (confirmed via the `Location:` header) as a standard practice to consolidate traffic onto one canonical URL — not a security measure. `example.com` returned `200 OK` directly since the requested URL was already the actual destination, no redirect needed.

* **Two separate caching layers, correctly distinguished:** `Cache-Control: max-age` governs how long *your own browser* should cache a response before re-fetching. `cf-cache-status: HIT` + `Age` (seen on the `example.com`/Cloudflare response) is a completely different actor — it tells you the *CDN* (Cloudflare) served this response from its own cache rather than asking the origin server fresh, and `Age` is how long that CDN-level cached copy has existed. Different systems, different caches, both operating simultaneously.

* **`Server:` header ≠ domain ownership — real correction needed here.** `Server: cloudflare` means Cloudflare's software generated/served the response (it's a CDN/reverse-proxy sitting in front of the site), not that Cloudflare owns the domain. To find actual ownership, the correct tool is `whois` (Day 20), a completely separate question from what the `Server:` header answers. Also clarified: `Server: gws` (seen on Google's response) stands for "Google Web Server" — an internal server software name, unrelated to DNS root servers (Day 10) despite surface-level naming similarity.

* **Traceroute latency calibration — refined against a real baseline:** A jump from ~7ms to ~32ms (seen today, Reliance Jio infrastructure → Cloudflare's network) does NOT indicate crossing an international border — that scale of jump is consistent with staying within the same country, likely just a handoff between two different network providers. True international-distance jumps look like Day 7's baseline (~250-380ms, India-to-US class latency). Good lesson in calibrating "does this jump mean something" against a previously observed real number, not just noticing that latency increased.

## 2. Hands-on Lab & Commands
```bash
curl -I google.com
curl -I example.com
traceroute cloudfare.com   # typo (missing 'l'), still resolved to a real, separate domain
```

Plus one subnetting problem, solved cleanly on the first attempt:
* `192.168.50.140/27` → correctly derived host bits (5), block width (32), block range (128-159), network address (`192.168.50.128`), broadcast address (`192.168.50.159`), and full usable range (`192.168.50.129`-`192.168.50.158`) — this time giving the actual range rather than just the count, confirming that earlier habit (Day 19/23) is now fully corrected.

## 3. Key Takeaway / Blocker Solved

Deliberately light day — three rounds of terminal-output comprehension questions (no new material taught upfront, just testing reading ability on real output) plus one subnetting problem. Caught and corrected two real gaps: `-I` flag meaning, and conflating a `Server:` header with domain ownership. Also demonstrated good self-advocacy — pushed back when an explanation about `-I` vs `-v` sounded ambiguous, prompting a clearer restatement rather than passively accepting a confusing answer. High Availability/Redundancy (STP) deferred to tomorrow as planned.