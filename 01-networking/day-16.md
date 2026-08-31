# Day 16 - Subnetting Drills (Blocked — To Be Revisited)
**Date:** 2026-08-31
**Focus Area:** Phase 1 - Networking (Subnetting Practice)
**Time Spent:** ~30 Minutes (cut short by work shift)

## 1. Key Concepts Learned (Partial)

* **Subnet mask from CIDR:** For `/26`, 26 bits are set to `1`. The first 24 bits fill three full octets (`255.255.255`), leaving 2 bits in the fourth octet (`11000000` = **192**). So `/26` → subnet mask `255.255.255.192`. **This part I got right on my own.**

* **Host bits vs. subnet bits — this is where I mixed things up:** Quick trick given but not yet internalized: `32 - CIDR number = host bits remaining`. For `/26`: `32 - 26 = 6` host bits. I initially miscounted this and got the usable-host formula wrong as a result.

* **Usable hosts formula:** `2^(host bits) - 2` (the `-2` removes the network address and broadcast address, neither of which can be assigned to a device). For `/26`: `2^6 - 2 = 62` usable hosts. **I got this wrong initially (said 10) and needed correction.**

* **First usable host:** Got this right — network address + 1 (e.g., `192.168.1.0` → first usable host `192.168.1.1`).

* **Block boundaries / broadcast address — did NOT click today.** The concept that each `/26` block is 64 addresses wide, and that blocks tile sequentially (`.0–.63`, `.64–.127`, `.128–.191`, `.192–.255`) confused me. I incorrectly identified `192.168.1.192` (start of a *different* block entirely) as the broadcast address of my own block, instead of the correct answer within `.0–.63`.

## 2. Hands-on Lab & Commands
No terminal commands today — pure manual math drilling on paper/in conversation, no CLI work.

## 3. Key Takeaway / Blocker Solved (Not Solved Yet — Honest Status)

**Genuinely stuck today, not resolved.** Ran out of time before my work shift started, so this is left as an open blocker rather than a false "understood everything" entry. Specific gap to revisit tomorrow: **block boundaries and how to correctly calculate the broadcast address** — I understand the mask and usable-host-count formulas now, but visualizing how subnet blocks tile across the address range (and picking the correct block to work within) is not solid yet.

**Plan for next session:** redo the same `/26` problem from scratch, focus specifically on the block-boundary step before touching anything else. Possibly try a visual/table approach (writing out all four blocks explicitly) rather than doing it in my head.
</br>
This is a fair, honest log — not every day is a clean win, and that's fine.