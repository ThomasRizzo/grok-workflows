---
name: rp2350-always-on-timer
description: "Lookup for the RP2350 always-on timer (AON Timer), outline 12.10, PDF pages 1110-1113. Use for AON Timer, 1 ms tick, LPOSC, XOSC, GPIO tick or 1 Hz sync, alarm, power-up, SET_TIME, READ_TIME, and POWMAN-shared timer writes. /rp2350-always-on-timer"
---

# RP2350 always-on timer

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF pages 1110–1113. Outline 12.10. Always-on timer.

Run `pdftotext -layout -f 1110 -l 1113` on that PDF. The range is under 25 pages; one window is enough. Answer only from that extract. Paraphrase; cite the PDF page index (not the printed footer). Page 1110 still ends the watchdog before 12.10. This range stops in 12.10.6; do not invent a base address, register map, or GPIO pin from later pages.

Depends on `/rp2350-powman` (shared POWMAN register block, power sequencer, power-up on alarm), `/rp2350-clocks` (LPOSC, XOSC, `clk_ref`), and `/rp2350-gpio` (external clock or tick). Tick generation is independent of other timers; do not use `/rp2350-system-timers` for this block.

Facts read from pages 1110–1113:

- One AON Timer; shares the POWMAN register block; writes are 16-bit with key `0x5afe` in the top half
- 64-bit counter aimed at 1 ms ticks; tick generator is independent of other chip tick generators
- Default source is on-chip LPOSC, nominal 32.768 kHz; 6.16 divider starts at 32.768
- `LPOSC_FREQ_KHZ_INT` default 32; `LPOSC_FREQ_KHZ_FRAC` default 0.768; write only if `TIMER.RUN` is 0 or `TIMER.USING_LPOSC` is 0
- Source flags: `TIMER.USING_LPOSC`, `TIMER.USING_XOSC`, `TIMER.USING_GPIO_1KHZ`; 1 Hz sync flag `TIMER.USING_GPIO_1HZ`
- XOSC arrives on `clk_ref`; check `CLK_REF_SELECTED` = `0x4`; 16.16 divider starts at 12000.0; `XOSC_FREQ_KHZ_INT` default 12000, `XOSC_FREQ_KHZ_FRAC` default 0
- Clock into the AON Timer from XOSC must be 2 kHz–29 MHz; do not set `LPOSC_FREQ_KHZ_INT` below 2 kHz
- Secure-only: external clock select, external tick select, `TIMER.PWRUP_ON_ALARM`, and `EXT_TIME_REF.DRIVE_LPCK` (32 kHz in place of LPOSC)
- External 1 kHz: `TIMER.USE_GPIO_1KHZ`, falling edge; pulse and gap longer than one LPOSC period (>31 µs); max 16 kHz
- External 1 Hz sync: `TIMER.USE_GPIO_1HZ`; usable with any tick source and in all power modes
- Time: `TIMER.RUN` loads `SET_TIME_63TO48`, `SET_TIME_47TO32`, `SET_TIME_31TO16`, `SET_TIME_15TO0`; read `READ_TIME_UPPER` then `READ_TIME_LOWER` then upper again; `TIMER.CLEAR` is self-clearing
- Alarm: `ALARM_TIME_63TO48`, `ALARM_TIME_47TO32`, `ALARM_TIME_31TO16`, `ALARM_TIME_15TO0`, `TIMER.ALARM_ENAB`, `TIMER.ALARM`; interrupt fields `INTR.TIMER`, `INTE.TIMER`, `INTF.TIMER`, `INTS.TIMER`
