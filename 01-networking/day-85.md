# Day 85 - HA and Redundancy, Part 1
**Date:** 2026-11-12
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Core idea:** a single point of failure (SPOF) is any one component whose failure takes the service down. HA removes SPOFs so one failure is survivable.

**Common SPOFs, from the user outward:**

| Component | If it fails | Redundancy option |
|---|---|---|
| Home router | Whole house offline | Second router, or standby gateway |
| ISP link | No internet | Second ISP link (dual-homing) |
| Switch | Group of devices loses network | Two switches with redundant links |
| Default gateway IP | Hosts can't leave their subnet | Virtual gateway shared by two routers |

**First-hop redundancy (HSRP and VRRP):**
* Two routers share one **virtual IP** (for example `192.168.1.1`), which every host uses as its gateway.
* One router is **active** and answers for the virtual IP. The other is **standby**.
* The active router uses a **virtual MAC**. Hosts ARP for the virtual IP once and cache the virtual MAC.
* On failure, the standby takes over the virtual IP and MAC. The hosts' ARP cache stays valid because the mapping hasn't changed.
* **Key point:** the virtual MAC doesn't belong to either physical router. That's the difference from Day 62's stale-ARP case, where a real replacement device had a different MAC.

**STP and HA (Day 28):**
* STP's job is **loop prevention**. Failover is a side effect: when the active link fails, the blocked path is unblocked.
* Keep the purpose and the behaviour separate in notes.

**My own home network:**
* **Router:** single point of failure, one device for the whole house.
* **Fiber line:** single point of failure, one physical path from the pole to the router. A cut by a squirrel or wind takes the whole network offline.
* **Difference:** a router fault is fixed by replacing or rebooting the device. A fiber cut needs the ISP's repair crew, so it can take hours or days.
* **Realistic backup:** a mobile hotspot covers the fiber case, though it doesn't fail over automatically.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Theory day, nothing run.
netstat -nr | grep default      # the gateway IP hosts depend on (the SPOF in question)
arp -a                          # the cached gateway MAC (virtual MAC if HSRP/VRRP)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** HA is about removing single points of failure. Each fix targets one component, so a second router doesn't help with a fiber cut.
* **Blocker solved:** STP is loop prevention, and its failover is a side effect, not its purpose.
* **Mistake to remember:** the virtual MAC in HSRP/VRRP is shared, not a real device's MAC, which is why hosts' ARP entries survive failover.
* **Next:** Day 86, HA Part 2 (load balancing, link aggregation, failover detection).