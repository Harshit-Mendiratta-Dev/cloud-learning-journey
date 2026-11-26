# Day 98 - Review: Re-reading Day 40 (Monitoring and Management)
**Date:** 2026-11-26
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the Day 40 notes on network monitoring and management, line by line.**

**Revisited:**
* **SNMP:** a manager polls devices for data, or a device sends a trap when something happens. Polling is the manager asking. A trap is the device telling.
* **MIB:** a catalogue of the data a device can report. It's a list of what's available, not a formatting rulebook.
* **OID:** the address of one item inside the MIB.
* **Syslog:** devices push their logs to a central server. The server doesn't pull them.
* **Mac's unified logging:** `log show` reads recent system log entries, which is how the Day 40 session explored local logs.
* **TCP state data (Day 40):** the connection states seen in a live capture of the session's own handshake.

**Still to firm up:**
* Stating polling versus traps in one line, without a pause.
* Explaining why syslog is a push and not a pull.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
log show --last 5m | head -50     # recent system log entries (Day 40)
netstat -an | grep ESTABLISHED    # live TCP connection states (Day 40)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** monitoring has two directions. SNMP can be pulled by a manager or pushed as a trap, and syslog is always pushed by the device.
* **Blocker:** a MIB is easy to confuse with a file format. It's a catalogue of what a device can report.
* **Next:** Day 99, the re-read of Days 64-69 (the recall quiz and first re-reads), then IPv6 depth and QoS.