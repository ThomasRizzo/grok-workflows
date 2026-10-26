---
name: rp2350-electrical-mechanical
description: "Lookup for the RP2350 electrical and mechanical section, outline 14, PDF pages 1325-1346. Use for QFN-60/QFN-80 packages, pinout, supplies, absolute maximum ratings, IO levels, core-regulator limits, power consumption, solder profile, and flash-in-package. /rp2350-electrical-mechanical"
---

# RP2350 electrical and mechanical

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF pages 1325–1346. Outline 14. Electrical and mechanical.

Run `pdftotext -layout` on that PDF for this range, in windows of at most 25 pages. The range is 22 pages, so one window is enough; if you split, use 1325–1336 then 1337–1346. Answer only from that extract. Paraphrase and cite the PDF page index, not the printed footer (footer is one less than the PDF index). Page 1346 is only Figure 156. Do not continue into Appendix A (PDF 1347). This section has no register base address.

Use a sibling skill only for the behaviour this section does not specify: `/rp2350-qmi` (QSPI controller and a second chip-select), `/rp2350-bootrom` (BOOTSEL and UART boot on the QSPI pins), `/rp2350-core-regulator` (regulator registers; VREG pins and Table 1689 stay here), `/rp2350-gpio` (pad control; pin numbers and IO tables stay here), `/rp2350-adc` (conversion; which pins are analogue stays here), `/rp2350-usb` (USB controller; series resistors stay here), `/rp2350-resets` (reset causes beyond the RUN pin), `/rp2350-clocks` (clock tree and WAKE_EN; the DVDD-versus-frequency curve stays here), `/rp2350-powman` and `/rp2350-power-reduction` (how to enter the P1.x states; measured current stays here), `/rp2350-otp` (OTP contents; USB_OTP_VDD stays here), `/rp2350-debug` (SWD protocol; SWCLK/SWDIO pin numbers stay here).

Where to look:

- 1325–1327: 14.1 QFN-60 and 14.2 QFN-80, thermal Tables 1670–1671, footprints Figures 148 and 150
- 1328–1330: 14.3 flash in package, 14.4 markings, 14.5 storage, 14.6 solder profile (Figure 151, Table 1672)
- 1331: 14.7 compliance; 14.8.1.1 QFN-60 pin locations, Figure 152
- 1332–1335: QFN-80 Figure 153, pin types Table 1673, pin list Tables 1674–1679
- 1336–1341: 14.9.1 absolute maximum through 14.9.5 supplies (Tables 1680–1688, Figure 154)
- 1342–1346: 14.9.6 core voltage regulator (Table 1689, Figure 155) and 14.9.7 power (Tables 1690–1693, Figure 156)

Facts read from pages 1325–1346:

- QFN-60 is RP2350A/RP2354A, 7×7 mm; QFN-80 is RP2350B/RP2354B, 10×10 mm; ground is the exposed pad
- RP2354A/B add a stacked Winbond W25Q16JVWI (2 MB); QSPI_IOVDD is 2.7–3.6 V; that flash’s maximum clock is 133 MHz
- QFN-60 is MSL1; QFN-80 MSL is not finished; Pb-free Tp is 260°C; store under 30°C and 85% RH
- Marking: package letter A or B, package revision 0, die stepping A2, A3, or A4
- QFN-60 digital IO is GPIO0–GPIO29, with ADC on GPIO26_ADC0–GPIO29_ADC3 (pins 40–43); QFN-80 IO is GPIO0–GPIO47, with ADC on GPIO40_ADC0–GPIO47_ADC7
- QSPI_SD3, QSPI_SCLK, QSPI_SD0, QSPI_SD2, QSPI_SD1, QSPI_SS are QFN-60 pins 55–60 and QFN-80 pins 70–75; domain QSPI_IOVDD; QSPI_SS is also USB BOOTSEL
- XIN/XOUT are QFN-60 21/22 and QFN-80 30/31, domain IOVDD; 12 MHz crystal, or a square wave into XIN
- RUN is QFN-60 pin 26 and QFN-80 pin 35 (active-low chip enable); SWCLK is 24/33; SWDIO is 25/34
- USB_DP/USB_DM are QFN-60 52/51 and QFN-80 67/66, domain USB_OTP_VDD; USB needs external 27 Ω series resistors
- Named supplies: DVDD, IOVDD, QSPI_IOVDD, USB_OTP_VDD, ADC_AVDD, VREG_AVDD, VREG_PGND, VREG_LX, VREG_VIN, VREG_FB; QFN-60 QSPI_IOVDD is pin 54 and QFN-80 QSPI_IOVDD is pin 69
- Figure 152 shows QFN-60 pin 1 as IOVDD; Table 1679 lists QFN-60 IOVDD as 11, 20, 30, 38, 45, 54 — use the figure before treating pin 54 as IOVDD
- Default digital drive is 4 mA; fault-tolerant pins tolerate up to 5.5 V when IOVDD is 3.3 V
