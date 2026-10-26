---
name: rp2350-system-timers
description: "Look up the RP2350 system timers (outline 12.8, PDF pages 1095-1106) for TIMER0/TIMER1, the 64-bit microsecond counter, four alarms, and tick vs clk_sys. Use when the question is TIMER0_BASE, TIMEHR/TIMELR, ALARM0–3, SOURCE, or /rp2350-system-timers."
---

# RP2350 system timers

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF pages 1095-1106. Outline 12.8. System timers. Heading `12.8. System timers` is on PDF page 1095. `12.9. Watchdog` starts on PDF page 1106 after `INTS`.

Run `pdftotext -layout -f N -l M` on that range, in windows of at most 25 pages. Answer only from that extract. Paraphrase; cite the PDF page. Do not paste register-description tables.

The counter does not run until the clocks tick generator is running (section 8.5): use `rp2350-clocks`. Alarms match only the low 32 bits (~72 minutes). Longer alarms, or a timer that runs in the lowest power state, are the always-on timer (section 12.10): use `rp2350-always-on-timer`. No timer pins in this block.

## Facts

- Two instances, TIMER0 and TIMER1, one per security domain.
- `TIMER0_BASE` `0x400b0000`; `TIMER1_BASE` `0x400b8000`. Register list: outline 12.8.5, PDF page 1100.
- 64-bit counter. Default source is the 1 µs tick. `SOURCE` `0x038` bit 0: 0 tick, 1 `clk_sys`.
- Writes: `TIMELW` `0x004` then `TIMEHW` `0x000`. Latched read: `TIMELR` `0x00c` then `TIMEHR` `0x008`. Raw, no latch: `TIMERAWL` `0x028`, `TIMERAWH` `0x024`.
- Four alarms, `ALARM0`–`ALARM3` at `0x010`–`0x01c`. Each has its own IRQ. Match is `ALARMn == TIMELR`. `ARMED` `0x020` (write-1 clears).
- `INTR` `0x03c`, `INTE` `0x040`, `INTF` `0x044`, `INTS` `0x048`. Alarm bits 0–3. `INTS` detail is on PDF page 1106.
- `PAUSE` `0x030`. `DBGPAUSE` `0x02c`: `DBG0` and `DBG1` reset to 1. `LOCKED` `0x034` blocks writes until reset.
- RP2350-only versus RP2040: second instance, external tick generators, `LOCKED`, and `SOURCE`. Overview on PDF pages 1095-1096.
