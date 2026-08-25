# Day 10 - DNS Hierarchy, Query Chains & Record Types
**Date:** 2026-08-25
**Focus Area:** Phase 1 - Networking
**Time Spent:** 3 Hours

## 1. Key Concepts Learned (2h Theory)
1. Key Concepts Learned

    **DNS (Domain Name System):** The protocol that translates human-readable domain names into IP addresses.

    **Recursive Resolver vs. Authoritative Server:**

        Recursive Resolver: The "detective" server (like your ISP or Cloudflare's 1.1.1.1) that walks the global internet tree on your behalf to find the IP.

        Authoritative Nameserver: The final server that actually owns and holds the official records for a specific domain.

    **The DNS Resolution Hierarchy:**

        Root Servers (.): The invisible dot at the end of every domain. There are 13 global clusters that direct queries to the correct domain extension.

        TLD (Top-Level Domain) / gTLD (Generic Top-Level Domain): Servers responsible for extensions like .com, .org, or .net.

        Authoritative Nameservers: (e.g., ns1.google.com) The servers that return the final answer.

    **Key DNS Record Types:**

        A Record: Maps a domain name directly to an IPv4 address. (Often returns multiple IP addresses for load balancing).

        CNAME (Canonical Name): An alias record that points one domain name to another domain name instead of an IP. This is heavily used in AWS and cloud networks to hide changing infrastructure behind a static name.

    **TTL (Time to Live) in DNS:** The time (in seconds) that a local machine or recursive resolver is allowed to cache the IP address before it must ask the Authoritative Nameserver again.

## 2. Hands-on Lab & Commands (1.0h Lab)
```bash
# Attempt 1: Trace the DNS resolution path using default ISP resolver
dig +trace google.com
```
    Finding 1 (The Block): The default local IPv6 resolver (XXXX:XXX:XXXX:XXXX::XXXX:XXXX) refused to provide the global root hints, returning a tiny 28-byte response and stalling the trace. Many local ISP routers block root-level traces.
```bash
# Attempt 2: Bypass local cache/ISP blocks by querying Cloudflare's resolver directly
dig @1.1.1.1 +trace google.com
```
    Finding 2 (The Full Traversal): Bypassing the local block successfully forced the Mac terminal to walk the entire 4-stage DNS tree:

        Hit the Root Servers (a.root-servers.net.).

        Routed to the .com gTLD servers (e.gtld-servers.net.).

        Reached Google's Authoritative servers (ns4.google.com.).

        Returned multiple A records (e.g., 192.XXX.XXX.XXX) with a TTL of 300 seconds (5-minute cache).        
```bash
# Inspect CNAME alias records and CDN endpoints
         dig @1.1.1.1 www.yahoo.com
```
     Finding 3 (CNAME Aliasing): Confirmed that [www.yahoo.com](https://www.yahoo.com) does not point directly to an IP address. Instead, it returned a CNAME pointing to an internal load balancer/CDN domain (me-ycpi-cf-www.gXX.yahoodns.net.).

## 3. Key Takeaway / Blocker Solved
Blocker Solved: Local dig +trace commands can fail if the ISP's DNS server restricts root queries. Resolved by using the @ flag to explicitly route the initial query to a public resolver (@1.1.1.1).

Synthesis: Witnessed the complete recursive DNS chain live in the terminal. The CNAME discovery perfectly bridges networking fundamentals to cloud computing, demonstrating how large platforms abstract dynamic, ever-changing IP addresses behind canonical domain names.