---
name: rp2350-usb
description: "Look up the RP2350 USB controller (outline 12.7, PDF pages 1059-1094): device and host, DP/DM PHY, DPSRAM, clk_usb, and USBCTRL registers. Use when asked about USB, USBCTRL_REGS_BASE, USBCTRL_DPRAM, endpoints, VBUS, or /rp2350-usb."
---

# RP2350 USB

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF page range: 1059-1094. Outline: 12.7. USB.

Run `pdftotext -layout -f START -l END` on that range, in windows of at most 25 pages. Do not load the whole chapter at once. Suggested windows: 1059-1070 (overview, RP2040 deltas, functional description), 1071-1094 (programmer’s model and register list). Answer from the extract. Paraphrase; cite the PDF page index, not the printed footer (footer number is one less than the PDF index).

`clk_usb` and `clk_sys` come from the clocks block; use `rp2350-clocks`. DP, DM, and VBUS GPIO muxing is in `rp2350-gpio`. Bring the controller out of reset with `RESET_USBCTRL`; use `rp2350-resets`. Bus access to USBCTRL and DPSRAM is filtered by `rp2350-access-control`.

## Facts read this run

- One USB 2.0 controller: FS device 12 Mb/s; host LS 1.5 Mb/s and FS, including hubs (PDF p.1059).
- Integrated USB 1.1 PHY on DP and DM; pins are 3.3 V GPIO when the controller is unused (PDF p.1059).
- DPSRAM 4 kB at `USBCTRL_DPRAM_BASE` `0x50100000`; device usable buffer space 3840 bytes; endpoints 0–15 IN and OUT (PDF p.1059).
- DPSRAM system port is `clk_sys`; controller port is `clk_usb` (PDF p.1059). Set `SIE_CTRL.START_TRANS` at least two `clk_usb` cycles after other `SIE_CTRL` writes (PDF p.1068).
- Host EPx control at DPSRAM offset `0x80`; EPx buffer control at `0x100` (PDF p.1069). Host polls up to 15 interrupt endpoints (PDF p.1059).
- MMIO base `USBCTRL_REGS_BASE` `0x50110000`; register list Table 1306 starts PDF p.1074.
- `ADDR_ENDP` `0x000`; `ADDR_ENDP1`–`ADDR_ENDP15` `0x004`–`0x03c`; `MAIN_CTRL` `0x040`; `SIE_CTRL` `0x04c`; `SIE_STATUS` `0x050`; `INT_EP_CTRL` `0x054`; `BUFF_STATUS` `0x058` (PDF p.1074).
- `USB_MUXING` `0x074` (includes `USBPHY_AS_GPIO`, PDF p.1085); `USB_PWR` `0x078` and `USBPHY_DIRECT` `0x07c` (PDF p.1086).
- `INTR` `0x08c` (PDF p.1089), `INTE` `0x090` (p.1090), `INTF` `0x094` (p.1091), `INTS` `0x098` (p.1092); `SM_STATE` `0x108` and `EP_TX_ERROR` `0x10c` (p.1093); `EP_RX_ERROR` `0x110` (p.1094).
- Clear `MAIN_CTRL.PHY_ISO` before using the PHY; `SIE_CTRL.PULLDOWN_EN` and `USB_MUXING.TO_PHY` reset to 1 (PDF p.1060).
- VBUS enable is in `SIE_CTRL`; VBUS detect and overcurrent are in `SIE_STATUS`; VBUS detect can be forced in `USB_PWR` (PDF p.1070).
- Example init uses `RESET_USBCTRL` and `USBCTRL_IRQ` (PDF p.1071).
