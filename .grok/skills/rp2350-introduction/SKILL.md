---
name: rp2350-introduction
description: "RP2350 Introduction (outline 1, PDF pages 14-22): family variants, chip overview, pinout, GPIO banks, name decode, and silicon versions. Trigger on RP2350A/B, RP2354A/B, QFN-60/80, feature list, pinout, or /rp2350-introduction."
---

# RP2350 Introduction

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

Outline: 1. Introduction. PDF pages 14–22 (printed footers 13–21).

Run `pdftotext -layout -f 14 -l 22` on that PDF. The range is under 25 pages; if you split it, keep each window at most 25 pages. Answer only from that extract. Paraphrase; cite the PDF page index, not the printed footer.

This section is an overview. It has no register map and no base address. For depth, use the sibling skill that owns the topic:

- Processors: `rp2350-cortex-m33`, `rp2350-hazard3`
- Bus fabric: `rp2350-system-bus`
- SRAM and XIP: `rp2350-memory`
- Boot and OTP: `rp2350-bootrom`, `rp2350-otp`
- External QSPI: `rp2350-qmi`
- Clocks and core supply: `rp2350-clocks`, `rp2350-core-regulator`
- Pins, SIO, and PIO: `rp2350-gpio`, `rp2350-sio`, `rp2350-pio`
- Package drawings and electrical limits: `rp2350-electrical-mechanical` (Section 14)

## Where to look

- PDF 14: feature list; Table 1 device family
- PDF 15–16: Section 1.1 chip overview; Figure 1
- PDF 16–17: Section 1.2 pin locations; Figure 2 QFN-60; Figure 3 QFN-80
- PDF 17–18: Table 2 pin descriptions
- PDF 18–21: Section 1.2.3 GPIO bank 0; Tables 3–4
- PDF 21: Section 1.2.4 GPIO bank 1; Tables 5–6
- PDF 22: Section 1.3 name; Section 1.4 Table 7 versions

## Facts read from this range

- Table 1 (PDF 14): RP2350A QFN-60, no flash, 30 GPIO, 4 analogue; RP2350B QFN-80, no flash, 48 GPIO, 8 analogue; RP2354A/B same packages with 2 MB flash.
- PDF 14: dual Cortex-M33 or Hazard3 at 150 MHz; 520 kB SRAM in 10 banks; 8 kB OTP.
- PDF 14: dedicated QSPI up to 16 MB flash or PSRAM, plus optional second chip-select for another 16 MB.
- PDF 14: 2 UARTs, 2 SPI, 2 I2C, 24 PWM channels, 12 PIO state machines, 1 HSTX, USB 1.1 host/device.
- PDF 15: two PLLs; fixed 48 MHz for USB or ADC; system clock up to 150 MHz.
- PDF 16: core regulator buck up to 200 mA when awake; low-Iq LDO up to 1 mA when asleep.
- PDF 21: GPIOs 0–29 on every package; GPIOs 30–47 only on QFN-80 (RP2350B).
- PDF 21: analogue on GPIO 26–29 (QFN-60, four inputs) or GPIO 40–47 (QFN-80, eight inputs).
- PDF 21: GPIO bank 1 is USB DP, USB DM, QSPI SCK, QSPI CSn, QSPI SD0–SD3.
- PDF 22: name digits — 2 cores; 3 means Cortex-M33 or Hazard3; 5 means at least 512 kB SRAM (device has 520 kB); 0 means no on-chip flash (RP2354 has 2 MB).
- PDF 22: Table 7 — A2 initial release; A4 production. Revision detail is Appendix C and PCN 28, not this section.
- PDF 16 and 17: pin electrical specs and package drawings are Section 14, not this section.
