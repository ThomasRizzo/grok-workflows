---
name: rp2350-pwm
description: "RP2350 PWM block lookup (outline 12.5, PDF pages 974-1010) for slices, GPIO channel map, clk_sys divider, duty cycle, phase, IRQ0/IRQ1, DREQ, PWM_BASE, and registers. Trigger on those questions and on /rp2350-pwm."
---

# RP2350 PWM

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF page range: 974–1010. Outline: 12.5 PWM. Printed footers in this extract are one less than the PDF page index. The next outline section is 12.6 DMA on PDF page 1011.

Run `pdftotext -layout -f <first> -l <last>` on that range, in windows of at most 25 pages. Do not load 974–1010 in one pass. Answer only from the extract. Paraphrase; do not paste register tables or bit descriptions. Cite the PDF page index.

Where to look:

- 974–975: 12.5.1 overview, RP2040 changes, 12.5.2 pin map (Table 1183)
- 976–978: 12.5.2.1–12.5.2.3 PWM compare, 0%/100% duty, double buffering
- 979–981: 12.5.2.4–12.5.2.6 clock divider, input measure modes, period
- 982–983: 12.5.2.7 IRQ and DREQ, 12.5.2.8 phase, 12.5.3 register list
- 983–985: Table 1184 offsets through CH11 and `EN`
- 986–1010: per-channel fields, then `EN`, `INTR`, `IRQ0_*`, `IRQ1_*` (Tables 1185–1252)

Facts read from this range:

- 12 slices; each has A and B outputs (24 outputs on QFN-80)
- Base `0x400a8000` (`PWM_BASE`), stated on PDF 983
- Slice stride 0x14: `CHn_CSR`, `DIV`, `CTR`, `CC`, `TOP` (CH0 at 0x000, CH11_TOP at 0x0ec)
- Globals: `EN` 0x0f0, `INTR` 0x0f4, IRQ0 `INTE`/`INTF`/`INTS` 0x0f8/0x0fc/0x100, IRQ1 0x104/0x108/0x10c
- GPIO map (PDF 975): slices 0–7 on GPIO 0–15 and again 16–31; slices 8–11 on GPIO 32–39 and again 40–47
- 16-bit counter; 8.4 fractional divider; count rate is system clock divided by `DIV` (example uses `clk_sys`)
- Two shared IRQ lines (`IRQ0_*` and `IRQ1_*`); wrap also pulses a one-cycle DMA DREQ
- QFN-60 has no GPIO 30–47; slices 8–11 still usable as repeating timers

Sibling skills only where this block depends on them:

- `/rp2350-clocks` for `clk_sys`, the counter clock before the fractional divider
- `/rp2350-gpio` for PWM function select; Table 1183 points at the GPIO function table
- `/rp2350-dma` for the controller that consumes the PWM wrap DREQ; request behaviour is in 12.5.2.7
