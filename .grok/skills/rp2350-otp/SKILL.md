---
name: rp2350-otp
description: "RP2350 OTP block (outline 13) on datasheet PDF pages 1181-1324. Use for OTP_BASE, data aliases, page locks, SBPI, ECC rows, clk_ref limit, RMA, and predefined OTP rows. Trigger: /rp2350-otp."
---

# RP2350 OTP

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF pages 1181–1324. Outline 13. OTP. The printed footer on a page in this range is one less than that PDF index.

Run `pdftotext -layout -f <start> -l <end>` on this range, in windows of at most 25 pages. Do not load the whole range at once. Answer only from that extract. Paraphrase; cite the PDF page index. Do not copy register tables or description text into the reply.

Sibling skills, only where this block’s procedure uses them: `rp2350-bootrom` (bootrom `otp_access` and USB PICOBOOT program OTP; outline 5), `rp2350-clocks` (`clk_ref` after the OTP boot oscillator). No GPIO pin function in this section.

Facts read from this range:

- One instance; control base `0x40120000` (`OTP_BASE`); 128 kB region (PDF page 1181).
- Read aliases: `0x40130000` `OTP_DATA_BASE`, `0x40134000` `OTP_DATA_RAW_BASE`, `0x40138000` `OTP_DATA_GUARDED_BASE`, `0x4013c000` `OTP_DATA_RAW_GUARDED_BASE` (PDF page 1181).
- 4096 rows × 24 bits; 64 pages × 64 rows; ECC capacity 8192 bytes (PDF page 1181).
- `clk_ref` must not exceed 25 MHz while accessing OTP; boot oscillator first (section 13.3, PDF page 1183).
- `SW_LOCK0`–`SW_LOCK63` at offsets `0x000`–`0x0fc` (register list starts PDF page 1191).
- `SBPI_INSTR` `0x100`; `SBPI_WDATA_0`–`3` `0x104`–`0x110`; `SBPI_RDATA_0`–`3` `0x114`–`0x120`; `SBPI_STATUS` `0x124`.
- `USR` `0x128`; `DBG` `0x12c`; `BIST` `0x134`; `CRT_KEY_W0`–`W3` `0x138`–`0x144`; `CRITICAL` `0x148`; `KEY_VALID` `0x14c`.
- `DEBUGEN` `0x150`; `DEBUGEN_LOCK` `0x154`; `ARCHSEL` `0x158`; `ARCHSEL_STATUS` `0x15c`; `BOOTDIS` `0x160`.
- `INTR` `0x164`; `INTE` `0x168`; `INTF` `0x16c`; `INTS` `0x170`.
- Section 13.10 predefined OTP rows starts on PDF page 1208 (`CHIPID0` row `0x000`).
- Section heading `13. OTP` is on PDF page 1181; register detail continues through PDF page 1324.
