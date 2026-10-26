---
name: rp2350-powman
description: "Lookup for the RP2350 power management (POWMAN) registers, outline 6.4, PDF pages 415-445. Use for POWMAN base address, password writes, power-domain state, brown-out, LPOSC, GPIO wake and EXT_CTRL, AON time/alarm, scratch/boot, and POWMAN interrupts. Trigger: /rp2350-powman."
---

# RP2350 POWMAN registers

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF page range: 415–445 (`pdftotext -f` / `-l` indexes, not the printed footer). Outline: 6.4. On this range the printed footer is one less than the PDF index.

Run `pdftotext -layout` on that range in windows of at most 25 pages. Read 415–426 first (heading, password rule, register list), then 427–445 for bitfields. Answer only from that extract. Paraphrase; cite the PDF page index. Do not paste register tables or bitfield text.

## Facts

- Heading on PDF page 415: 6.4. Power management (POWMAN) registers.
- Single block, base `0x40100000` (`POWMAN_BASE`), always-on address space shared with the core regulator.
- Password `0x5AFE` in bits 31:16. Offsets through `0x0ac` are protected. Unprotected writes: `SCRATCH0`–`SCRATCH7`, `BOOT0`–`BOOT3`, `INTR`, `INTE`, `INTF`.
- Offsets `0x000` `BADPASSWD` through `0x0ec` `INTS`. The list starts on PDF page 415.
- `STATE`: bit 3 switched-core, bit 2 XIP cache, bit 1 SRAM0, bit 0 SRAM1; 0 = powered up, 1 = powered down.
- `PWRUP0`–`PWRUP3`: source 0–47 = GPIO0–47; 48–53 = QSPI SS, SD0, SD1, SD2, SD3, SCLK.
- `EXT_CTRL0` and `EXT_CTRL1` select GPIO 0–30. `EXT_TIME_REF` GPIO choices: 12, 20, 14, 22.
- POWMAN clock: `clk_ref` (fast) or LPOSC (slow), via `SEQ_CFG` `USE_FAST_POWCK` / `USING_FAST_POWCK`.
- Interrupt sources in `INTR`/`INTE`/`INTF`/`INTS`: `VREG_OUTPUT_LOW`, `TIMER`, `STATE_REQ_IGNORED`, `PWRUP_WHILE_WAITING`.

## Siblings

Name only when the question crosses that block:

- `rp2350-core-regulator` — `VREG_*` lives in this address space.
- `rp2350-gpio` — power-up sources and `EXT_CTRL` / `EXT_TIME_REF` pins.
- `rp2350-clocks` — `clk_ref`, LPOSC, and XOSC as POWMAN and timer clocks.
- `rp2350-always-on-timer` — time, alarm, and LPOSC/XOSC frequency registers feed that timer.
- `rp2350-watchdog` — `WDSEL` versus a PSM watchdog reset.
- `rp2350-bootrom` — `BOOTDIS` and `BOOT0`–`BOOT3`.
- `rp2350-resets` — `CHIP_RESET` and the power-on state machine.
- `rp2350-power-reduction` — sleep, dormant, and power domains after this register chapter.
