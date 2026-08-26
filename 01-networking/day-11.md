# Day 11 - Recap, Revision & DNS Tools
**Date:** 2026-08-26
**Focus Area:** Phase 1 - Networking (Revision + DNS Tooling)
**Time Spent:** ~2 Hours

## 1. Key Concepts Learned (Revision + New)

**Revision — corrected/clarified from Days 5-10:**
* **Default Gateway ≠ automatically ".1":** The network address, host range, and broadcast address are fixed by subnet math — but *which* host is configured as the default gateway is just convention (often `.1`, but could be any usable address).
* **TCP 3-Way Handshake, precisely:** SYN (client requests connection) → SYN-ACK (server acknowledges AND requests its own connection, in one packet) → ACK (client confirms). Both sides request and confirm — it's not one-directional.
* **ARP is local-only:** ARP (Address Resolution Protocol) never leaves the local network segment. It only resolves "IP → MAC" for devices on the same physical network (e.g., finding the default gateway's MAC). It has no role in reaching the ISP or the internet.
* **DHCP vs. NAT — not the same thing:**
  - **DHCP** (Dynamic Host Configuration Protocol) = assigns a *private* IP to a device joining the local network.
  - **NAT** (Network Address Translation) = happens at the router, swaps a private IP for the router's public IP when traffic leaves the local network. This — not DHCP — is what allows multiple devices to share one public IP.
* **There are two unrelated things both called "TTL":**
  - **DNS TTL** = a cache expiration time (in seconds) attached to a DNS record, telling resolvers how long to cache an answer before re-querying.
  - **IP Packet TTL** = an 8-bit hop-counter in every IP packet's header, decremented by 1 at each router; hits 0 → packet dropped, "Time to live exceeded" sent back. This is what `traceroute` and TTL-based ping experiments (Day 9) rely on.
  - Unqualified "TTL" in a networking/troubleshooting context defaults to the IP packet TTL.
* **CNAME's real purpose:** A CNAME (Canonical Name) record is an alias — "this name = another name," used for indirection/flexibility (e.g., changing backend infrastructure without updating every reference). Load balancing from multiple returned IPs is a separate technique (round-robin DNS) that can happen independently of whether a CNAME is involved.

**New concept — MX Records:**
* **MX** = Mail Exchange record. Tells the internet which server handles email for a domain (e.g., `google.com mail is handled by 10 smtp.google.com`).
* The number preceding the mail server (`10`, `5`) is a **priority** — lower numbers are tried first. Multiple MX records provide failover (if the primary mail server is down, mail falls back to the next-priority server).

**New concept — DNS Tool Comparison:**

| Tool | Style | Best For |
|---|---|---|
| `dig` | Verbose, technical, shows full response sections/flags | Deep troubleshooting, `+trace` for full hierarchy |
| `host` | Clean, one-liner per record, human-readable | Quick checks, surfaces record types like MX easily |
| `nslookup` | Older, cross-platform (works on Windows too) | Legacy environments, basic name→IP lookups |

* **"Non-authoritative answer"** (seen in `nslookup` output) means the answer came from a resolver's cache, not directly from the domain's authoritative nameservers — still correct, just secondhand, unlike `dig +trace` which walks the authoritative chain directly.

## 2. Hands-on Lab & Commands (Revision + Tool Exploration)
```bash
# Self-quiz on Days 5-10 (no notes) — subnetting, TCP handshake, grep behavior,
# MAC/router mechanics, IP TTL vs DNS TTL, dig +trace hierarchy, A vs CNAME records

# New tool exploration
host google.com
host etherkart.com
nslookup google.com
```

## 3. Key Takeaway / Blocker Solved
Ran a self-quiz across Days 5–10 without notes to test retention before adding new material. Most concepts held up well (subnetting logic, MAC-vs-IP delivery model, grep substring-matching behavior, DNS hierarchy order). Caught and corrected three real gaps: confusing DHCP with NAT, conflating DNS TTL with IP packet TTL, and misattributing CNAME's purpose to load balancing rather than aliasing/indirection. These are common points of confusion even beyond Day 11, worth having gotten wrong now rather than later.

Also picked up MX records "for free" while exploring `host` as an alternative DNS tool — a good example of hands-on exploration surfacing new concepts naturally, rather than needing to be taught upfront.