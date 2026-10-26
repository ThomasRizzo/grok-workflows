---
name: rp2350-debug
description: "Lookup for the RP2350 Debug block, outline 3.5, PDF pages 84-96. Use for SWD, SWCLK, SWDIO, SW-DP, Mem-AP, RP-AP, self-hosted CoreSight, RISC-V debug module, trace FIFO, rescue reset, or debug-disable keys. /rp2350-debug"
---

# RP2350 Debug

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF pages 84–96. Outline 3.5. Debug.

Run `pdftotext -layout -f 84 -l 96` on that PDF. The range is under 25 pages; one window is enough. If you split it, stay at or under 25 pages per call. Answer only from that extract. Paraphrase; cite the PDF page index (not the printed footer). Do not copy register bit descriptions into the reply.

Depends on `/rp2350-powman` (switched-core power-up before most APs), `/rp2350-clocks` (`clk_sys` and LPOSC), `/rp2350-gpio` (SWD pads, JTAG mux, trace port), `/rp2350-access-control` (Mem-AP/SBA filters and trace FIFO DMA enable), `/rp2350-dma` (trace FIFO DREQ), `/rp2350-otp` (debug-disable flags and keys), `/rp2350-bootrom` (rescue halt), `/rp2350-resets` (RUN and `CHIP_RESET.RESCUE_FLAG`), `/rp2350-watchdog` (switched-core reset), `/rp2350-cortex-m33` (Arm debug registers, section 3.7.5), and `/rp2350-hazard3` (RISC-V debug, section 3.8.5).

Facts read from pages 84–96:

- One SW-DP; dedicated pins SWCLK and SWDIO (tables 1677 and 1687); dormant at power-up or RUN (PDF 84)
- Debug-space offsets: ROM `0x00000`; AHB-AP core 0 `0x02000`; AHB-AP core 1 `0x04000`; timestamp `0x06000`; ATB funnel `0x07000`; TPIU `0x08000`; CTI `0x09000`; APB-AP `0x0a000`; RP-AP `0x80000` (PDF 84)
- Self-hosted window `CORESIGHT_PERIPH_BASE` `0x40140000`; same AP offsets; RP-AP is not in that window (PDF 86)
- Two AHB5 Mem-APs (two Cortex-M33s); one APB Mem-AP to one RISC-V DM (spec 0.13.2); hart IDs 0 and 1; DM at APB offset 0, spec addresses ×4 (PDF 84–86)
- SW-DP and RP-AP always-on; other APs need POWMAN switched-core power-up, OTP PSM flags, and `clk_sys`; poll `CDBGPWRUPACK` after `CDBGPWRUPREQ`; LPOSC sequences that power-up (PDF 84, 86)
- `SYSCFG` `DBGFORCE` detaches the pads for software SWD bitbang; a core must not use its own AHB-AP through the self-hosted window (PDF 86)
- Trace is Arm-only: TPIU 4-bit DDR on GPIOs; clock `clk_sys`/2 (75 MHz at 150 MHz `clk_sys`, up to 600 Mb/s); FIFO `CORESIGHT_TRACE_BASE` `0x50700000`, `CTRL_STATUS` `0x000`, `TRACE_CAPTURE_FIFO` `0x004` (8×32); DMA DREQ 53 after ACCESSCTRL `CORESIGHT_TRACE` DMA bit (PDF 87–88)
- RP-AP at debug offset `0x80000`: `CTRL` `0x000`, `DBGKEY` `0x004`, `DBG_POW_STATE_SWCORE` `0x008`, `DBG_POW_STATE_XIP` `0x00c`, `DBG_POW_STATE_SRAM0` `0x010`, `DBG_POW_STATE_SRAM1` `0x014`, `DBG_POW_OVRD` `0x018`, `DBG_POW_OUTPUT_TO_GPIO` `0x01c`, `IDR` `0xdfc` (PDF 89–96)
- `CTRL.JTAG_FUNCSEL` muxes JTAG onto GPIO0–3; `CTRL.DBG_FRCE_GPIO_LPCK` drives LPOSC from gpio20 (80-pin) or gpio34 (60-pin) (PDF 91)
- Rescue: set then clear RP-AP `CTRL.RESCUE_RESTART`; sets `CHIP_RESET.RESCUE_FLAG`; bootrom halts before user code (PDF 89)
- OTP: `CRIT1.DEBUG_DISABLE` and `CRIT1.SECURE_DEBUG_DISABLE`; 128-bit keys 5 and 6; software re-enable is OTP `DEBUGEN`, lock is `DEBUGEN_LOCK` (PDF 89)
- Mem-AP and RISC-V SBA accesses are separate from processor accesses for ACCESSCTRL bus filters (PDF 84–86)
