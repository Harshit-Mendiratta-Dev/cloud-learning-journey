# Day 95 - Review: HA and Redundancy, Part 2 (Re-read)
**Date:** 2026-11-23
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours

## 1. Key Concepts Learned 

**Re-read the Day 86 and Day 90 notes on HA Part 2, line by line.**

**Revisited:**
* **Load balancing:** several identical servers behind one virtual address. The balancer forwards each request to a healthy server, and clients only see the virtual address.
* **Balancing methods:** round robin (take turns), and least connections (send to the server with the fewest active connections).
* **Health checks:** the balancer removes a failing server from rotation until it passes again.
* **Session problem:** a login on one server is unknown to another, so the user looks logged out. Fix it with session affinity (one client to one server) or shared session storage (every server reads the same store).
* **LACP:** bundles two or more physical links into one logical link. It adds capacity across flows and survives one member failing.
* **Hashing:** the hash of source and destination picks the member link. The same input always gives the same output, so one flow stays on one link. A single large transfer stays at 1 Gbps.
* **Failover triggers:** the heartbeat (HSRP and VRRP), interface or link tracking (priority drops when the upstream link fails), and health checks.
* **Split brain:** a lost heartbeat with both routers still running. Both claim the virtual IP, so priority and tie-breakers are part of the design.

**Still to firm up:**
* Stating why a single flow gets one link's speed, in one line.
* Naming the three failover triggers without a pause.

## 2. Hands-on Lab & Commands 
```bash
# Review day, nothing run.
netstat -nr | grep default      # the gateway HSRP/VRRP would protect
arp -a                          # the cached gateway MAC (virtual MAC if HSRP/VRRP)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** HA Part 2 is three parts that each remove a different single point of failure: the single server, the single link, and the failure that goes undetected.
* **Blocker:** the difference between "capacity across flows" and "speed for one flow" is the point that blurs.
* **Next:** Day 96, the re-read of Days 64-69 (the recall quiz and first re-reads), then IPv6 depth and QoS.