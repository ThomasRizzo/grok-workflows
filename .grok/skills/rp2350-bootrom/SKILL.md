---
name: rp2350-bootrom
description: "Look up the RP2350 Bootrom (outline 5, PDF pages 327-400): boot sequence, BOOTSEL, partition tables, IMAGE_DEF, secure boot, ROM APIs, UF2/PICOBOOT, and UART boot. Trigger on bootrom, BOOTSEL, UF2, PICOBOOT, IMAGE_DEF, partition table, or /rp2350-bootrom."
---

# RP2350 Bootrom

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF page range: 327–400 (`pdftotext -f` / `-l` indexes, not the printed footer numbers).
Outline: 5. Bootrom.

Run `pdftotext -layout` on that range, in windows of at most 25 pages (327–351, 352–376, 377–400). Answer only from that extract. Paraphrase; cite the PDF page index. Do not paste datasheet sentences, tables, or register descriptions into the answer.

There is no bootrom MMIO register map in this chapter. Fixed ROM words, OTP flags, POWMAN boot registers, and watchdog scratch registers are the lookup targets. When the question is the ROM memory map, OTP pages, POWMAN `BOOT0`–`BOOT3`, watchdog `SCRATCH4`–`SCRATCH7`, QMI `ATRANS`, XOSC/USB PLL, QSPI pad functions, or the USB device used by the bootloaders, use `rp2350-memory`, `rp2350-otp`, `rp2350-powman`, `rp2350-watchdog`, `rp2350-qmi`, `rp2350-clocks`, `rp2350-gpio`, and `rp2350-usb`. Arm versus RISC-V entry differences also need `rp2350-cortex-m33` or `rp2350-hazard3`. UART boot is not the PL011 UART.

Suggested windows: concepts and boot sequence in 327–351 (5.1–5.3); ROM API addresses and USB/PICOBOOT in 346–376 (5.4–5.6); white-label, UART boot, metadata, and examples in 377–400 (5.7–5.10).

Facts (confirm in the extract; each is one short lookup, not a description):

- Heading `5. Bootrom` is on PDF page 327. One 32 kB mask ROM.
- Arm fixed words at `0x00000000`; magic `'M','u',0x02` at `0x00000010`; version byte `0x00000013`; `BOOTROM_ROMTABLE_START` at `0x00000014` (PDF 347).
- RISC-V ROM table pointer `0x00007df6`; entry instruction `0x00007dfc` (PDF 348, Table 465).
- Partition table optional; at most 16 partitions; offsets in 4 kB sectors; bootable partitions wholly in the first 16 MB (PDF 328).
- Default runtime flash base `0x10000000` via QMI `ATRANS0`–`ATRANS3`; translated window to `0x11000000` (PDF 336).
- Boot inputs: QSPI CS0 flash, POWMAN `BOOT0`–`BOOT3`, watchdog `SCRATCH4`–`SCRATCH7`, OTP `CRIT1` / `BOOT_FLAGS0` / `BOOT_FLAGS1`, QSPI CSn, QSPI SD1 (PDF 337).
- Vector magic `0xb007c0d3`; entry XOR `0x4ff83f2d`. Special `pc` `0xb007c0d3` with `sp` 2 = BOOTSEL, 3 = RAM image, 4 = flash update (PDF 342).
- BOOTSEL: QSPI CSn low. SD1 low = USB; SD1 high = UART. Default XOSC 12 MHz; USB PLL 48 MHz. `BOOT_FLAGS0` and `BOOT_FLAGS1` are OTP page 1 (PDF 345–346).
- UART boot: TX QSPI SD2, RX QSPI SD3, 1 Mbaud from 48 MHz; link at `0x20000000` (PDF 379–380).
- USB defaults: `idVendor` `0x2e8a`, `idProduct` `0x000f` (OTP-overridable). PICOBOOT `dMagic` `0x431fd10b` (PDF 370–371, Tables 470 and 472).
- Block loop HEADER word `0xffffded3` (Table 487, PDF 381); FOOTER word `0xab123579` in that table.
- UF2 family IDs in Table 469 (section 5.5.3): `rp2350_arm_s` `0xe48bff59`, `rp2350_riscv` `0xe48bff5a`, `rp2350_arm_ns` `0xe48bff5b`.
