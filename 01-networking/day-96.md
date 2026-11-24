# Day 96 - Hands-on Check: Gateway and ARP State (Days 62, 95)
**Date:** 2026-11-23
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Checked the Mac's current gateway and ARP state, and compared it with the Day 62 output.**

**What I found:**
* **Gateway:** `default` on `en1` still points to the router, `192.168.x.1`. It hasn't changed since Day 62.
* **Router entry:** the router's MAC is cached, and it matches the MAC from earlier captures. The gateway is the same device.
* **Another device:** a different host on the Wi-Fi is cached through ARP (`?` means no hostname was resolved). Its entry has changed since Day 62, because ARP entries expire and are relearned.
* **Subnet broadcast:** `192.168.x.255` maps to the MAC broadcast address `ff:ff:ff:ff:ff:ff`. It's not learned by ARP. It's the destination for packets sent to every host on the segment, the subnet version of DHCP's broadcast from Day 12.
* **mDNS:** `224.0.0.251` is a permanent multicast entry, mapped by a fixed rule, not learned by ARP.

**Still to firm up:**
* Saying why an ARP entry for a host can change between days, while the gateway's entry stays the same.
* Naming which entries in `arp -a` are learned and which are fixed by rule (multicast and broadcast).

## 2. Hands-on Lab & Commands 
```bash
netstat -nr | grep default      # gateway on the first default line
arp -a                          # cached IP-to-MAC entries
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** `arp -a` mixes learned entries (the router and other hosts) with rule-based ones (multicast and the subnet broadcast). Knowing which is which explains why most entries change and some never do.
* **Blocker:** the subnet broadcast entry is new in the output. It isn't a device, so it doesn't belong in the same category as the router.