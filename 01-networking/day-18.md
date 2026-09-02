# Day 18 - Nmap Revisited: Closing the Day 4 Loop
**Date:** 2026-09-02
**Focus Area:** Phase 1 - Networking (Port Scanning)
**Time Spent:** 2.5 Hours

## 1. Key Concepts Learned (Theory)

* **How a SYN scan works, mechanically (built directly on Day 13's TCP handshake knowledge):**
  1. Nmap sends a **SYN** to a target port
  2. **SYN-ACK** received → port is **open** (something's listening, willing to complete a handshake)
  3. **RST** received → port is **closed** (host is reachable, actively refusing — nothing listening there)
  4. **No response at all** → port is **filtered** (a firewall is silently dropping the packet — same "no matching NAT/firewall table entry → dropped" behavior from Day 14)
  5. Nmap **never sends the final ACK** — it learns what it needs from the SYN-ACK alone, then abandons the connection. This is why it's called a "half-open" scan.

* **Scan type depends on privilege level:**
  - Without `sudo`: Nmap can't craft raw SYN packets, so it silently falls back to a **TCP Connect Scan** (`-sT`) — completes the *full* 3-way handshake on every port, then closes it. Slower, and closed ports are reported as `conn-refused` (an OS/socket-level description).
  - With `sudo` + `-sS`: a true **SYN Stealth Scan** runs — half-open, no full handshake, significantly faster. Closed ports are reported as `reset` (the raw TCP flag observed directly, since Nmap is working below the OS socket layer here).

* **`-s<letter>` scan type flag family:**
  - `-sS` = SYN Scan (half-open, needs `sudo`)
  - `-sT` = Connect Scan (full handshake, default fallback without `sudo`)
  - `-sU` = UDP Scan (different technique — UDP has no handshake to exploit)
  - `-sV` = Version detection (identifies the actual software running on an open port)

* **`-v` (verbose) on Nmap:** narrates every internal step — DNS resolution, a preliminary "is the host even alive" ping scan, explicit confirmation of which scan type is running, and real-time port discovery as results come in, rather than waiting for the whole scan to finish.

## 2. Hands-on Lab & Commands (Practical)
```bash
which nmap                          # confirm installed (via Homebrew)

# Default scan — no sudo, silently falls back to Connect Scan
nmap scanme.nmap.org

# Typo led to a different real host (scamme.nmap.org / ack.nmap.org) —
# accidental but useful comparison against a more locked-down server
sudo nmap scamme.nmap.org

# Explicit SYN Stealth Scan, verbose
sudo nmap -sS -v scanme.nmap.org
```

## 3. Key Takeaway / Blocker Solved

**This closes the Day 4 loop.** Back then, an Nmap scan against a private `10.x.x.x` IP was confusing and unexplainable — no understanding yet of TCP states, NAT, or firewalls. Today, with that foundation in place, every result was fully explainable:

- **`scanme.nmap.org`** (no `sudo`) → ran as a slow (82.15s) Connect Scan, `conn-refused` for closed ports — matches theory for privilege-less scanning.
- **`scamme.nmap.org`** (typo, resolved to a real but different, more locked-down host `ack.nmap.org`) → mostly filtered ports, direct evidence that firewall policy varies significantly between hosts — not every server is configured the same way.
- **`scanme.nmap.org` with `-sS -v`** → confirmed, explicitly and in verbose output, a true SYN Stealth Scan: ~8x faster (10.50s vs. 82.15s), closed ports now labeled `reset` (the raw TCP flag, not an OS-level description), and real-time port discovery logged as results arrived.
- **Packet-level proof of "filtered":** `Raw packets sent: 1113 | Rcvd: 1027` — the gap between sent and received directly confirms that filtered ports got a SYN sent but no response of any kind came back, exactly matching the "silently dropped by firewall" theory.