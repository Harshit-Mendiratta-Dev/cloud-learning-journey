# Day 14 - NAT/PAT: Seeing Address Translation Live
**Date:** 2026-08-29
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2 Hours

## 1. Key Concepts Learned (1h Theory)

* **NAT (Network Address Translation):** Exists because IPv4 doesn't have enough addresses for every device on Earth to have its own public IP. NAT lets a router translate private IPs (used inside a home/office network) into a public IP (used on the internet) and back.

* **PAT (Port Address Translation):** The specific flavor of NAT almost every home router uses. Instead of needing one public IP per device, PAT lets *many* private devices share a *single* public IP by tracking connections using port numbers — each outgoing connection gets a unique port, so the router can tell which reply belongs to which internal device.

* **The NAT Translation Table (conceptual):** The router maintains a mapping like `private IP:port ↔ public IP:port` for every active connection. When a reply comes back addressed to the public IP, the router checks this table to know exactly which internal device/port to forward it to. Unsolicited incoming traffic with no matching table entry is simply dropped — part of why home networks are relatively safe by default, and also the root of real-world NAT traversal problems (VoIP, gaming, remote access).

* **NAT is an IPv4-specific problem.** IPv6 has such a large address space that every device can have its own real, globally routable address — no translation needed. Confirmed directly: today's `ifconfig` output showed the Mac holding actual global IPv6 addresses, and later `lsof` showed IPv6 connections going out with **no NAT involved at all**, unlike IPv4 connections on the same machine at the same time.

* **Router's dual addressing role:** Same physical router, two completely different addresses depending on which "side" it's facing — its private IP (`[home-router]`, e.g. `192.168.x.1`, from Day 9's traceroute) when talking to devices on the local network, versus its public IP (masked here) when representing the whole network to the internet.

* **`lsof` (List Open Files):** On Unix systems, network sockets are treated as files, so `lsof -i` lists every live network connection, per-process — showing protocol (TCP/UDP), IP version, local/remote address and port, and connection state (e.g. `ESTABLISHED`).

## 2. Hands-on Lab & Commands (1h Lab)
```bash
# Confirm private IPv4 address (and observe IPv6 addresses too)
ifconfig en1 | grep "inet"

# Confirm public-facing IPv4 address (forces IPv4 specifically)
curl -4 ifconfig.me

# List every live connection, process by process
lsof -i -n -P | grep ESTABLISHED
```

## 3. Key Takeaway / Blocker Solved

No command errors today — clean run throughout. The real takeaway was direct, hands-on proof of a concept previously only understood in theory:

- `ifconfig` showed the private IP (`192.168.x.x`) my Mac believes it has.
- `curl -4 ifconfig.me` showed a completely different public IP — the address the internet actually sees — direct evidence of NAT translation happening at the router.
- `lsof -i -n -P` then showed this exact mechanism multiplied across multiple simultaneous apps: Firefox, VS Code, and Claude each holding separate IPv4 connections on the same shared public IP, distinguished only by different local port numbers (PAT in action). In parallel, apps using IPv6 (Claude, VS Code, Firefox on some connections) bypassed NAT entirely, connecting with globally routable addresses of their own — confirming IPv6 doesn't need NAT the way IPv4 does.
- Also spotted `rapportd`, a macOS system service, using purely link-local (`fe80::`) IPv6 addresses to talk directly to another Apple device on the same local network — no router or internet involvement at all, a useful contrast against the routed/NAT'd traffic seen elsewhere in the same output.

This ties directly back to Day 13's TCP capture (which showed a specific source port in use) and Day 9's router dual-addressing (private vs. public identity) — the full picture of how a single device's traffic gets identified, translated, and routed is now backed by live evidence rather than just diagrams.