---
name: rp2350-sha-256
description: "Lookup for the RP2350 SHA-256 accelerator (outline 12.13, PDF pages 1132-1136). Use for hash-block, base-address, CSR/WDATA/SUM, padding, byte-swap, DMA DREQ, or throughput questions. Trigger: /rp2350-sha-256."
---

# RP2350 SHA-256 accelerator

PDF: ./RP-008373-DS-3-rp2350-datasheet.pdf
PDF page range: 1132-1136 (`pdftotext -f` / `-l` indexes, not printed footer numbers).
Outline: 12.13 SHA-256 accelerator.

Run `pdftotext -layout -f 1132 -l 1136` on that PDF. The range is five pages, so one window is enough; never extract more than 25 pages in one call. Answer only from that extract. Paraphrase, and cite the PDF page index. Do not copy sentences, tables, or register descriptions.

The block requests the DMA controller for full blocks. For channel setup, use skill rp2350-dma. This section defines no pin functions.

Facts from this extract:

- One SHA-256 accelerator
- Base address 0x400f8000 (SHA256_BASE)
- CSR 0x000, WDATA 0x004, SUM0–SUM7 0x008–0x024
- Overview and message padding: PDF page 1132
- Throughput, endianness, DMA DREQ, register list: PDF page 1133
- CSR fields: PDF page 1134
- WDATA and SUM0–SUM2: PDF page 1135
- SUM3–SUM7: PDF page 1136
- No external pins
