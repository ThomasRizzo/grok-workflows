---
name: rp2350-system-bus
description: "RP2350 System bus lookup for part rp2350, outline 2, PDF pages 23-32 of RP-008373-DS-3. Use for AHB5 crossbar, upstream and downstream ports, bus priority, security filters, atomic aliases, APB timing, address-map bases, or BUSCTRL. Trigger with /rp2350-system-bus."
---

# RP2350 system bus

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF pages: 23-32 (outline 2. System bus). Printed footers on these pages are not the PDF index.

Run `pdftotext -layout -f 23 -l 32` on that PDF. The range is under 25 pages; if you split it, use windows of at most 25 pages. Answer only from that extract. Paraphrase; cite the PDF page index. Do not copy register maps or tables into the reply.

This section is the fabric and the coarse address map. For block detail, use a sibling only when the question needs it:

- `/rp2350-memory` for SRAM, ROM, and XIP contents beyond the bases below
- `/rp2350-sio` for the core-local SIO ports
- `/rp2350-access-control` for ACCESSCTRL lists that the bus filters enforce
- `/rp2350-sysctrl` for the BUSCTRL register block (priority and performance counters)
- `/rp2350-dma` for the DMA read and write managers
- `/rp2350-cortex-m33` and `/rp2350-hazard3` for the two cores’ instruction and data ports and for PPB versus bus faults

## Facts read from pages 23-32

- Heading on PDF page 23: 2. System bus; subsections 2.1 Bus fabric and 2.2 Address map.
- Main AHB5 crossbar: 6 upstream ports, 17 downstream ports, 32-bit paths, up to 6 transfers per cycle.
- Managers: Core 0 I and D, Core 1 I and D, DMA read, DMA write. SIO is a separate per-core load/store path, not a crossbar manager.
- Symmetric subordinates: Boot ROM (1), XIP (2, striped), SRAM (10, striped). Fast AHB peripherals share one port; APB bridge is one more port. Instruction fetch from peripherals is disconnected.
- Priority: BUSCTRL `BUS_PRIORITY` for core 0, core 1, DMA read, DMA write. Level 1 before level 0; same level is round-robin. At 150 MHz, stated max sustained bandwidth is 3.6 GB/s.
- APB bridge: minimum 3 cycles for a read and 4 for a write. AHB peripheral segment: minimum one cycle, up to one wait state.
- Atomic aliases per 4 kB register block: +0x0000 normal, +0x1000 XOR, +0x2000 set, +0x3000 clear (16 kB total). I2C, UART, SPI, and SSI use a bus interposer (two extra cycles). SIO, CoreSight self-hosted window, standard M33 PPB, and OTP SBPI do not use this alias scheme.
- Four 24-bit saturating counters `PERFCTR0` onward; read and clear `BUSCTRL_PERFCTRx`; event select `BUSCTRL_PERFSELx` (20 events). See outline 12.15.4.
- Coarse decode on address bits 31:28. Segment bases: ROM `0x00000000`, XIP `0x10000000`, SRAM `0x20000000`, APB `0x40000000`, AHB `0x50000000`, SIO `0xd0000000`, Cortex-M33 PPB `0xe0000000`. Unmapped ranges bus-error.
- `BUSCTRL_BASE` `0x40068000`. Also `SIO_BASE` `0xd0000000`, `SIO_NONSEC_BASE` `0xd0020000`, `PPB_BASE` `0xe0000000`, `EPPB_BASE` `0xe0080000`.
- SRAM0-3 and SRAM4-7 striped on address bits 3:2. `SRAM_BASE` `0x20000000`, `SRAM4_BASE` `0x20040000`, `SRAM_STRIPED_END` `0x20080000`, `SRAM8_BASE` `0x20080000`, `SRAM9_BASE` `0x20081000`, `SRAM_END` `0x20082000`.
- XIP mirrors: `XIP_BASE` `0x10000000`, `XIP_NOCACHE_NOALLOC_BASE` `0x14000000`, `XIP_MAINTENANCE_BASE` `0x18000000`, `XIP_NOCACHE_NOALLOC_NOTRANSLATE_BASE` `0x1c000000`. Lower 32 MB is QMI; `XIP_SRAM_BASE` is gone.

Peripheral base lists (APB Table 13, AHB Table 14) stay in the PDF extract. Do not paste them here.
