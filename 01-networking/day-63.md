# Day 63 - Video Review: Three Commands, the FIXIT Framework, and Three Troubleshooting Approaches
**Date:** 2026-10-18
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**Source:** Network Troubleshooting for Beginners - 3 commands , 1 framework, 3 methods

**The three bread-and-butter commands:**

| Tool | Layer it tests | What it answers | How the video uses it |
|---|---|---|---|
| `ping` | 3 (Network) | Is the remote device reachable? | First test. No reply means something is broken on the path or the far end. |
| `traceroute` | 3 (Network) | Where does the path stop? | Checks each hop with TTL, showing which hop stops replying. |
| `telnet host port` | 4 (Transport) | Is this service listening on this port? | Checks a specific port, e.g. a database on port 1521. Success means the port is open. Failure can mean a firewall, a closed port, or a service that's down. |

**The FIXIT framework (five steps, iterative):**
1. **Find the problem:** collect data and define the scope. Who is affected? One user, or everyone? One location, or all?
2. **Inspect the symptoms:** narrow the problem with specifics. Does it happen only at peak hours? Only on mobile? The answer changes the diagnosis.
3. **Exclude possibilities:** rule out causes with an open mind, knowing you could be wrong. The video's example: if login fails only at peak hours, the port and router are less likely, and server load is more likely.
4. **Implement a fix hypothesis:** design a test that proves or disproves the cause. Example: ask five users to access the same server at peak time. If all five fail, load is likely. If only one fails, test that user on another server. If the hypothesis is wrong, form a new one.
5. **Track it:** document the fix and monitor it under different conditions, so the next occurrence doesn't start from zero.

The framework is iterative. You loop through steps 3-4 until the cause is narrowed to a specific point.

**The three troubleshooting approaches (OSI-based):**

| Approach | Starts at | Best for | Who usually uses it |
|---|---|---|---|
| Top-down | Layer 7 (application), moves down | An issue persisting in one application, or affecting one user or set of users | Application-side people |
| Bottom-up | Layer 1 (physical), moves up | A total outage, such as a whole office or house offline | Network specialists |
| Hybrid | Layer 3, moves up or down | Anything in between. Ping one server: no reply means the server, but a reply from another server on the same subnet means the routers are fine. | Intermediate users |

**Hybrid logic from the video:**
* If one server fails but others on the same subnet work, the path and routers are probably fine. Look up the stack (Layers 4-7) at that server.
* If the whole subnet is unreachable, look down (Layers 1-2).

**Connecting this to my Day 62 table:**
* `ping` and `traceroute` are Layer 3 tools, matching the table.
* `telnet host port` tests Layer 4 (a port connection), matching the table's "Can a connection to this port form?"
* The video doesn't cover `dig` or `curl`. Those cover Layers 5-7, which I'd tested separately.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Video day, nothing run. Commands the video covered:
ping <ip>                 # Layer 3: reachable?
traceroute <ip>           # Layer 3: where does the path stop?
telnet <ip> <port>        # Layer 4: is this port open and listening?
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** start with the three commands to get data, use FIXIT to narrow the cause, and pick an approach (top-down, bottom-up, or hybrid) based on whether the problem is one app or a whole network.
* **Blocker solved:** the layer-to-tool mapping is now connected. Ping and traceroute test Layer 3, and telnet tests a port at Layer 4.
* **Mistake to remember:** "ping fails" alone isn't a conclusion. The video frames it as the first data point, not the answer.
* **Connection to earlier days:** the hybrid approach is the divide-and-conquer method from Day 45. Both start in the middle at Layer 3.
* **Open question for next session:** the stale ARP entry after a router replacement (from Day 62).