---
name: rp2350-spi
description: "Look up the RP2350 SPI block (outline 12.3, PDF pages 944-963): SPI0/SPI1 instances, bases, pins, clocks, FIFOs, formats, DMA, and SSP registers. Trigger on SPI, PL022, SSP, SCK/TX/RX/CSn, or /rp2350-spi."
---

# RP2350 SPI

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF page range: 944–963 (`pdftotext -f` / `-l` indexes, not the printed footer numbers).
Outline: 12.3 SPI.

Run `pdftotext -layout` on that range, in windows of at most 25 pages (this range fits in one window). Answer only from that extract. Paraphrase; cite the PDF page index. Do not paste register tables or datasheet sentences into the answer.

This block uses the GPIO function select and the peripheral/bus clocks, and it can handshake with DMA. When those details matter, use `rp2350-gpio`, `rp2350-clocks`, and `rp2350-dma`. Do not answer SPI questions from the QMI section.

Facts (confirm in the extract; each is one short lookup, not a description):

- Heading `12.3. SPI` is on PDF page 944.
- Two identical controllers: SPI0 and SPI1; Arm PrimeCell SSP PL022 r1p4.
- Bases: SPI0 `0x40080000`, SPI1 `0x40088000` (PDF page 957; SDK `SPI0_BASE`, `SPI1_BASE`).
- Timing clock `clk_peri` (SSPCLK); bus clock `clk_sys` (PCLK).
- GPIO signals SCK, TX, RX, CSn; names such as `SPI0 TX` in the Bank 0 function table (section 9.4).
- TX and RX FIFOs: 8 locations, 16 bits wide; data size 4–16 bits.
- Master or slave; Motorola SPI, TI synchronous serial, or National Microwire.
- Register list on PDF page 957: SSPCR0 `0x000`, SSPCR1 `0x004`, SSPDR `0x008`, SSPSR `0x00c`, SSPCPSR `0x010`, SSPIMSC `0x014`, SSPRIS `0x018`, SSPMIS `0x01c`, SSPICR `0x020`, SSPDMACR `0x024`; ID registers from `0xfe0`.
- Interrupts: SSPTXINTR, SSPRXINTR, SSPRORINTR, SSPRTINTR; one combined interrupt to the processors.
- DMA: SSPDMACR plus single and burst TX/RX requests; burst trigger at four FIFO entries (PDF pages 956–957).
- Slave TX output-enable is the peripheral `nSSPOE` (section 12.3.1).
- Not the QSPI memory interface (outline 12.14).
