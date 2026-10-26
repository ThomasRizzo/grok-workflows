---
name: rp2350-memory
description: "RP2350 Memory lookup (outline 4, PDF pages 312–326) for ROM, SRAM banks, boot RAM, XIP cache, XIP_CTRL/XIP_AUX bases, and OTP size. Trigger on SRAM map, 0x20000000, BOOTRAM, cache-as-SRAM, XIP window, or /rp2350-memory."
---

# RP2350 Memory

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF pages: 312–326 (pdftotext indexes, not the printed footer). Outline: 4. Memory.

Run `pdftotext -layout -f N -l M` on that range, at most 25 pages per call. This range is 15 pages, so one window is enough. Answer only from that extract. Paraphrase; cite the PDF page. Do not paste register tables or datasheet sentences into the reply.

Subsections to open by question:

- 4.1 ROM — PDF 312
- 4.2 SRAM and 4.2.1 other on-chip RAM — PDF 312–313
- 4.3 Boot RAM and 4.3.1 BOOTRAM registers — PDF 313–314
- 4.4 XIP, cache, cache-as-SRAM, QMI pointer, streaming DMA, counters — PDF 315–320
- 4.4.5 XIP_CTRL registers — PDF 320–323
- 4.4.6 XIP_AUX registers — PDF 324–325
- 4.5 OTP (pointer only) — PDF 326

Section 5 Bootrom starts on PDF 327. Stop there.

Facts read from this PDF (not a register map):

- ROM: 32 kB at 0x00000000 (PDF 312)
- SRAM: 520 kB, 10 banks (8×64 kB + 2×4 kB), base 0x20000000 (PDF 312)
- Stripe split at 0x20040000 (SRAM0/SRAM1); 4 kB banks at 0x20080000 and 0x20081000 (PDF 312–313)
- Optional USB DPRAM: 4 kB at 0x50100000; XIP cache pin size 16 kB (PDF 313)
- Boot RAM: 1 kB, BOOTRAM_BASE 0x400e0000, registers from offset 0x800, BOOTLOCK0–7 (PDF 313–314)
- XIP: 16 kB cache, two 8 kB banks; AHB 0x10001234 maps to QSPI offset 0x001234 (PDF 315)
- XIP_CTRL_BASE 0x400c8000 (PDF 320); XIP_AUX_BASE 0x50500000 (PDF 324)
- QMI: up to two external QSPI devices; detail is outline 12.14 (PDF 319)
- OTP: 8 kB; detail is outline 13 (PDF 326)

Sibling skills, only where this section defers:

- ROM contents, boot RAM layout, and boot locks reserved for the bootrom: `rp2350-bootrom`
- Boot locks match SIO spinlocks: `rp2350-sio`
- QSPI serial interface and chip-select config: `rp2350-qmi`
- XIP stream FIFO and DREQ_XIP_STREAM: `rp2350-dma`
- USB data DPRAM reuse: `rp2350-usb`
- IDAU regions and ACCESSCTRL for peripheral memory: `rp2350-access-control`
- OTP programming and layout: `rp2350-otp`
