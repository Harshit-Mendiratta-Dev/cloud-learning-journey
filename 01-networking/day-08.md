# Day 08 - Layer 2: MAC Addresses & ARP
**Date:** 2026-08-23
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2.5 Hours

## 1. Key Concepts Learned (2h Theory)
* **IP vs. MAC — The Delivery Analogy:** IP addresses (Layer 3) are the postal address on an envelope — the final destination, unchanged for the whole trip. MAC addresses (Layer 2) are the delivery truck — the physical device moving the frame one hop at a time, changing at every stop along the way.
* **Why Layer 2 exists:** Cables and Wi-Fi antennas don't understand IP addresses — only MAC addresses. Every packet needs a physical MAC-addressed "wrapper" (an Ethernet frame) to actually move across a wire or over the air, even though the IP address inside never changes.
* **ARP (Address Resolution Protocol):** Bridges Layer 3 and Layer 2. When a device needs to reach an IP on the local network (like the default gateway), it broadcasts a request essentially asking "who has this IP? Tell me your MAC address." The reply gets cached in the ARP table (seen via `arp -a` on Day 07) so it doesn't have to ask every time.
* **The "different truck, same envelope" principle:** When sending a packet to a remote IP (e.g. Cloudflare's `1.1.1.1`), the destination MAC in the Ethernet frame is NOT Cloudflare's MAC — it's the local router's MAC. The router receives the frame, strips off that Layer 2 wrapper, and builds a brand new frame (with its own MAC as source) to send the packet on to the next hop. The IP address stays constant across every hop; the MAC address changes at each one.

## 2. Hands-on Lab & Commands (0.5h Lab)
```bash
# Review IP-to-MAC mappings cached locally
arp -a

# Capture an Ethernet frame with Layer 2 (MAC) info visible via -e flag
sudo tcpdump -n -e -i en1 -c 1 icmp
# (run in a second terminal, while the capture is listening:)
ping -c 1 1.1.1.1
```

## 3. Key Takeaway / Blocker Solved
Blocker Solved: first attempt (`sudo tcpdump -n -e -i en1 c 1 icmp`) threw a syntax error from a missing dash before the count flag (`c 1` instead of `-c 1`) — corrected and re-ran successfully.

Captured frame confirmed the lesson directly: source MAC was my Mac's Wi-Fi card (`5a:70:9a:88:c0:0b`), destination MAC was my home router (`f0:ed:b8:5f:bf:f5`) — matching the router's entry from Day 07's `arp -a` output exactly — even though the ICMP packet's actual IP destination was Cloudflare (`1.1.1.1`), nowhere near my router. Confirms that Layer 2 (MAC) only ever targets the *next hop*, while Layer 3 (IP) targets the *final destination*, for the entire trip.