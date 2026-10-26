---
name: rp2350-clocks
description: "Look up the RP2350 clocks block (outline 8, PDF pages 468-538): CLOCKS, XOSC, ROSC, LPOSC, tick generators, and PLL_SYS/PLL_USB. Use when asked about clk_sys, clk_ref, clk_peri, clk_usb, clk_adc, clk_hstx, clk_gpout, GPIN, resus, the frequency counter, or /rp2350-clocks."
---

# RP2350 clocks

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF page range: 468-538. Outline: 8. Clocks.

Run `pdftotext -layout -f START -l END` on that range, in windows of at most 25 pages. Do not load the whole chapter at once. Suggested windows: 468-479 (sources and generators), 480-504 (CLOCKS register list), 505-529 (XOSC, ROSC, LPOSC, ticks), 530-538 (PLLs). Answer from the extract. Paraphrase; cite the PDF page index, not the printed footer (footer number is one less than the PDF index).

This chapter includes the clock generators and the XOSC, ROSC, LPOSC, tick, and PLL register blocks. For GPIO pinmux of `GPIO_FUNC_GPCK`, use `rp2350-gpio`. LPOSC trim registers live in POWMAN; use `rp2350-powman`. External tick in place of LPOSC is specified under the always-on timer; use `rp2350-always-on-timer`. SLEEP and DORMANT entry that stops oscillators is in `rp2350-power-reduction`.

## Facts read this run

- One CLOCKS instance, `CLOCKS_BASE` `0x40010000`; register list starts PDF p.482.
- Generators: `clk_gpout0`–`clk_gpout3`, `clk_ref`, `clk_sys`, `clk_peri`, `clk_usb`, `clk_adc`, `clk_hstx` (PDF p.473).
- Nominal: `clk_ref` 6–12 MHz; `clk_sys` 150 MHz; `clk_peri` 12–150 MHz; `clk_usb` and `clk_adc` 48 MHz; `clk_hstx` 150 MHz (PDF p.473).
- Up to four GPIO clock outputs, 50 MHz max (PDF p.468); SDK function `GPIO_FUNC_GPCK` (PDF p.481).
- GPIN GPIOs in the SDK snippet: 20 and 22, plus 12 and 14 on RP2350 (PDF p.481). External 32.768 kHz on GPIO 12, 14, 20, or 22 (PDF p.522).
- `XOSC_BASE` `0x40048000` (PDF p.510). Crystal range 1–50 MHz; design example 12 MHz; PLL needs at least 5 MHz (PDF p.507).
- `ROSC_BASE` `0x400e8000` (PDF p.516). Boot ROSC nominal 11 MHz, guaranteed 4.6–19.6 MHz (PDF p.469).
- LPOSC registers are in `POWMAN_BASE` `0x40100000` (PDF p.522). Tick generators: `TICKS_BASE` `0x40108000` (PDF p.523).
- Two PLLs: `PLL_SYS_BASE` `0x40050000`, `PLL_USB_BASE` `0x40058000` (PDF p.534). SDK default from 12 MHz: SYS 150 MHz, USB 48 MHz (PDF p.533).
- A3 reset: `CLK_SYS_CTRL.SRC` 0 to 1; `AUXSRC` 0 to 2 (ROSC) (PDF p.469).
- Glitchless mux only on `clk_ref` and `clk_sys` (PDF p.473). Resus forces stopped `clk_sys` onto `clk_ref` (PDF p.475).
