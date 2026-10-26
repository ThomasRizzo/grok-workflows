---
name: rp2350-sysctrl
description: "Look up RP2350 System Control Registers (outline 12.15, PDF pages 1165-1180): SYSINFO chip ID and package, SYSCFG sync bypass, debug force, and SRAM power, TBMAN, and BUSCTRL priority and fabric counters. Trigger on SYSINFO, SYSCFG, TBMAN, BUSCTRL, CHIP_ID, PACKAGE_SEL, MEMPOWERDOWN, bus priority, JEDEC ID, or /rp2350-sysctrl."
---

# RP2350 System Control Registers

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF page range: 1165–1180 (`pdftotext -f` / `-l` indexes, not the printed footer). Printed footer on these pages is the PDF index minus 1.
Outline: 12.15 System Control Registers.

Run `pdftotext -layout` on that range, in windows of at most 25 pages (this range fits in one window). Answer only from that extract. Paraphrase; cite the PDF page index. Do not paste register tables or datasheet sentences into the answer.

Bus priority is the scheme in section 2.1.1; use `rp2350-system-bus` for that. `MEMPOWERDOWN` does not remove SRAM array power; that is the Power Manager, `rp2350-powman`. GPIO input synchroniser bypass feeds the SIO GPIO input register (`rp2350-sio`); pin indexes are the GPIO block (`rp2350-gpio`). The NMI mask moved to the Cortex-M33 EPPB (`rp2350-cortex-m33`, section 3.7.5.1). `DBGFORCE` drives the on-chip SWD port (`rp2350-debug`).

Facts (confirm in the extract; each is one short lookup, not a description):

- Heading `12.15. System Control Registers` is on PDF page 1165. Next outline `13. OTP` starts on PDF page 1181.
- Four blocks, one instance each: SYSINFO, SYSCFG, TBMAN, BUSCTRL.
- `SYSINFO_BASE` `0x40000000`. Offsets: `CHIP_ID` `0x000`, `PACKAGE_SEL` `0x004`, `PLATFORM` `0x008`, `GITREF_RP2350` `0x014` (PDF page 1165).
- `CHIP_ID` reset: `REVISION` `0x3`, `PART` `0x0004`, `MANUFACTURER` `0x493`, `STOP_BIT` `0x1` (PDF page 1166).
- `PACKAGE_SEL`: 0 = QFN80, 1 = QFN60 (PDF page 1165).
- `SYSCFG_BASE` `0x40008000`. Offsets: `PROC_CONFIG` `0x000`, `PROC_IN_SYNC_BYPASS` `0x004`, `PROC_IN_SYNC_BYPASS_HI` `0x008`, `DBGFORCE` `0x00c`, `MEMPOWERDOWN` `0x010`, `AUXCTRL` `0x014` (PDF page 1167).
- Sync bypass: GPIO 0–31; `_HI` is GPIO 32–47, USB GPIO 56–57, QSPI GPIO 58–63 (PDF page 1167).
- `TBMAN_BASE` `0x40160000`. Only `PLATFORM` at `0x000` (PDF page 1170).
- `BUSCTRL_BASE` `0x40068000`. `BUS_PRIORITY` `0x000`, `BUS_PRIORITY_ACK` `0x004`, `PERFCTR_EN` `0x008`, then `PERFCTRn`/`PERFSELn` for n = 0..3 at `0x00c`–`0x028` (PDF page 1171).
- Four 24-bit saturating fabric counters. `PERFSEL` event numbers run 0–67 (PDF pages 1171–1180).
- Bus-priority controls implement section 2.1.1 (PDF page 1171).
- NMI mask moved to EPPB section 3.7.5.1. Full SRAM power removal is Power Manager section 6 (PDF page 1167).
