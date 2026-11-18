# Day 91 - Review: Troubleshooting Methodology (Day 45 Revision)
**Date:** 2026-11-18
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Revisited the troubleshooting methodology from Day 45.**

**The seven CompTIA steps:**
1. Identify the problem: gather information, question the user, find what changed.
2. Establish a theory of probable cause.
3. Test the theory. If it fails, form a new theory or escalate.
4. Establish a plan of action.
5. Implement the solution, or escalate if it's outside your access or expertise.
6. Verify full system functionality, and implement preventive measures.
7. Document findings, actions, and outcomes for future reference.

**The three approaches (where to start looking, used in step 2):**
* **Top-down:** start at the application (Layer 7) and move down. Use it when one app or a few users are affected.
* **Bottom-up:** start at the physical layer (Layer 1) and move up. Use it for a total outage, such as a whole office or house offline.
* **Divide-and-conquer (hybrid):** start at Layer 3, test in the middle, then move up if it passes or down if it fails.

**How the steps and approaches relate:** the seven steps say *what to do*. The approaches say *where to start looking*, inside step 2.

**Still to firm up:**
* Naming the seven steps in order without a pause.
* Which approach fits which situation, in one line each.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ping -c 2 127.0.0.1         # Layer 3 sanity check, the start of divide-and-conquer
netstat -nr | grep default  # the gateway, the next test in the middle
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** the method is the same for every scenario from Days 48 to 62. Identify the problem, form a theory, test it, act or escalate, verify, and document.
* **Blocker:** the steps and the approaches are still easy to mix up, since one answers "what to do" and the other "where to start." Keep them as two separate lists.