---
name: rp2350-cortex-m33
description: "Lookup for the RP2350 Cortex-M33 processor, outline 3.7, PDF pages 117-226. Use for core count, Armv8-M config, NVIC and 52 IRQs, MPU/SAU, UNCROSS_I_D, PPB_BASE, EPPB_BASE, CTI, WIC/sleep, or the coprocessor interface. Trigger: /rp2350-cortex-m33."
---

# RP2350 Cortex-M33 processor

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF pages 117–226. Outline 3.7. Cortex-M33 processor.

Run `pdftotext -layout` on that PDF, in windows of at most 25 pages (117–141, 142–166, 167–191, 192–216, 217–226). Read 117–128 first for overview and configuration. The Arm register list starts on page 136; the Raspberry Pi EPPB list is on page 226. Answer only from the extract you just made. Paraphrase; cite the PDF page index, not the printed footer. Page 117 still ends 3.6.4 before the 3.7 heading. Page 226 starts 3.7.5.1; 3.8 begins on page 227 and is out of range. Do not paste the register map.

Depends on `/rp2350-m33-coprocessors` (CPIF accelerators; single-precision FPU is in this core), `/rp2350-system-bus` (C-AHB and S-AHB into the AHB5 crossbar), `/rp2350-memory` (code versus SRAM routing), and `/rp2350-debug` (SWD and chip-level CoreSight; PPB debug registers stay in this section). SLEEP, DEEPSLEEP, and WIC are described here; chip power sequencing is `/rp2350-powman`. Core warm reset of the EPPB block is `/rp2350-resets`.

Facts:

- Two Cortex-M33 cores; each has its own NVIC; the same interrupts go to both (page 118)
- Single-precision FPU, DSP, SECEXT, CPIF (page 117); MPU 8 non-secure and 8 secure; SAU 8 regions; 52 external IRQs; 4 priority bits (page 118)
- Full debug: 4 watchpoints, 8 breakpoints, debug monitor; DWT and ITM; ETM; no MTB (page 118)
- WIC present; WICLINES 55: all external IRQs plus NMI, RVEX, and Debug (page 118)
- UNCROSS_I_D: all fetches use the Code port; loads, stores, and debugger accesses use the System port; the unmodified core sent addresses below `0x20000000` to the Code port (page 118)
- Architectural clock gating for SLEEP and DEEPSLEEP; power gating is not supported (page 118)
- RAR: reset all registers on power up (page 118)
- PPB `0xE0000000`–`0xE00FFFFF`; on-core `0xE0000000`–`0xE0043FFF`; EPPB APB `0xE0044000`–`0xE00FFFFF` (page 124)
- 3.7.5 register list, Table 127, starts on page 136; SDK `PPB_BASE` is `0xe0000000`; offsets run through page 225
- CTI `CTICONTROL` at `0xE0042000` (page 135)
- 3.7.5.1 EPPB, SDK `EPPB_BASE` `0xe0080000`, one copy per core, warm-reset; `NMI_MASK0` +`0x000` (IRQ 0–31), `NMI_MASK1` +`0x004` (IRQ 32–51), `SLEEPCTRL` +`0x008` (page 226)
