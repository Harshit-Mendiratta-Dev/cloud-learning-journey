# Day 75 - Hands-on Refresh: Whois, Nmap, Telnet, Traceroute
**Date:** 2026-10-30
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

| Tool | Layer it tests | What it showed |
|---|---|---|
| `whois microsoft.com` | Registry data (not a network layer) | IANA to VeriSign (`.com` registry) to MarkMonitor (registrar). Registry gives nameservers, status, and dates. Registrant details come from the registrar. |
| `sudo nmap -sS -v scanme.nmap.org` | Layer 4 (ports) | Open: 22, 80, 9929. Filtered (no reply): 25, 135, 139, 179, 445, 646. Closed (reset): the other 991 ports. |
| `traceroute microsoft.com` | Layer 3 (path) | Hops 1-7 reply, then a long run of silent hops. Reaches the final region around hop 21. |

**What I noticed:**
* `nmap -sS` without `sudo` quit. A SYN scan needs raw packet access, so it needs root. This is a permissions failure, not a network one.
* Filtered ports are the firewall-drop signal from Day 18: no reply at all. Closed ports send a reset.
* Silent traceroute hops don't mean a dead path. Routers often ignore or deprioritise probe replies, and the trace still reaches the destination region.
* Hop 1 is my router, `reliance.reliance (192.168.x.x)`, matching Day 9.

**Corrections from the re-run:**
* A `* * *` hop in traceroute isn't a fault on its own. Look at whether later hops reply.
* Registry is not registrant: the registry (VeriSign) holds the nameservers, and the registrar (MarkMonitor) holds the registrant contact.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
whois microsoft.com                        # registry and registrar chain (Day 20)
sudo nmap -sS -v scanme.nmap.org           # SYN scan needs root (Day 18)
telnet <host> 443                          # is this TCP port accepting connections? (Day 55)
traceroute microsoft.com                   # where does the path stop? (Day 9)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** the refresh tied each tool to its layer. Nmap's three port states and traceroute's silent hops are the two readings to keep straight.
* **Blocker solved:** `nmap -sS` needs `sudo`, and the first error was permissions, not networking.
* **Next:** Day 76, the re-read of Days 41-44 (the four consolidation blocks), then the remaining Phase 1 review.