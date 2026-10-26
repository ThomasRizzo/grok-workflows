---
name: rp2350-errata
description: "Lookup for the RP2350 Errata appendix, outline D, PDF pages 1352-1368. Use for RP2350-E1 through E31, which revision is affected, the workaround, and what Fixed by names. Trigger: /rp2350-errata."
---

# RP2350 Errata

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF page range: 1352–1368 (`pdftotext -f` / `-l` indexes, not the printed footer). Outline: D. The range is 17 pages, so one `pdftotext -layout -f 1352 -l 1368` window is enough. If you narrow it, still stay at or under 25 pages. Answer only from that extract. Paraphrase Summary, Affects, Workaround, and Fixed by. Cite the PDF page index. Do not paste the description.

Find an item by `RP2350-En` or by the section name below, then read that PDF page. Each item is Reference, Summary, Affects, Description, Workaround, Fixed by.

## Facts

- Heading on PDF page 1352: Appendix D. Errata. Alphabetical by section. Printed footer on 1352 is 1351.
- 11 sections. 31 items, IDs RP2350-E1 through RP2350-E31, no missing numbers.
- PDF 1352: D.1 ACCESSCTRL (E3; A2, QFN-60 only). D.2 Bootrom starts on 1352.
- Bootrom: E10 (1352); E13, E14, E15 (1353); E18, E19, E20 (1354); E21 (1355); E22, E23, E24 (1356); E25, E30 (1357).
- PDF 1357: D.3 Bus Fabric (E27). PDF 1358: D.4 DMA (E5). PDF 1359: E8 and D.5 GPIO (E9). PDF 1360 is the E9 leakage figures only.
- PDF 1361: D.6 Hazard3 (E4, E6). PDF 1362: E7, E29. PDF 1363: E31, then D.7 OTP (E16).
- PDF 1364: E17, E28. PDF 1365: D.8 RCP (E26) and D.9 SIO (E1). PDF 1366: E2 and D.10 XIP (E11). PDF 1367–1368: D.11 USB (E12).
- Closed on A3 (not listed for A3/A4): E3, E9, E10, E13, E14, E15, E16, E19, E20, E21, E22, E23.
- Still listed for A3, closed by A4 bootrom: E18, E24, E25. E16 Fixed by is A3; E18 and E24 Fixed by is A4 bootrom.
- Still listed through A4: E1, E2, E4, E5, E6, E7, E8, E11, E12, E17, E26, E27, E28, E29, E30, E31. E12 says mitigated on A3.
- No errata base address and no register map. E3 is the only package-limited item (QFN-60).

## Siblings

Name a sibling only when the question also needs that block’s normal behaviour:

- `rp2350-access-control` — D.1 / E3 (`GPIO_NSMASK`, PADS).
- `rp2350-bootrom` — D.2. `rp2350-qmi` for E14 CS1 / `FLASH_DEVINFO`. `rp2350-powman` for E19 `FRCE_OFF`. `rp2350-glitch-detector` for E20, E21, E24. `rp2350-otp` for bootrom OTP rows (E15, E18).
- `rp2350-system-bus` — D.3 / E27.
- `rp2350-dma` — D.4 / E5, E8.
- `rp2350-gpio` — D.5 / E9. `rp2350-adc` only if the E9 input-enable note is in play.
- `rp2350-hazard3` — D.6. `rp2350-debug` for E4 SBA and E29 breakpoints.
- `rp2350-otp` — D.7 / E16, E17, E28.
- `rp2350-m33-coprocessors` — D.8 RCP / E26.
- `rp2350-sio` — D.9 / E1, E2.
- `rp2350-memory` — D.10 XIP / E11.
- `rp2350-usb` — D.11 / E12.
