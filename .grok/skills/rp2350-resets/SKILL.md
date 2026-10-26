---
name: rp2350-resets
description: "Lookup for the RP2350 Resets block, outline 7, PDF pages 451-467. Use for chip-level reset, RUN, POR, BOD/BOR, PSM, subsystem RESETS, WDSEL, RESET_DONE, or CHIP_RESET. /rp2350-resets"
---

# RP2350 Resets

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF pages 451–467. Outline 7. Resets.

Run `pdftotext -layout -f 451 -l 467` on that PDF. The range is under 25 pages; one window is enough. If you split it, stay at or under 25 pages per call. Answer only from that extract. Paraphrase; cite the PDF page index (not the printed footer).

Depends on `/rp2350-powman` (CHIP_RESET and BOD live in POWMAN), `/rp2350-watchdog` (can start chip, system, or subsystem resets), `/rp2350-clocks` (PSM releases ROSC, XOSC, and CLOCKS), `/rp2350-bootrom` (DOUBLE_TAP and RESCUE_FLAG), `/rp2350-debug` (debugger reset request and rescue), `/rp2350-glitch-detector` (chip-level source), and `/rp2350-core-regulator` (POR/BOD supply is VREG_AVDD). Subsystem bits reset other peripherals; those skills are not required to read this block.

Facts read from pages 451–467:

- Three groups: chip-level, system (PSM), subsystem (RESETS); one PSM and one RESETS controller
- PSM base `0x40018000` (`PSM_BASE`): `FRCE_ON` `0x000`, `FRCE_OFF` `0x004`, `WDSEL` `0x008`, `DONE` `0x00c`
- PSM bits 0–24: `PROC_COLD`, `OTP`, `ROSC`, `XOSC`, `RESETS`, `CLOCKS`, `PSM_READY`, `BUSFABRIC`, `ROM`, `BOOTRAM`, `SRAM0`–`SRAM9`, `XIP`, `SIO`, `ACCESSCTRL`, `PROC0`, `PROC1`
- RESETS base `0x40020000` (`RESETS_BASE`): `RESET` `0x000` (reset `0x1`), `WDSEL` `0x004` (reset `0x0`), `RESET_DONE` `0x008` (RO, reset `0x0`)
- RESETS bits 0–28: `ADC`, `BUSCTRL`, `DMA`, `HSTX`, `I2C0`, `I2C1`, `IO_BANK0`, `IO_QSPI`, `JTAG`, `PADS_BANK0`, `PADS_QSPI`, `PIO0`, `PIO1`, `PIO2`, `PLL_SYS`, `PLL_USB`, `PWM`, `SHA256`, `SPI0`, `SPI1`, `SYSCFG`, `SYSINFO`, `TBMAN`, `TIMER0`, `TIMER1`, `TRNG`, `UART0`, `UART1`, `USBCTRL`
- Chip sources: POR, BOR, RUN, debugger reset request, rescue, watchdog POWMAN async/soft, watchdog SWCORE, SWCORE power-down, glitch detector, watchdog PSM; `FRCE_ON` is unused on production parts
- External reset is the RUN pin; tie high if unused; a double tap sets `CHIP_RESET.DOUBLE_TAP`; rescue sets `CHIP_RESET.RESCUE_FLAG`
- `CHIP_RESET` and AON scratch live in POWMAN; POWMAN base `0x40100000`; BOD names: `BOD_CTRL`, `BOD`, `BOD_LP_ENTRY`, `BOD_LP_EXIT`
- POR: `DVDDTH.POR` typ 0.957 V (0.924–0.99 V); `tPOR.ASSERT` typ 3 µs, max 10 µs (PDF page 463)
- BOD: nominal threshold 0.946 V; `BOD.EN` and `BOD.VSEL`; `tBOD.ASSERT` typ 3 µs, max 10 µs; `VREG_AVDDTH.ACTIVE` typ 1.1 V (0.87–1.26 V)
