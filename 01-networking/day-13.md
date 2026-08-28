# Day 13 - The TCP 3-Way Handshake & Teardown, Live
**Date:** 2026-08-28
**Focus Area:** Phase 1 - Networking
**Time Spent:** 3.5 Hours

## 1. Key Concepts Learned (1.5h Theory)

* **TCP is connection-oriented (Layer 4, Transport):** Unlike DHCP/UDP (Day 12, connectionless — just broadcasts, no handshake), TCP requires both sides to formally establish a connection before any data is exchanged. This applies regardless of distance — the same handshake happens whether the two endpoints are on the same LAN or across the internet; TCP's job is reliable, ordered delivery, not distance-crossing itself.

* **The 3-Way Handshake:**
  - **SYN:** Client → Server: "I want to connect, my starting sequence number is X."
  - **SYN-ACK:** Server → Client: "Acknowledged, and I also want to connect — my sequence number is Y." (One packet does double duty — request AND acknowledgment.)
  - **ACK:** Client → Server: "Confirmed, ready to send data."
  - The `ack` number in each packet is always "the next byte I expect from you" — e.g., replying to `seq 278831700` gives `ack 278831701` (seq + 1).

* **`tcpdump` Flag Decoder (as actually seen, not just theory):**
  | Flag | Meaning |
  |---|---|
  | `[S]` | SYN |
  | `[S.]` | SYN-ACK (`.` = ACK) |
  | `[.]` | ACK only, no data |
  | `[P.]` | PUSH-ACK — actual payload being delivered immediately, not buffered |
  | `[F.]` | FIN-ACK — one side signaling it's done sending |

* **ECN (Explicit Congestion Notification) — a real-world twist not in the original plan:** Modern TCP handshakes often include extra flags beyond plain SYN/ACK:
  - `[SEW]` on the SYN = client also announcing ECN + CWR support (asking "can we use congestion signaling instead of just dropping packets under load?")
  - `[S.E]` on the SYN-ACK = server confirming ECN support
  - This is a capability negotiation happening *during* the handshake itself, layered on top of the basic 3-way process.

* **TCP Teardown is NOT always simultaneous — often a "half-close":** Each side closes independently:
  1. Side A sends `[F.]` ("I'm done sending")
  2. Side B acknowledges, then sends its *own* `[F.]` when it's also done
  3. Side A sends a final `[.]` to acknowledge B's FIN
  - This is more accurate to real traffic than a symmetric "both sides FIN at once" mental model.

* **IPv6 in practice:** Today's capture happened entirely over IPv6 (`IP6` in every line) rather than IPv4 — a reminder that modern client/server pairs often prefer IPv6 automatically when both support it, even though most tutorials default to IPv4 examples.

## 2. Hands-on Lab & Commands (1.0h Lab)
```bash
# First attempt — filter typo, no packets captured (good lesson in double-checking filters)
sudo tcpdump -n -i en1 host example.com and port 8^C

# Successful capture — dropped the port filter, added -v for full decode
sudo tcpdump -n -i en1 -v host example.com

# Trigger traffic in a second terminal
curl http://example.com
```

## 3. Key Takeaway / Blocker Solved

Blocker Solved: First `tcpdump` attempt failed with 0 packets captured due to an incomplete filter (`port 8` cut off mid-command). Re-ran without the port filter and captured cleanly.

Captured the complete TCP lifecycle for an HTTP request to `example.com`: SYN (with ECN negotiation) → SYN-ACK → ACK → PUSH-ACK carrying the `GET` request → PUSH-ACK carrying the `200 OK` response and page body → asymmetric FIN/ACK teardown from each side independently. Confirmed the `ack = seq + 1` mechanic directly from raw sequence numbers in the capture, and saw firsthand that real-world TCP traffic includes extra negotiated capabilities (ECN) beyond the textbook-simple 3-flag handshake.

Also did a rapid-fire self-review compressing ARP, TCP, DNS, CNAME, and NAT/PAT into single-sentence summaries — caught one real gap (calling CNAME a "proxy" instead of an "alias" — a proxy actively sits in the traffic path, CNAME is just a static DNS-level pointer resolved once, with no traffic passing through it) — otherwise strong recall across 8 days of material.