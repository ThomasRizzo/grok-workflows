---
name: rp2350-hstx
description: "RP2350 HSTX (high-speed serial transmit, outline 12.11) lookup on datasheet PDF pages 1114-1122. Use for HSTX pins, clk_hstx, DDR rate, FIFO, HSTX_CTRL base, CSR/BIT/EXPAND registers, TMDS expander, or PIO coupled mode. Trigger: /rp2350-hstx."
---

# RP2350 HSTX

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`. PDF pages 1114–1122. Outline 12.11.

Run `pdftotext -layout -f <first> -l <last>` on that range, at most 25 pages per call. Answer only from that extract. Paraphrase; cite the PDF page index (not the printed footer). In this section the printed footer is the PDF index minus 1.

Depends on sibling skills only when the question needs them: `rp2350-clocks` (`clk_hstx`, `CLK_HSTX_CTRL`), `rp2350-gpio` (FUNCSEL on the HSTX pins), `rp2350-dma` (FIFO writes), `rp2350-pio` (coupled mode).

## Facts read this run

- One output-only HSTX; capable pins are GPIO 12–19.
- Clock is `clk_hstx`; maximum 150 MHz; DDR is two bits per pin per cycle (300 Mb/s per pin at that clock).
- Data FIFO is 8 entries × 32 bits (`HSTX_FIFO` on AHB); DMA is one FIFO write per `clk_sys` cycle.
- Control block base `0x400c0000` (`HSTX_CTRL_BASE`); APB access crosses to `clk_hstx`.
- Control list: CSR `0x000`, BIT0–BIT7 `0x004`–`0x020`, EXPAND_SHIFT `0x024`, EXPAND_TMDS `0x028`.
- FIFO register list is section 12.11.8; it is not in PDF pages 1114–1122. Do not invent a FIFO base.
- Coupled mode: `CSR.COUPLED_SEL` picks PIO 0–2; `CLK_HSTX_CTRL.AUXSRC` must be `clk_sys` itself, not a same-frequency source.
- Generated clock: `CSR.CLKDIV` (0 means period 16) and `CSR.CLKPHASE`; a pin uses the clock when `BITx.CLK` is set.
