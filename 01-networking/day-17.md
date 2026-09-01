# Day 17 - Subnetting Rebuilt From Scratch
**Date:** 2026-09-01
**Focus Area:** Phase 1 - Networking (Subnetting Mastery)
**Time Spent:** ~1.5 Hours

## 1. Key Concepts Learned (Rebuilt From First Principles)

* **The core mental model, step by step:**
  1. **Host bits** = `32 - CIDR number` (e.g., `/26` → `32 - 26 = 6` host bits)
  2. **Block width** = `2^(host bits)` — this is the single most important number; every subnet of a given CIDR is exactly this many addresses wide
  3. **Blocks tile starting from 0**, in fixed increments of the block width (e.g., for a 64-wide block: `0, 64, 128, 192`) — a subnet can *only* start at these multiples, never anywhere else
  4. **Find which block an address falls in** by checking which range it lands between
  5. Once the block is known: **network address** = first number in the block, **broadcast address** = last number in the block, **usable range** = everything in between

* **Why the math works (derived, not memorized):** With all host bits set to `0`, the value is the lowest possible in that range (→ network address). With all host bits set to `1`, the value is the highest possible (→ broadcast address). E.g., for 6 host bits: `000000` = 0, `111111` = 63 — so the block spans `0` to `63`, which is `2^6 = 64` total values.

* **Usable hosts vs. total block size:** A block of size `N` (e.g., 64) has `N - 2` *usable* addresses, since the first (network) and last (broadcast) values in the block are reserved and can't be assigned to a device.

* **Common failure mode identified:** if one block's range is written even slightly wrong, every subsequent block drifts too, since each one is calculated relative to the last. Fix: always verify `end - start + 1 = block width` before moving to the next block.

## 2. Hands-on Lab & Commands
No terminal commands — pure manual subnetting drills, worked through by hand across three problems of increasing difficulty:
* `192.168.1.130/26` → solved cleanly, first attempt
* `10.0.5.75/28` → block boundary listing had an early slip (off-by-one on a range), corrected mid-problem, final answer accurate after fix
* `172.16.4.100/29` → solved fully correctly, including skipping ahead to the correct block (13) via direct multiplication rather than listing every intermediate block

## 3. Key Takeaway / Blocker Solved

**Yesterday's Day 16 blocker (block boundaries / broadcast address) is now resolved.** The fix wasn't more formulas — it was rebuilding the concept visually from binary first principles: seeing *why* all-zeros-in-host-bits gives the network address and all-ones gives the broadcast address, rather than just applying a memorized rule.

Solved three subnetting problems across three different CIDR notations (`/26`, `/28`, `/29`) with increasing independence — the first needed no correction, the second had one minor early-block listing error that self-corrected without affecting the final answer, and the third was solved cleanly including a shortcut (direct multiplication to jump to the correct block rather than listing every one). This confirms the mental model generalizes across CIDR values, not just memorized for one specific case from yesterday's failed attempt.