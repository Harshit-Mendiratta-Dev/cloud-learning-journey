# Day 45 - Troubleshooting Methodology (Part 1: Theory)
**Date:** 2026-09-29
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 
* **CompTIA 7-step method:** (1) Identify the problem, (2) establish a theory of probable cause, (3) test the theory, (4) establish a plan of action, (5) implement the solution or escalate, (6) verify full functionality and implement preventive measures, (7) document findings, actions, and outcomes.
* **Step 5 has a branch:** if the fix is outside my access or expertise, I escalate with my findings.
* **Step 6 has two parts:** verify the fix works, then put preventive measures in place.
* **3 approaches for choosing where to look (step 2):** Bottom-up (start at Layer 1, go up), Top-down (start at Layer 7, go down), Divide-and-conquer (start in the middle, e.g. gateway or 1.1.1.1, then go up or down based on the result).
* **How they relate:** the 7 steps are *what to do*; the 3 approaches are *where to start looking*.

## 2. Hands-on Lab & Commands 
```bash
# No lab today, theory only.
# Planned for Day 46 (layered check on [my-mac]):
ping -c 2 127.0.0.1         # network stack on my own machine
ping -c 2 192.168.x.x       # my own interface
ping -c 2 [home-router]     # gateway
ping -c 2 1.1.1.1           # internet, no DNS involved
dig +short google.com       # DNS
curl -I https://google.com  # application layer (HTTP)
```

## 3. Key Takeaway / Blocker Solved
* **Blocker:** I couldn't tell how the 3 approaches related to the 7 steps, and the video I watched didn't cover them. They are a separate idea used inside step 2.
* **Takeaway:** troubleshooting is a repeatable process, not guessing. Step 7 (documentation) is what makes escalation and future fixes possible.
* **Open question for Day 46:** why start in the middle (gateway or 1.1.1.1) instead of at the bottom? Answer in my own words after the lab.