# Day 99 - Review: Spanning Tree Protocol (Day 28)
**Date:** 2026-11-27
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Revisited STP from Day 28, using the topology-tracing method from that session.**

**Why STP exists:**
* Redundant links between switches are good for HA, but without STP they form a loop.
* A broadcast in a loop never dies. Each switch floods it to the others, which floods it back, and the network saturates. This is a **broadcast storm**.

**How STP prevents it:**
* Switches exchange BPDUs (bridge protocol data units) and agree on one **root bridge**.
* The root bridge is elected by the lowest **Bridge ID**, which is priority first, then MAC address.
* Every other switch calculates its shortest path to the root. Ports on that path forward. Ports on redundant paths are **blocked**.

**Failover:**
* A blocked link stays in place as insurance. If the active link fails, STP recalculates and unblocks the standby path.
* This is the failover side effect from Day 85. STP's purpose is loop prevention.

**Topology-tracing method (Day 28):**
* Work out the root first, then compute each switch's shortest path to it, then mark the blocked ports.
* Day 28 traced a full seven-switch topology by hand this way.

**Still to firm up:**
* Stating the Bridge ID order (priority, then MAC) without a pause.
* Explaining why the blocked link is kept, rather than removed.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run. STP runs on switches, so there's no Mac-side command here.
arp -a      # MAC entries the Mac has learned, for comparison with a switch's MAC table
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** STP removes loops by blocking redundant paths, and failover is what happens when the active path fails.
* **Blocker:** Bridge ID is the detail that blurs. Priority comes first, then MAC, and the lowest value wins the root election.