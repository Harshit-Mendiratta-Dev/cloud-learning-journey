# Day 86 - HA and Redundancy, Part 2
**Date:** 2026-11-13
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2 Hours

## 1. Key Concepts Learned 

**Load balancing**
* Several identical servers sit behind one virtual address. The balancer forwards each request to a healthy server.
* Hosts only see the virtual address, so adding or removing servers doesn't change what clients use.
* Methods: **round robin** (take turns) and **least connections** (send to the server with the fewest active connections). Neither one keeps a client's session on one server.
* **Session breaks** when requests bounce between servers, because the session state lives on one server. Fix it with **session affinity** (sticky sessions, usually by cookie or client IP hash) or **shared session storage** that every server can read.
* Health checks: the balancer stops sending traffic to a server that fails them.

**Link aggregation (LACP, also called EtherChannel or port-channel)**
* Two or more physical links between the same two devices are bundled into one logical link.
* Adds capacity and survives one link failing, as long as one member works.
* Traffic is spread by a **hash** of the source and destination (sometimes ports). The hash output picks one member link, and the same input always gives the same output, so one conversation stays on one link and its packets stay in order.
* **Key point:** LACP adds capacity **across flows, not speed for a single flow.** Two 1 Gbps links give 2 Gbps across many clients. One large file transfer stays at 1 Gbps, because every packet hashes to the same link.

**Failover detection**
* A standby only helps once the switch-over is triggered.
* **HSRP and VRRP heartbeat:** the active router sends periodic "I'm alive and active" messages. If the standby doesn't hear them for the hold time, it takes over.
* **Interface tracking:** if the active router's upstream link fails, its priority drops and the standby takes over. This is a separate trigger from the heartbeat.
* **Health checks:** a load balancer removes a failed server from rotation.
* **Split brain:** if the heartbeat is lost but both routers are still running, the standby takes over while the original is still up. Both claim the virtual IP, and the network can misbehave.

**Corrections from today's questions**
* Least connections and round robin don't cause session breaks. The fault is where the session state lives.
* A hash isn't random: the same input always gives the same output, which is why it keeps a flow on one link.
* The heartbeat doesn't carry link state or health-check results. Those are separate triggers.

**Still to firm up**
* Stating the two separate failover triggers (heartbeat timeout and interface tracking) in one line each.
* Explaining split brain without a pause.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Theory day, nothing run.
netstat -nr | grep default      # the gateway IP that HSRP/VRRP would protect
arp -a                          # the cached gateway MAC (virtual MAC if HSRP/VRRP)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** HA has three parts: spread load (balancing), add capacity across links (aggregation), and detect failure fast enough to switch (failover). Each one solves a different problem, and none of them replaces the others.
* **Blocker solved:** a single file transfer doesn't get the bundle's total, because the hash keeps one flow on one link.
* **Mistake to remember:** a heartbeat is a separate signal from link state and health checks. A heartbeat loss with both routers running is split brain, not a clean failover.