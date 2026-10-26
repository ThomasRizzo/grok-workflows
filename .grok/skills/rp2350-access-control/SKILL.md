---
name: rp2350-access-control
description: "RP2350 ACCESSCTRL access control (outline 10.6, PDF pages 740-784): use for GPIO_NSMASK, bus-endpoint permissions, LOCK, FORCE_CORE_NS, CFGRESET, the 0xacce write key, and which manager (core 0, core 1, DMA, debugger) may touch a peripheral or memory window. Trigger on ACCESSCTRL, Secure/Non-secure GPIO or bus filters, or /rp2350-access-control."
---

# RP2350 access control

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

Outline 10.6. PDF pages 740–784. The XIP_AUX bit table continues on PDF page 785, then outline 10.7 starts; include 785 only to finish that table.

Run `pdftotext -layout` on that range in windows of at most 25 pages. Suggested windows: 740–751 (overview and Table 944), then 752–776 and 777–785 (per-register tables). Answer only from the extract. Paraphrase and cite the PDF page. Do not paste register tables or datasheet sentences into the answer.

Open a sibling skill only for the dependency named here, not for a generic peripheral question: `rp2350-gpio` (NS mask filters IO and pad banks and function select), `rp2350-sio` (Non-secure SIO GPIO bits; SIO has no ACCESSCTRL filter), `rp2350-m33-coprocessors` (GPIO coprocessor NSACR/CPACR), `rp2350-resets` (RESETS access and per-block reset controls follow the endpoint), `rp2350-dma` (DMA manager bit and DREQ disconnect), `rp2350-pio` (PIO0–PIO2, cross-PIO, Secure-only GPIO), `rp2350-cortex-m33` and `rp2350-hazard3` (SP/SU versus NSP/NSU, and FORCE_CORE_NS), `rp2350-system-bus` (AHB5 managers), `rp2350-bootrom` (CFGRESET on the boot path), `rp2350-memory` (ROM, SRAM, and XIP windows are endpoints defined there).

Facts read from this PDF (names, counts, addresses, pages only):

- Heading on PDF page 740: 10.6. Access control
- One instance; base `0x40060000` (`ACCESSCTRL_BASE`); Table 944 on PDF page 744
- 59 registers, offsets `0x000`–`0x0e8`; last name `XIP_AUX` (bits finish on PDF page 785)
- `LOCK` `0x000`, `FORCE_CORE_NS` `0x004`, `CFGRESET` `0x008`, `GPIO_NSMASK0` `0x00c`, `GPIO_NSMASK1` `0x010`
- LOCK bits: CORE0(0), CORE1(1), DMA(2, RO, reset 1), DEBUG(3); sticky until a full ACCESSCTRL reset
- `FORCE_CORE_NS.CORE1` is bit 1; `CFGRESET` is bit 0 and does not clear LOCK or FORCE_CORE_NS
- `GPIO_NSMASK0`: GPIO0–31; `GPIO_NSMASK1`: GPIO32–47 plus QSPI and USB DP/DM; NS IRQs `IO_IRQ_BANK0_NS`, `IO_IRQ_QSPI_NS` (PDF page 742)
- Each bus filter: 8 bits DBG, DMA, CORE1, CORE0, SP, SU, NSP, NSU
- Write key `0xacce` in bits 31:16 on every ACCESSCTRL register except `GPIO_NSMASK0` and `GPIO_NSMASK1`
- No bus-filter register for Cortex-M PPB, SIO, ACCESSCTRL, or Boot RAM (PDF page 744)
- Endpoints `ROM` `0x014` through `XIP_AUX` `0x0e8` include SRAM0–SRAM9 and PIO0–PIO2
- Detail tables: Table 945 on PDF page 751 through Table 1003 on PDF pages 784–785
