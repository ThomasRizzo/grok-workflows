---
name: rp2350-gpio
description: "Look up the RP2350 GPIO block (outline 9, PDF pages 539-734): banks, FUNCSEL, pads, interrupts, and IO/PADS bases. Trigger on GPIO, IO_BANK0, IO_QSPI, PADS_BANK0, PADS_QSPI, pad isolation, or /rp2350-gpio."
---

# RP2350 GPIO

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF page range: 539–734 (`pdftotext -f` / `-l` indexes, not the printed footer numbers).
Outline: 9 GPIO.

Run `pdftotext -layout` on that range, in windows of at most 25 pages. Answer only from that extract. Paraphrase; cite the PDF page index. Do not paste register tables or datasheet sentences into the answer.

Read the overview first (PDF 539–550). For a single register, open the list page below instead of the whole map (user-bank IO from PDF 549, QSPI IO from PDF 682, user pads from PDF 705, QSPI pads from PDF 731–734).

Processor bit-bang registers are in `rp2350-sio`. Pin mux targets that are not fully specified here belong to `rp2350-pio`, `rp2350-clocks`, `rp2350-uart`, `rp2350-i2c`, `rp2350-spi`, `rp2350-pwm`, `rp2350-adc`, `rp2350-hstx`, `rp2350-qmi`, and `rp2350-usb`. Secure versus Non-secure GPIO masks are in `rp2350-access-control`. The M33 GPIO coprocessor instruction set is in `rp2350-m33-coprocessors`. The pull-down caution is in `rp2350-errata` (RP2350-E9).

Facts (confirm in the extract; each is one short lookup, not a description):

- Heading `9. GPIO` is on PDF page 539; pull-down caution names erratum RP2350-E9, Appendix D.5.1.
- Up to 54 GPIOs: Bank 0 is 30 on RP2350A (QFN-60, GPIO0–29) or 48 on RP2350B (QFN-80, GPIO0–47). Bank 1 is six QSPI pins plus USB DP/DM; USB pins are not part of the 54.
- ADC inputs on Bank 0: GPIO26–29 (QFN-60, four) or GPIO40–47 (QFN-80, eight).
- FUNCSEL is in each pin CTRL; one function per pin; one peripheral input on several pins is ORed. Table 677 PDF 541, names Table 678 PDF 542; QSPI/USB Table 679 and Table 680 PDF 543.
- Bank 0 function columns include HSTX, SPI, UART, I2C, PWM, SIO, PIO0, PIO1, PIO2, clock/trace/QMI CS1n, USB VBUS, and an alternate UART. Reset mux is the null function.
- Two external clock inputs and four general-purpose clock outputs (PDF 539).
- Reset (PDF 540): Bank 0 output high-Z, input off, pulled low, ISO latched; digital use needs IE set and ISO clear. Bank 1 IE resets set; SCK, SD0, SD1 pull down; SD2, SD3, CSn pull up.
- Interrupts (PDF 543–544): level high, level low, rising edge, falling edge. Edges stick in INTR. Twelve IRQ lines: both banks, to proc0, proc1, and dormant_wake, each Secure and Non-secure.
- `IO_BANK0_BASE` `0x40028000` (PDF 549): `GPIOn_STATUS` / `GPIOn_CTRL` pairs from offset `0x000`, 8 bytes per pin, GPIO0–GPIO47.
- `IO_QSPI_BASE` `0x40030000` (PDF 682): USBPHY_DP, USBPHY_DM, GPIO_QSPI_SCLK, SS, SD0–SD3 STATUS/CTRL from `0x000`; IRQ summary and INTE from `0x200`.
- `PADS_BANK0_BASE` `0x40038000` (PDF 705), including SWCLK and SWD. `PADS_QSPI_BASE` `0x40040000` (PDF 731): SCLK, SD0–SD3, SS.
- SIO GPIO control is section 9.8 (PDF 546); the high registers cover GPIO32–47, the QSPI pads, and USB DM/DP. Coprocessor port is section 9.9 (PDF 546).
