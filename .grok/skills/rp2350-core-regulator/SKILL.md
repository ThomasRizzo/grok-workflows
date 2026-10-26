---
name: rp2350-core-regulator
description: "Look up the RP2350 core voltage regulator (outline 6.3, PDF pages 406-414): DVDD modes, VREG and VREG_CTRL, Power Manager LP entry/exit, VREG_VIN/VREG_AVDD/VREG_LX, and external components. Triggers on core regulator, DVDD, VREG, VSEL, HIZ, or /rp2350-core-regulator."
---

# Core voltage regulator

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF page range: 406–414 (pdftotext `-f`/`-l` indexes, not the printed footer). Outline: 6.3.

Run `pdftotext -layout -f 406 -l 414` on that path. The range is 9 pages, so one window is enough; never extract more than 25 pages at a time. Answer only from that extract. Paraphrase; cite the PDF page index. Do not paste datasheet sentences, tables, or a register map into the reply.

This block is controlled with the Power Manager on low-power transitions. For register ownership, sequencing, and the LP state machine, use `rp2350-powman`. Electrical limits referenced as Section 14.9.6 belong in `rp2350-electrical-mechanical`.

Facts read from this range:

- Heading on PDF 406: `6.3. Core voltage regulator`; body ends in 6.3.8.3 on PDF 414.
- One on-chip regulator; intended load is digital core supply DVDD.
- Named pins: VREG_VIN, VREG_AVDD, VREG_LX, VREG_PGND, VREG_FB.
- Modes: Normal (switching), low-power (linear; Power Manager only), high-impedance (`VREG.HIZ`).
- Software path: `VREG_CTRL.UNLOCK` once, then `VREG` (`VSEL`, `HIZ`, `UPDATE_IN_PROGRESS`).
- `VREG_CTRL` also names `DISABLE_VOLTAGE_LIMIT` and `HT_TH`.
- `VREG_STS` fields: `VOUT_OK`, `STARTUP`.
- LP values: `VREG_LP_ENTRY` and `VREG_LP_EXIT` (extra `MODE` field).
- No base address and no register map in this section.
- Current-limit and regulator electrical detail: Section 14.9.6.
- Figures 22–28 (PDF 408–414); inductor AOTA-B201610S3R3-101-T; QFN-60 DVDD pin 23 noted for a second output capacitor.
- Startup default: enabled, Normal mode, nominal 1.1 V output.
