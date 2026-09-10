# Day 25 - Wireless Networking: 802.11, Bands, Channels & Security
**Date:** 2026-09-09
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2.0 Hours

## 1. Key Concepts Learned (1.5h Theory)

* **802.11** — the IEEE standard family for wireless networking (Wi-Fi). Each letter suffix (a/b/g/n/ac/ax) represents a generational evolution, generally trading up for faster speeds and better efficiency. Modern naming simplifies this to **Wi-Fi 4/5/6/6E/7** (mapping to 802.11n/ac/ax/ax-6GHz/be respectively) — both naming systems are still used interchangeably in the real world.

* **Frequency bands — the core trade-off:**
  - **2.4 GHz** — longer range, better wall penetration, but slower and heavily congested (shared with Bluetooth, microwaves, and many other devices)
  - **5 GHz** — faster, less congested, but shorter range and weaker wall penetration
  - **6 GHz** (Wi-Fi 6E/7 only) — newest, least congested, shortest range of all

* **Channels** — each frequency band is divided into numbered channels. Nearby devices sharing the same or overlapping channel cause interference — a very common, real cause of "slow Wi-Fi" complaints.

* **Security protocol evolution:**
  - **WEP** (Wired Equivalent Privacy) — old, cryptographically broken, should never be used
  - **WPA / WPA2** (Wi-Fi Protected Access) — WPA2 has been the long-standing modern standard
  - **WPA3** — newest, fixes several WPA2 weaknesses (notably around password-guessing resistance)
  - **Personal vs. Enterprise mode:** "Personal" uses one shared passphrase for everyone; "Enterprise" uses individual per-user login credentials (common in corporate environments)

* **Signal metrics, read from a real capture:**
  - **dBm (decibel-milliwatts)** — logarithmic unit; counterintuitively, values *closer to 0* are stronger (e.g., -52 dBm is much better than -80 dBm)
  - **RSSI (Received Signal Strength Indicator)** — the signal strength reading itself
  - **SNR (Signal-to-Noise Ratio)** — the gap between signal and noise readings; often a more meaningful diagnostic number than signal strength alone, since strong signal next to strong noise can still perform poorly
  - **MCS Index (Modulation and Coding Scheme)** — a technical index representing how efficiently data is being encoded over the link based on current signal conditions; higher generally means better conditions being exploited for higher throughput
  - **PHY Mode** — which 802.11 standard (a/b/g/n/ac/ax) the current connection is actively using

## 2. Hands-on Lab & Commands (0.5h Lab)
```bash
system_profiler SPAirPortDataType
```
(Note: data type name is case-sensitive — `SPAirPortDataType`, not `SPAirportDataType`.)

## 3. Key Takeaway / Blocker Solved

Blocker Solved: First attempt failed silently due to incorrect capitalization (`SPAirportDataType` vs. the correct `SPAirPortDataType`) — `system_profiler` data type names are case-sensitive.

Read a real, live capture of my own Wi-Fi connection: connected via 802.11ac (Wi-Fi 5), 5GHz, Channel 149, WPA2 Personal security, with an excellent Signal/Noise reading (-52 dBm / -93 dBm, roughly 41 dB SNR).

The most valuable finding was in the **"Other Local Wi-Fi Networks"** section — direct, real evidence of channel congestion in a dense area: three separate neighboring networks all crammed onto 2.4GHz Channel 1, and one neighbor sharing the exact same 5GHz channel (149) as my own connection. This is a textbook, real-world cause of Wi-Fi slowdowns — and exactly the kind of finding a Cloud/IT Support engineer would use to diagnose a "my Wi-Fi is slow" ticket, tracing it to channel congestion rather than a fault in the user's own setup.

Redacted all neighboring network names (SSIDs) before sharing output, consistent with the established personal-data policy for the public repo — MAC addresses and specific network identifiers masked, technical findings (channels, security types, signal readings) kept intact since they carry the educational value without exposing identifying information.