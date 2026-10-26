---
name: rp2350-qmi
description: "RP2350 QSPI memory interface (QMI, outline 12.14) on datasheet PDF pages 1137-1164. Use for QMI/XIP_QMI_BASE, two chip-select windows, QSPI pins, SCK divisor, direct-mode FIFOs, read/write formats, and ATRANS. Trigger: /rp2350-qmi."
---

# RP2350 QMI

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF pages 1137–1164. Outline 12.14. QSPI memory interface (QMI). The printed footer on a page is one less than that PDF index.

Run `pdftotext -layout -f <start> -l <end>` on this range, in windows of at most 25 pages (1137–1161, then 1162–1164). Answer only from that extract. Paraphrase; cite the PDF page index. Do not copy register tables or description text into the reply.

Sibling skills, only where this block’s procedure uses them: `rp2350-memory` (XIP windows, cache flush after ATRANS, streaming DMA in outline 4.4), `rp2350-gpio` (dedicated QSPI pins and the Bank 0 GPIO XIP chip-select function), `rp2350-clocks` (SCK is an integer divide of the system clock), `rp2350-bootrom` (ATRANS called rolling windows in outline 5.1.19).

Facts read from this range:

- One QMI instance, inside the XIP subsystem; replaces RP2040 SSI (PDF page 1137).
- Base `0x400d0000` (`XIP_QMI_BASE`); register list is outline 12.14.6 on PDF page 1146.
- Two devices: CS0 window `0x10000000` (dedicated pin); CS1 window `0x11000000` (GPIO function); 16 MB each, 32 MB max.
- Pins: `QSPI_SS`, `QSPI_SD[3:0]`, `QSPI_SCLK`; extra chip select is a Bank 0 GPIO XIP function.
- `SCK` divisor 1–256 of the system clock; sample delay in half system-clock cycles; SPI mode 0 only.
- Direct mode: TX FIFO 4×21-bit, RX FIFO 4×16-bit; `DIRECT_CSR`/`DIRECT_TX`/`DIRECT_RX` at `0x000`/`0x004`/`0x008`.
- Per window: `M0_TIMING`–`M0_WCMD` at `0x00c`–`0x01c`; `M1_TIMING`–`M1_WCMD` at `0x020`–`0x030`.
- `ATRANS0`–`ATRANS7` at `0x034`–`0x050`; eight 4 MB panes; identity map at reset.
- Address phase is 24 bits; reset read format is `03h` serial; reset write format is `02h` serial.
- Window-1 writes stay off until `XIP_CTRL_WRITABLE_M1` (XIP control, not a QMI register).
- Section heading `12.14. QSPI memory interface (QMI)` is on PDF page 1137; detail runs through the assigned end page 1164.
