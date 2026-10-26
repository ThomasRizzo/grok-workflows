---
name: rp2350-i2c
description: "Look up the RP2350 I2C block (outline 12.2, PDF pages 893-943): I2C0/I2C1, bases, SCL/SDA, clk_sys, FIFOs, master/slave, DMA, and DW_apb_i2c registers. Trigger on I2C, SCL, SDA, IC_CON, IC_DATA_CMD, or /rp2350-i2c."
---

# RP2350 I2C

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF page range: 893–943 (`pdftotext -f` / `-l` indexes, not the printed footer numbers).
Outline: 12.2 I2C.

Run `pdftotext -layout` on that range in windows of at most 25 pages. Use 893–917, then 918–942, then 943. Answer only from that extract. Paraphrase; cite the PDF page index. Do not paste register tables or datasheet sentences into the answer.

Pins are chosen in the GPIO function select (section 9.4), and the block is clocked from `clk_sys`. It can handshake with DMA. When those details matter, use `rp2350-gpio`, `rp2350-clocks`, and `rp2350-dma`.

Facts (confirm in the extract; each is one short lookup, not a description):

- Heading `12.2. I2C` on PDF page 893.
- Instance count: 2 (I2C0, I2C1).
- I2C0 base `0x40090000`; I2C1 base `0x40098000` (PDF page 915; `I2C0_BASE`, `I2C1_BASE`).
- Clock name: `clk_sys` (`ic_clk` is the same clock).
- Pin functions: SCL and SDA, one GPIO pair per controller (section 9.4).
- IP name: Synopsys DW_apb_i2c v2.03a; default slave address `0x055`.
- FIFO counts: 16 transmit, 16 receive.
- Register list: Table 1108, PDF page 915; `IC_CON` `0x000`, `IC_DATA_CMD` `0x010`, `IC_ENABLE` `0x06c`, `IC_DMA_CR` `0x088`, `IC_COMP_TYPE` `0x0fc`.
- DMA register names: `IC_DMA_CR`, `IC_DMA_TDLR`, `IC_DMA_RDLR` (PDF pages 917–918).
- Speed names: standard, fast, fast-mode plus; not high-speed; not SMBus or PMBus (PDF pages 893–894).
- Fast-mode plus minimum `ic_clk`: 32 MHz (section 12.2.12).
- Spike-suppression default: 7 `clk_sys` cycles; register `IC_FS_SPKLEN` `0x0a0`.
