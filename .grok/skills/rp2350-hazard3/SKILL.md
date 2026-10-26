---
name: rp2350-hazard3
description: "Lookup for the RP2350 Hazard3 processor, outline 3.8, PDF pages 227-309. Use for RISC-V cores, harts, ISA and custom extensions, PMP, Xh3irq, CSRs, the debug module, and instruction timing. Trigger: /rp2350-hazard3."
---

# RP2350 Hazard3 processor

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF page range: 227–309 (`pdftotext -f` / `-l` indexes, not the printed footer). Outline: 3.8. Hazard3 processor.

Run `pdftotext -layout` on that range in windows of at most 25 pages. Suggested windows: 227–238 (overview and ISA start), 239–263 (rest of the instruction reference, memory, PMP, exceptions), 264–288 (debug, custom extensions, cycle counts, configuration, CSR list start), 289–309 (remaining CSR bitfields). Answer only from that extract. Paraphrase; cite the PDF page index. Do not paste instruction listings, cycle-count tables, or CSR bitfields.

PDF 309 still holds the last 3.8.9 identity CSRs, then outline 3.9 starts on the same page. Stop at `3.9. Arm/RISC-V architecture switching`. Do not answer architecture switching from this skill.

## Facts

- PDF 227 heading: `3.8. Hazard3 processor`. Two cores (0 and 1); one hart each; hart IDs 0 and 1.
- M-mode and U-mode only. No Sv32. 3-stage in-order pipeline. `x0` hardwired to 0 (PDF 229).
- Extensions on: RV32I, M, A, C/Zca, Zba, Zbb, Zbs, Zbkb, Zcb, Zcmp, Zicsr, Zifencei. Custom: Xh3power, Xh3bextm, Xh3irq, Xh3pmpm. Zbc off (PDF 279).
- Table 387 (PDF 279): `PMP_REGIONS` 11, `PMP_GRAIN` 3, `NUM_IRQS` 52, `IRQ_PRIORITY_BITS` 4, `BREAKPOINT_TRIGGERS` 4, `MVENDORID_VAL` `32'h00000493`, `MIMPID_VAL` `32'h86fc4e3f`. Source rev `86fc4e3` (PDF 228).
- PMP (PDF 260): regions 0–7 dynamic, 8–10 hardwired, 11–15 OFF. NAPOT or OFF. 32-byte granule. Lowest number wins. E6: field order R, W, X (PDF 261).
- Hardwired U-mode RWX (PDF 280): ROM `0x00000000`–`0x0fffffff` (`PMPADDR8`), peripherals `0x40000000`–`0x5fffffff` (`PMPADDR9`), SIO `0xd0000000`–`0xdfffffff` (`PMPADDR10`). Config in `PMPCFG2`.
- One Debug Module, spec 0.13.2, both cores (PDF 264–265). APB-AP offset `0xa000` in the debug address space; multiply DM register addresses by 4. Four instruction triggers.
- `MIP.MTIP`: SIO RISC-V platform timer, also `SIO_IRQ_MTIMECMP` IRQ 29. `MIP.MSIP`: SIO `RISCV_SOFTIRQ`, one bit per hart (PDF 264). Standard priority: meip > msip > mtip. Spare IRQs 46–51 (PDF 267).
- Preferred `-march` (PDF 228): `rv32ima_zicsr_zifencei_zba_zbb_zbs_zbkb_zca_zcb_zcmp`.
- CSRs are per hart; Zicsr only, not loads/stores. Table 388 starts PDF 281; bitfields through PDF 309. Offsets: `MSTATUS` `0x300`, `MTVEC` `0x305`, `PMPCFG0` `0x3a0`, `PMPADDR0` `0x3b0`, `DCSR` `0x7b0`, `PMPCFGM0` `0xbd0`, `MEINEXT` `0xbe4`, `MSLEEP` `0xbf0`.
- `MSLEEP.DEEPSLEEP` deasserts the processor clock enable in sleep (PDF 308). `h3.block` / `h3.unblock`: PDF 271. Misaligned load/store faults (PDF 238).
- Instruction reference PDF 228–258. Cycle counts PDF 273–278. `MHARTID` matches SIO `CPUID` (PDF 265).

## Siblings

Name only when the question crosses that block:

- `rp2350-sio` — RISC-V platform timer, `RISCV_SOFTIRQ`, `CPUID`, and the hardwired SIO PMP window.
- `rp2350-debug` — SW-DP and self-hosted access to the Debug Module (section 3.5).
- `rp2350-access-control` — per-peripheral M/U assignment under the hardwired peripheral PMP window.
- `rp2350-errata` — RP2350-E6 PMP configuration field order.
- `rp2350-bootrom` — `Xh3bextm` is used by the bootrom; ROM is hardwired PMP region 8.
- `rp2350-memory` — what occupies the ROM, peripheral, and SIO windows named above.
- `rp2350-clocks` — only if the question is which system clock the processor clock-enable gates.
