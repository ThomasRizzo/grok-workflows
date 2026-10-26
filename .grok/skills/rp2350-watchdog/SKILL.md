---
name: rp2350-watchdog
description: "Look up the RP2350 watchdog (outline 12.9, PDF pages 1106-1109): countdown reset, tick input, LOAD/CTRL, scratch registers, and reset selectors. Trigger on watchdog, WATCHDOG_BASE, scratch boot vector, or /rp2350-watchdog."
---

# RP2350 Watchdog

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF page range: 1106–1109 (`pdftotext -f` / `-l` indexes, not the printed footer numbers).
Outline: 12.9 Watchdog.

Run `pdftotext -layout` on that range, in windows of at most 25 pages (this range fits in one window). Answer only from that extract. Paraphrase; cite the PDF page index. Do not paste register tables, SDK listings, or datasheet sentences into the answer.

The countdown uses a tick from the system ticks block (section 8.5), not a tick generator inside the watchdog. Which reset the timeout fires is chosen outside this block: `POWMAN_WATCHDOG` (chip-level), `PSM_WDSEL` (PSM sequence), and `RESETS_WDSEL` (subsystem). Scratch values and the boot magic are read by the bootrom (section 5.2.4). When those details matter, use `rp2350-clocks`, `rp2350-powman`, `rp2350-resets`, and `rp2350-bootrom`.

Facts (confirm in the extract; each is one short lookup, not a description):

- Heading `12.9. Watchdog` is on PDF page 1106.
- One watchdog; base `0x400d8000` (`WATCHDOG_BASE`); register list on PDF page 1109.
- Tick input is the system ticks block, section 8.5 (PDF page 1107).
- External reset selects: `POWMAN_WATCHDOG`, `PSM_WDSEL`, `RESETS_WDSEL` (PDF page 1107).
- Counter load is `LOAD`; live count is `CTRL.TIME` (PDF page 1107).
- Eight scratch registers, `SCRATCH0`–`SCRATCH7`, offsets `0x00c`–`0x028` (PDF page 1109).
- Offsets: `CTRL` `0x000`, `LOAD` `0x004`, `REASON` `0x008` (PDF page 1109).
- `LOAD` maximum `0xffffff`, about 16 seconds (PDF page 1109).
- `CTRL` fields: `TRIGGER` 31, `ENABLE` 30, `PAUSE_DBG1` 26, `PAUSE_DBG0` 25, `PAUSE_JTAG` 24, `TIME` 23:0 (PDF page 1109).
- Scratch clears on a watchdog chip-level reset or `rst_n_run` (RUN pin or DVDD) (PDF page 1107).
- Bootrom checks scratch for a magic number; section 5.2.4 (PDF page 1107).
- This range ends at the `CTRL` bitfield (Table 1359) on PDF page 1109.
