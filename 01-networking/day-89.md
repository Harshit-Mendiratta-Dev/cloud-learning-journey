# Day 89 - Review: DHCP and Day 12 Notes
**Date:** 2026-11-16
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the DHCP material and the Day 12 notes, line by line.**

**Revisited:**
* **DHCP is Layer 7 over UDP** (ports 67 and 68). It's an application protocol, not Layer 3, and it doesn't use ICMP.
* **DORA exchange:** Discover (client broadcasts to `255.255.255.255`), Offer (server replies with an IP and options), Request (client accepts the offer), Acknowledge (server confirms the lease).
* **Why DORA is broadcast:** the client has no IP yet, so it can't address the server directly. The broadcast reaches the DHCP server on the local segment.
* **Lease contents:** the IP address, subnet mask, default gateway, DNS server, and lease time.
* **Lease timers:** T1 is the point where the client tries to renew with the same server. T2 is the fallback, where it broadcasts to any server. Once the lease expires, the client has to start over.
* **BOOTP header fields (Day 12):** `yiaddr` (the IP being offered), `siaddr` (the server's address), `chaddr` (the client's MAC), and `xid` (the transaction ID that matches the replies to the request).
* **Diagnostic commands (Day 12):** `ipconfig getpacket en1` shows the lease that was received, and `ipconfig set en1 DHCP` forces a fresh request.

**Still to firm up:**
* Saying why T1 renews with the same server, and T2 broadcasts to any server, in one line each.
* Naming the four DORA messages in order without a pause.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ipconfig getpacket en1          # the lease the Mac received (Day 12)
sudo ipconfig set en1 DHCP      # force a fresh DHCP request (Day 12)
ipconfig getifaddr en1          # the current IP, or 169.254 if DHCP failed
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** DHCP is a Layer 7 exchange that has to finish before the Mac can route at Layer 3. Each step of DORA is visible in the lease details.
* **Blocker:** the two lease timers (T1 and T2) are easy to blur. T1 is the same-server renewal, T2 is the any-server fallback.