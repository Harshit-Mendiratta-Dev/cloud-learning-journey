# Day 70 - Recall Quiz: Re-read of Days 15-25
**Date:** 2026-10-25
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2 Hours 

## 1. Key Concepts Learned 

**Topics covered in the quiz, and what I corrected:**

* **NAT and PAT (Day 15 and Day 24 gap):** NAT translates addresses, one private IP to one public IP. PAT (also called NAT overload) translates ports too, so many private hosts share one public IP. "NAT overload" and "PAT" are the same mechanism, so use either name. Write it as "NAT overload (PAT)," not "NAT overload or PAT."
* **`lsof -i -n -P` (Day 14):** lists open files, and on Unix a network socket counts as a file, so this lists live connections. `-i` selects internet connections, `-n` skips DNS lookups, `-P` shows port numbers. On Day 14 it showed several apps sharing one public IP through different ports, which is the port-level proof of PAT.
* **TCP handshake (Day 13):** the SYN-ACK carries the server's SYN flag and an ACK flag. Its sequence number is the server's own initial sequence number. Its acknowledgement number is the client's initial sequence number plus one. The client's ACK completes the handshake.
* **Host bits and the `/n` prefix:** `/n` counts network bits, not host bits. `/0` means zero fixed bits, so every address matches it, which is why `0.0.0.0/0` is the default route. A block's size is 2^(host bits), and each octet still stays at 255 or below.
* **`/26` worked example:** 6 host bits, 64 addresses in the block, 62 usable hosts. Network `192.168.1.64`, broadcast `192.168.1.127`, usable range `192.168.1.65` to `192.168.1.126`.
* **Stateful vs stateless firewalls (Day 22-23):** a stateful firewall tracks connections, so it lets replies through because it remembers the connection they belong to. A stateless firewall judges each packet alone and needs explicit rules for return traffic.
* **Wi-Fi bands (Day 25):**
  * 6 GHz is the least crowded, and only Wi-Fi 6E and Wi-Fi 7 devices can use it. It's fast but covers less distance.
  * 2.4 GHz has the best range and wall penetration, and it's the most crowded. Bluetooth, microwaves, and cordless phones share it.
  * Only channels 1, 6, and 11 don't overlap on 2.4 GHz, so neighbours on popular channels interfere with you.
* **Nmap scan types (Day 18):** the default connect scan completes the full TCP handshake. A SYN scan (`-sS`) stops after the SYN-ACK, which is why it's quieter.

**Still to firm up:**
* SNR and what it means for link quality (question still open).
* Stating stateful vs stateless in one sentence without a pause.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Quiz day, nothing run.
lsof -i -n -P                                  # live connections, ports as numbers
system_profiler SPAirPortDataType | grep -E "Channel|Signal|Noise"   # signal, noise, channel
nmap -sS -v scanme.nmap.org                    # SYN scan, for comparison only
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** the Day 15-25 foundations hold up when recalled. Most corrections were about wording (NAT overload vs PAT, "contains" vs specific fields, "band" vs "standard") rather than concepts.
* **Blocker solved:** the `/n` prefix counts network bits, so `/0` doesn't break the octet limit.
* **Mistake to remember:** I said "contains" for the SYN-ACK when I meant the fields. Be specific about which field and what it confirms.
* **Next:** SNR and link quality, then the Day 26-30 re-read (VLANs, trunking, STP).