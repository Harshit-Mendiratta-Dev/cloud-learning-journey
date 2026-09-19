# Day 35 - Physical Media & Cabling
**Date:** 2026-09-19
**Focus Area:** Phase 1 - Networking (Physical Layer)
**Time Spent:** 1.5 Hours

## 1. Key Concepts Learned (1.5h Theory)

* **Copper (twisted pair) vs. Fiber optic:**
  - **Copper** — electrical signals over metal wires; cheaper, easier to install; susceptible to **EMI (Electromagnetic Interference)**; effective distance limited to roughly 100 meters before signal degrades.
  - **Fiber optic** — light pulses through glass/plastic strands; much faster, far longer distances before needing a signal boost, completely immune to EMI (no electrical interference possible with light); more expensive, more fragile, requires specialized tools/skill.

* **Twisted pair cable categories:** Cat5e (up to 1 Gbps), Cat6 (up to 10 Gbps over shorter distances), Cat6a (10 Gbps over full 100m), Cat7/Cat8 (higher speeds, individually shielded pairs, mostly data-center use). The "twisted" design is functional, not cosmetic — pairs are twisted specifically to cancel electrical interference between them (differential signaling).

* **Connectors:** RJ45 (standard Ethernet connector, used constantly without previously knowing the name), LC/SC (common fiber optic connector types).

* **Straight-through vs. crossover cables:** Straight-through connects *different* device types (PC to switch, switch to router) with identical wiring on both ends; crossover historically connected *similar* devices directly (PC to PC) with swapped transmit/receive pairs on one end. Mostly obsolete today due to **Auto-MDI-X**, a feature in modern equipment that automatically detects and adjusts for either cable type — still worth knowing conceptually since it explains a real, historically common networking problem.

* **Decoded a real Cat6 cable's printed jacket label, field by field:**
  - `24 AWG` — American Wire Gauge; lower number = thicker wire; 24 AWG is standard for Ethernet cabling
  - `4pr` — 4 twisted pairs (8 individual wires total)
  - `75°C` — temperature rating for continuous safe use
  - `CAT6` — confirms cable category and its speed/distance capability
  - `ETL Verified` — independent lab certification confirming the cable genuinely meets its claimed spec (not just a manufacturer's own claim)
  - `ANSI/TIA-568` — the actual industry standard this cable complies with; TIA-568 defines structured cabling standards including RJ45 wire color-coding order (T568A/T568B pinouts); ANSI is the body formally recognizing it as a national standard

## 2. Hands-on Lab & Commands
No terminal commands today — examined a real, physical Cat6 Ethernet cable's printed jacket label directly (in active use on a separate office PC, not available to test/unplug on the Mac mini due to its age and fragility — a legitimate real-world judgment call, not a missed opportunity, since disturbing an old, already-bent cable risks triggering a failure from unseen internal wire fatigue).

## 3. Key Takeaway / Blocker Solved

No blocker — a genuinely refreshing, concrete day after a stretch of abstract/theoretical topics (VLANs, STP, WAN, dynamic routing). Real hands-on value came from decoding an actual cable label rather than running commands — confirmed a legitimate, properly certified Cat6 cable built to the real industry standard (ANSI/TIA-568), with every printed spec matching directly onto today's theory. Made a sound practical call to leave a working-but-fragile, already-deployed cable undisturbed rather than risk breaking it for the sake of a lab exercise — itself a small, real piece of cabling judgment worth having.