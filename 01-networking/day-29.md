# Day 29 - IPv6 In Depth: Address Types, Privacy Extensions & Live Tracing
**Date:** 2026-09-13
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2.0 Hours

## 1. Key Concepts Learned (1.5h Theory)

* **Why IPv6 exists:** IPv4's 32-bit address space (~4.3 billion addresses) isn't enough for every device on Earth (Day 14's NAT/PAT reasoning). IPv6 uses **128 bits**, providing an address space (~340 undecillion) large enough that exhaustion isn't a realistic future concern.

* **Address format:** 8 groups of 4 hexadecimal digits, colon-separated (e.g., `2405:201:4008:8130:104c:f298:61c5:aeab`) — structurally different from IPv4's 4 dot-separated decimal numbers. Shorthand rules: leading zeros in a group can be dropped; one consecutive run of all-zero groups can be collapsed to `::` (only once per address, to avoid ambiguity).

* **Address types, with real examples classified from my own machine:**
  - **Link-local** (`fe80::/10`) — local-segment only, never routes anywhere, auto-generated. Confirmed on my own interface.
  - **Global unicast** — publicly routable, real internet addresses. My machine has **two simultaneously**, generated for different privacy purposes:
    - **"Secured"** — a stable address for the current network session, with the host portion (last 64 bits) generated via a privacy-randomized method rather than embedding the real MAC address directly (older IPv6 auto-config used to do this, creating a tracking risk) — persists roughly for the current network session, not permanently fixed.
    - **"Temporary"** — a periodically-rotating global address, specifically used for *outgoing* connections, so a repeatedly-visited site can't use a stable IPv6 address as a long-term tracking identifier. Confirmed directly: my own outgoing `traceroute6` used this address as the source, not the "secured" one.
  - **Unique local** (`fc00::/7` or `fd00::/8`) — roughly IPv6's equivalent to private IPv4 ranges, though used less commonly in practice than expected.
  - **General classification shortcut confirmed:** anything not starting with `fe80::` (link-local) or `fc00::`/`fd00::` (unique local) is generally global/public.

* **IPv6 doesn't use NAT the way IPv4 does** — confirmed directly via live traceroute: even the very first hop (home router) showed a real, global IPv6 address, not a private range, since every device can have its own globally routable address.

## 2. Hands-on Lab & Commands (0.5h Lab)
```bash
ifconfig en1 | grep "inet6"
traceroute6 example.com
```

## 3. Key Takeaway / Blocker Solved

Initial misread: assumed "secured" meant private/non-routable and "temporary" meant a borrowed public IP — both incorrect. Corrected: both are global/public addresses; the labels describe *how* each was generated for privacy purposes, not whether they're routable.

Live `traceroute6` confirmed several concepts directly: the outgoing connection used the **temporary** address as source (exactly matching its stated purpose), hop 1 revealed the home router's own global IPv6 address (no NAT hiding anything, unlike IPv4), silent hops behaved identically to IPv4 traceroutes (packet still passing through, just not responding to probes), and address prefixes shifted visibly at each network boundary crossed — offering a more transparent view of network topology than IPv4's NAT-obscured equivalent. Also observed a load-balancing artifact at hop 11, where three probes returned two genuinely different addresses, including a recognizable Cloudflare prefix near the end of the path — consistent with `example.com`'s known Cloudflare-fronted infrastructure.

Confirms IPv6 traceroute uses the identical underlying mechanism (TTL/hop-limit, Day 9) as IPv4 — only the addressing structure and NAT-free transparency differ.