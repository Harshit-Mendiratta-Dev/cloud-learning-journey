# Day 67 - Review: Re-reading Days 16-20
**Date:** 2026-10-22
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 16-20, line by line.**

**Revisited:**
* Subnetting from first principles: host bits give the block size, and blocks are tiled from 0.
* Identifying a subnet's network, broadcast, and usable range, with the network and broadcast addresses excluded from hosts.
* Nmap scan types: the default connect scan compared with a SYN scan (`-sS`), and what open, closed, and filtered port states mean.
* The TCP handshake as the reason each Nmap state looks the way it does.
* WHOIS: registrant versus registry, and why domain locks and typosquatting registrations exist.

**Still to firm up:**
* The `/24` host count from Day 64, which is still a drill.
* Working a `/26` or `/28` block end to end without a pause.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ipcalc 192.168.10.0/26        # check network, broadcast, and host range (if installed)
nmap -sS -v scanme.nmap.org   # SYN scan from Day 18, for comparison only
whois example.com             # registry and registrant details from Day 20
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 16-20 build the subnetting and scanning skills that the port-level troubleshooting depends on.
* **Blocker:** the subnetting block-boundary step is the one that tripped up Day 16. It's worth working one problem slowly.