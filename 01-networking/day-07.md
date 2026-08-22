# Day 07 - OSI Layer Synthesis: Watching a Request Travel Live
**Date:** 2026-08-22
**Focus Area:** Phase 1 - Networking
**Time Spent:** 3 Hours

## 1. Key Concepts Learned (1h Theory)
* **curl -v (Application Layer):** Shows the full HTTP conversation — `>` lines are the request my machine sends, `<` lines are the response headers the server sends back. Without `-v`, curl only shows the raw body (the "cargo"); `-v` reveals DNS resolution, TCP connect status, and the full request/response headers around it.
* **netstat -an (Transport Layer):** Snapshot of active TCP/UDP sockets. `-a` = show all sockets (listening + established), `-n` = numeric output (raw IPs/ports, no hostname resolution). The `State` column matters most — `ESTABLISHED` means the connection is live, `LISTEN` means a process is waiting for connections, `TIME_WAIT` means a connection just closed and is cooling down.
* **grep gotcha:** `grep 80` matches the literal digits "80" anywhere in a line — including inside unrelated numbers like `49380` or port `993`. Not the same as filtering for "port 80 specifically." Also confirmed `-a` (without `-n`) resolves port numbers to service names (e.g. `http` instead of `80`), which broke a `grep 80` filter entirely.
* **traceroute (Network Layer):** Sends packets with increasing TTL so each hop's router "dies" one step further and reveals itself. `* * *` means a hop didn't reply (often intentional — routers can be configured to not respond to probes), not necessarily broken. Reaching the final destination isn't required for traceroute to be useful — AWS/CDN edge infra often blocks probes entirely even when the site itself works fine.
* **arp -a (Data Link Layer):** Shows IP-to-MAC address mappings your machine has learned on the *local* network segment only — never leaves the router. `-a` = show the whole cached table. `(incomplete)` entries mean an ARP request went out but got no reply (device offline or doesn't exist).
* **TTL as a cross-layer concept:** Same TTL field that traceroute manipulates hop-by-hop is visible directly in a raw IP packet header (confirmed via tcpdump — saw `40` = TTL 64 in the hex dump).
* **tcpdump -X (raw packet capture):** Reading the actual bytes on the wire. Hand-decoded an ICMP echo request packet: IP version/header length, total length, TTL, protocol number (01 = ICMP), source/destination IP in hex, and ICMP type (08 = echo request) — all matching the human-readable summary line one field at a time.

## 2. Hands-on Lab & Commands (2.0h Lab)
```bash
curl -v http://neverssl.com
netstat -an | grep tcp | grep 80
traceroute neverssl.com
arp -a

# Timeout troubleshooting — isolating the variable
curl -v http://neverssl.com          # timed out (curl: 28, Couldn't connect to server)
curl -v http://google.com            # succeeded immediately (301 redirect to www)
netstat -an | grep tcp | grep 80     # re-checked live connections
curl -v http://neverssl.com          # retried — succeeded via IPv6 this time

# Raw packet inspection
sudo tcpdump -n -i en1 -c 1 -X host 1.1.1.1
```

## 3. Key Takeaway / Blocker Solved
Ran into an intermittent timeout on `curl -v http://neverssl.com` (`Operation timed out` on both IPv6 and IPv4). Isolated it by testing a different host (`google.com`, which worked instantly) — this ruled out my own network as the problem. Retried neverssl.com afterward and it succeeded via IPv6. Conclusion: transient routing issue specific to that host's path (consistent with the rough, high-latency, multi-hop route seen in yesterday's traceroute to the same AWS IP), not a local network or DNS problem. This is a real "isolate the variable" troubleshooting pattern: one host failing while everything else works points at that host's specific path, not your machine.

Also learned a `grep` limitation the hard way — `grep 80` is a substring match, not a port match, and gave false negatives/positives across two different netstat outputs today.