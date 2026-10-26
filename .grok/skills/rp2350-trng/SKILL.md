---
name: rp2350-trng
description: "RP2350 TRNG block (outline 12.12) on datasheet PDF pages 1123-1131. Use for TRNG base address, ROSC entropy, EHR_DATA, sample count, interrupts, enable/reset, and bootrom or SDK random paths. Trigger: /rp2350-trng."
---

# RP2350 TRNG

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF pages 1123–1131. Outline 12.12. TRNG. The printed footer on a page is one less than that PDF index.

Run `pdftotext -layout -f <start> -l <end>` on this range, in windows of at most 25 pages. This range is 9 pages, so one window is enough. Answer only from that extract. Paraphrase; cite the PDF page index. Do not copy register tables or description text into the reply.

Sibling skills, only where this block’s procedure uses them: `rp2350-clocks` (sample interval vs system clock; the TRNG ROSC is not a system clock), `rp2350-resets` (RESETS before the bootrom sequence), `rp2350-sha-256` and `rp2350-bootrom` (bootrom streams raw ROSC samples into SHA-256; see outline 5). No GPIO pin function in this section.

Facts read from this range:

- One control instance; base `0x400f0000` (`TRNG_BASE`); register list on PDF page 1126.
- No pin mux. Internal ring oscillator, asynchronous to RP2350 system clocks.
- 192-bit result in `EHR_DATA0`–`EHR_DATA5`, offsets `0x114`–`0x128`; reading `EHR_DATA5` clears the set.
- `RND_SOURCE_ENABLE` at `0x12c` (`RND_SRC_EN`); `TRNG_CONFIG` at `0x10c` selects one of four ROSC chain lengths.
- `SAMPLE_CNT1` at `0x130` (reset `0x0000ffff`) counts `rng_clk` cycles between samples.
- IRQ/status: `RNG_IMR` `0x100`, `RNG_ISR` `0x104`, `RNG_ICR` `0x108` (`EHR_VALID`, `VN_ERR`, `CRNGT_ERR`, `AUTOCORR_ERR`).
- `TRNG_SW_RESET` at `0x140`; `TRNG_BUSY` at `0x1b8`; list continues through `RNG_BIST_CNTR_2` at `0x1e8` (PDF page 1127).
- Overview rate about 7.5 kb/s at 150 MHz; chain length 0 or 1 with sample count 20–25 is about 2 ms (PDF page 1123).
- Section heading `12.12. TRNG` is on PDF page 1123; register detail runs through PDF page 1131 (`RNG_VERSION` at `0x1c0`).
