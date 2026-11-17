# Day 90 - HA and Redundancy, Part 2 (Elaborated)
**Date:** 2026-11-17
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Load balancing**
* **Problem:** one server can't handle all traffic, and it's a single point of failure.
* **Idea:** identical servers sit behind one virtual address. The balancer receives each request and forwards it to a healthy server. Clients only see the virtual address.
* **Methods:** round robin (take turns, simple and fair for similar requests), least connections (send to the server with the fewest active connections, better when request durations vary).
* **Health checks:** the balancer probes each server and removes a failing one from rotation until it passes again.
* **Session problem:** a login on server A is unknown to server B, so the user looks logged out. Fixes: session affinity (sticky sessions, usually by cookie or client IP hash) or shared session storage that every server reads.

**Link aggregation (LACP, also called EtherChannel or port-channel)**
* **Problem:** one link has a bandwidth ceiling, and one cable or port is a single point of failure.
* **Idea:** bundle two or more physical links between the same two devices into one logical link. LACP negotiates the bundle.
* **Gives you:** more capacity across flows (two 1 Gbps links carry about 2 Gbps across many clients), and survival of a member failure at lower capacity.
* **Key point:** one flow stays on one link. A hash of source and destination (sometimes ports) picks the member link, and the same input always gives the same output. A single large transfer stays at 1 Gbps.

**Failover detection**
* **Problem:** a standby does nothing until triggered, and slow detection means a longer outage.
* **Heartbeat:** the active router sends periodic "I'm alive" messages (HSRP and VRRP). If the standby misses them for the hold time, it takes over.
* **Interface tracking:** if the active router's upstream link fails, its priority drops and the standby takes over. A separate trigger from the heartbeat.
* **Health checks:** a load balancer removes a failed server from rotation.
* **Split brain:** heartbeat lost while both routers still run. Both claim the virtual IP. Priority and tie-breakers are part of the design to prevent it.

**The pattern across all three:** load balancing removes the single server, LACP removes the single link and adds capacity, and failover detection makes sure the backup takes over. HA needs all three.

**Still to firm up:**
* Stating the three failover triggers in one line each.
* Explaining split brain without a pause.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Theory day, nothing run.
netstat -nr | grep default      # the gateway that HSRP/VRRP would protect
arp -a                          # the cached gateway MAC (virtual MAC if HSRP/VRRP)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** HA is a design of three parts, not a single device. Each part removes a different single point of failure.
* **Blocker solved:** a single flow doesn't get the bundle's total, because the hash keeps it on one link.
* **Mistake to remember:** a lost heartbeat with both routers running is split brain, not a clean failover.