# Day 64 - Recall Quiz: Days 1-10
**Date:** 2026-10-19
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Quiz results (five questions, from memory):**

| Q | Topic | Result | Correction |
|---|---|---|---|
| 1 | OSI layers | Correct | Listed 1 to 7; the question asked for 7 to 1. Same content, reverse order. |
| 2 | HTTPS transport and port | Correct | TCP, port 443 |
| 5 | Tool for checking a remote TCP port | Correct | Several work: `telnet`, `nc -vz`, `nmap`, `curl -v`. Choose by purpose. |
| 6 | Usable hosts in a `/24` | Wrong (said 8) | 2^8 = 256 addresses, minus network and broadcast = **254** |
| 8 | MAC address in an outbound frame | Partly right | The frame carries the next hop's MAC. Each router **rewrites** the MAC fields at every hop. The destination IP stays fixed end to end. |
| 10 | CNAME | Partly right | A CNAME is a name-to-name alias. It resolves to another name, and the IP comes at the end of the chain (A/AAAA). |

**Two topics to revisit:**
* **Usable hosts in a subnet:** the host bits give the total, then subtract two reserved addresses.
* **CNAME chain:** the CNAME points to another name, not to an IP. The IP comes from the final A or AAAA record.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Quiz day, nothing run.
# Commands for the two topics to revisit:
dig www.example.com            # look for a CNAME line, then the A record that follows it
ipcalc 192.168.10.0/24         # check the usable host count (if installed)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** the quiz showed solid recall of layers, ports, and tool choice. The gaps are arithmetic and chain-following, not concepts.
* **Blocker solved:** I'd remembered "MAC gets added at each hop." The correct version is that the MAC gets **rewritten** at each hop while the IP stays fixed.
* **Mistake to remember:** a `/24` has 254 usable hosts, not 8. Subtract the network and broadcast addresses from the total.