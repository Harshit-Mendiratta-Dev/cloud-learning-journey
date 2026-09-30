# Day 46 - Troubleshooting Methodology (Part 2: Layered Lab)
**Date:** 2026-09-30
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2.0 Hours

## 1. Key Concepts Learned (0.5h Theory)
* **Divide-and-conquer:** start in the middle (Layer 3, e.g. ping `1.1.1.1`). If it passes, everything below is fine, so move up the stack. If it fails, the fault is at that layer or below, so move down.
* **"No route to host" vs "Request timeout":** "No route" comes from my own machine, which has no path out, so no packet was sent. A timeout means a packet was sent and nothing came back, which points at the path or the far end.
* **TTL as a hint:** local pings show ttl 64 (no routers crossed); `1.1.1.1` showed ttl 51 (many hops).
* **Latency as a hint:** ~0.2 ms to my own IP (never leaves the machine), ~6.7 ms to the router (crosses Wi-Fi), ~8-13 ms to `1.1.1.1`.
* **Mac interfaces:** en0 is Ethernet (no cable, so no IP); en1 is Wi-Fi.
* **HTTP 301:** a redirect (google.com to www.google.com), which counts as success for the application layer.

## 2. Hands-on Lab & Commands (1.5h Lab)
```bash
# Baseline: everything healthy (all passed)
ping -c 2 127.0.0.1                 # network stack on my machine
ipconfig getifaddr en1              # my IP (en0 blank: no Ethernet cable)
ping -c 2 [my-ip]                   # my own interface
netstat -nr | grep default          # gateway is the first "default" line
ping -c 2 [gateway-ip]              # router reachable via Wi-Fi
ping -c 2 1.1.1.1                   # internet, no DNS involved
dig +short google.com               # DNS works
curl -I https://google.com          # HTTP works (301 redirect)

# Break 1: Wi-Fi off
ipconfig getifaddr en1              # returned nothing: no IP address
ping -c 2 [gateway-ip]              # "No route to host"
ping -c 2 1.1.1.1                   # "No route to host"
# Wi-Fi reconnected mid-test: one packet lost, one returned at 135 ms

# Break 2: dead DNS server (queried directly; system DNS unchanged)
dig +time=2 +tries=1 google.com @10.255.255.1   # "connection timed out"
ping -c 2 1.1.1.1                   # pass
dig +short google.com               # pass (normal DNS)
```

## 3. Key Takeaway / Blocker Solved
* **Blockers solved:** typing `<ip>` literally (zsh treats `<` as a redirect and throws a parse error, so type only the real value); `-c` needs a number (`-c 2`); typing `+time=2`, not `=time=2`; double-checking IPs (I once typed `198.` instead of `192.`).
* **Takeaway:** starting in the middle is faster because one test rules out half the stack. In Break 2, `1.1.1.1` passing showed the network was fine, so I skipped Layers 1-3 and went straight to DNS.
* **Break comparison:** Wi-Fi off gave "No route to host" (fault at Layer 1/2, on my side). Dead DNS gave a timeout (network fine, only the DNS server not answering).