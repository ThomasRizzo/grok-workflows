---
name: rp2350-pio
description: "RP2350 PIO block lookup (outline 11, PDF pages 792-871) for PIO0/PIO1/PIO2, state machines, pioasm, instruction set, FIFOs, GPIO window, clock dividers, DMA DREQ, and registers. Trigger on PIO, state machine, SM, pioasm, GPIOBASE, or /rp2350-pio."
---

# RP2350 PIO

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF page range: 792–871. Outline: 11 PIO. Printed footers in this extract are one less than the PDF page index.

Run `pdftotext -layout -f <first> -l <last>` on that range, in windows of at most 25 pages. Do not load the whole range at once. Answer only from the extract. Paraphrase; do not paste register tables, bit descriptions, or program listings. Cite the PDF page index.

Where to look:

- 792–794: overview, RP2040 changes, security
- 794–798: programmer’s model, shift registers, FIFOs, stalling
- 800–803: 11.3 pioasm
- 803–814: 11.4 instruction set (16-bit encodings, then each opcode)
- 815–826: 11.5 side-set, wrap, FIFO join, autopush/pull, clock dividers, GPIO mapping
- 827–847: 11.6 examples
- 848–871: 11.7 register list and field descriptions. Next outline section is 12.1 UART on PDF page 872.

Facts read from this range:

- 3 PIO blocks, 4 state machines each (PDF 792)
- Shared instruction memory: 32 instructions, 4 read ports; instructions are 16 bits (PDF 792, 803)
- Each SM: TX and RX FIFOs 4 words deep, joinable to 8 words one way; FIFOs raise DREQ (PDF 792, 798)
- Per-SM clock divider: 16-bit integer plus 8-bit fractional, from the system clock (PDF 792, 822)
- Each block sees 32 GPIOs at a time (30 on RP2350A); GPIOBASE picks which 32 (PDF 793)
- Bases: PIO0 `0x50200000`, PIO1 `0x50300000`, PIO2 `0x50400000` (PDF 848)
- List starts PDF 848: CTRL 0x000, FSTAT 0x004, FDEBUG 0x008, FLEVEL 0x00c, TXF0 0x010, RXF0 0x020, IRQ 0x030, DBG_CFGINFO 0x044, INSTR_MEM0–31 0x048–0x0c4
- SM register groups begin at SM0 0x0c8, SM1 0x0e0, SM2 0x0f8, SM3 0x110; GPIOBASE 0x168; IRQ0_INTE 0x170 (PDF 850–851)
- DBG_CFGINFO.VERSION reads as 1 (PDF 793)
- Eight SM IRQ flags; IRQ0_INTE and IRQ1_INTE can enable all eight (PDF 793)
- Non-secure PIO (ACCESSCTRL) reads a Secure GPIO as 0; cross-PIO IRQs and CTRL next/prev are off between Non-secure and Secure-only blocks (PDF 794)
- Opcodes: JMP, WAIT, IN, OUT, PUSH, PULL, MOV, IRQ, SET; each executes in one clock cycle (PDF 803–804)

Sibling skills only where this block depends on them:

- `/rp2350-gpio` for the pins the state machines drive and sample
- `/rp2350-clocks` for the system clock the dividers subdivide (`clk_sys` in the examples)
- `/rp2350-dma` for the controller paced by PIO FIFO DREQ
- `/rp2350-access-control` for which PIO blocks and GPIOs are Non-secure
