---
name: rp2350-uart
description: "RP2350 UART block lookup (outline 12.1, PDF pages 872-892) for UART0/UART1, PL011, base addresses, baud rate, UARTCLK, FIFOs, flow control, DMA, interrupts, and registers. Trigger on those questions and on /rp2350-uart."
---

# RP2350 UART

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF page range: 872–892. Outline: 12.1 UART. Printed footers in this extract are one less than the PDF page index.

Run `pdftotext -layout -f <first> -l <last>` on that range, in windows of at most 25 pages. This range is 21 pages, so one window is enough. Answer only from the extract. Paraphrase; do not paste register tables or bit descriptions. Cite the PDF page index.

Where to look:

- 872–874: instances, pins, clocks, block diagram, FIFOs
- 874–877: baud clocks, framing, errors, hardware flow control
- 878–879: 12.1.5 DMA
- 879–880: 12.1.6 interrupts
- 880–881: 12.1.7 programmer’s model and baud example
- 882–892: 12.1.8 register list and fields (Table 1081 onward). Next outline section is 12.2 I2C on PDF page 893.

Facts read from this range:

- Two instances, Arm PrimeCell UART PL011 revision r1p5
- Bases: UART0 `0x40070000`, UART1 `0x40078000` (PDF 882)
- TX FIFO 32×8; RX FIFO 32×12; either can be set 1 deep
- UARTCLK from `clk_peri`; PCLK from `clk_sys`
- GPIO names: `uart0_` / `uart1_` plus `tx`, `rx`, `rts`, `cts` (mux table is Section 9.4)
- Modem mode and IrDA not supported
- Max baud `UARTCLK/16` (7.8 Mbaud at 125 MHz); `FUARTCLK` ≤ 5/3 × `FPCLK`
- Divisor: 16-bit `UARTIBRD` plus 6-bit `UARTFBRD`
- List starts PDF 882: `UARTDR` 0x000 through `UARTDMACR` 0x048; ID registers 0xfe0–0xffc
- DMA text PDF 878; interrupt sources PDF 879; init steps PDF 880 (release reset, enable `clk_peri`)

Sibling skills only where this block depends on them:

- `/rp2350-clocks` for `clk_peri`, `clk_sys`, and Figure 36 (outside this range)
- `/rp2350-gpio` for function select (Sections 9.4 and 9.10.1)
- `/rp2350-dma` for the controller that takes the UART single/burst requests; request control is in 12.1.5
- `/rp2350-resets` for the reset release that 12.1.7 lists as the first init step
