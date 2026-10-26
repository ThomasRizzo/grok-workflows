---
name: rp2350-dma
description: "Lookup for the RP2350 DMA controller, outline 12.6, PDF pages 1011-1058. Use for channel count, DMA_BASE, DREQ/TREQ pacing, chaining, CRC sniffer, security and MPU, bus errors, and register offsets. Trigger: /rp2350-dma."
---

# RP2350 DMA

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF page range: 1011–1058 (`pdftotext -f` / `-l` indexes, not the printed footer). Outline: 12.6. On the start page the printed footer is 1010, one less than the PDF index.

Run `pdftotext -layout` on that range in windows of at most 25 pages. Read 1011–1022 first (heading, channels, DREQ table), then 1023–1047 (examples, register list, `TREQ_SEL`), then 1048–1058 (security, MPU, debug offsets). Answer only from that extract. Paraphrase; cite the PDF page index. Do not paste register tables or bitfield text.

## Facts

- Heading on PDF page 1011: 12.6. DMA.
- One controller. Base `0x50000000` (`DMA_BASE`). Table 1255 starts on PDF page 1026; bitfields continue through 1058.
- 16 channels, stride `0x40`. `CH0_READ_ADDR` `0x000`, `WRITE_ADDR` `0x004`, `TRANS_COUNT` `0x008`, `CTRL_TRIG` `0x00c`. `N_CHANNELS` at `0x468` (equipped count, up to 16).
- Four shared IRQs. `INTR` `0x400`; `INTE0` starts at `0x404`. SDK example name: `DMA_IRQ_0`.
- `TREQ_SEL` is `CTRL` bits 22:17 (PDF 1043–1044): `0x0`–`0x3a` select DREQ n; 59–62 = TIMER0–TIMER3; 63 = permanent (unpaced). Named DREQs 0–54 are on PDF 1016–1017: PIO0–PIO2 TX/RX, SPI0/1, UART0/1, PWM wrap 0–11, I2C0/1, ADC, XIP stream, XIP QMI TX/RX, HSTX, CORESIGHT, SHA256.
- Four pacing timers, `TIMER0`–`TIMER3` at `0x440`, `0x444`, `0x448`, `0x44c`. Pace from `clk_sys` (`sys_clk`); at most one TREQ per cycle.
- `MULTI_CHAN_TRIGGER` `0x450`. `SNIFF_CTRL` `0x454`, `SNIFF_DATA` `0x458`. `CHAN_ABORT` `0x464`.
- Security: `SECCFG_CH0` `0x480` and one register per channel; `SECCFG_IRQ3` `0x4cc`; `SECCFG_MISC` `0x4d0`. MPU: `MPU_CTRL` `0x500`; eight regions, `MPU_BARn` `0x504 + 8k`, `MPU_LARn` `0x508 + 8k`, 32-byte granularity.
- Debug counters: `CH0_DBG_CTDREQ` `0x800`, then `+ 0x40` per channel through CH15.
- No DMA pins. SIO is not visible from the DMA AHB5 ports. ACCESSCTRL can reject a DMA or privilege level. Example APB fault: ADC while `clk_adc` is stopped (timeout 65535 cycles).
- `TRANS_COUNT` bits 31:28 are MODE. One sequence holds at most 2^28−1 transfers. Mode 0 matches RP2040; mode 1 retriggers; mode `0xf` does not decrement.
- Channels 1 and above reset `CHAIN_TO` to channel 0. Each channel has AL1–AL3 aliases in the same `0x40` block. Writing 0 to a trigger register is a null trigger and does not start the channel.

## Siblings

Name only when the question crosses that block:

- `rp2350-pio`, `rp2350-spi`, `rp2350-uart`, `rp2350-pwm`, `rp2350-i2c`, `rp2350-adc` — peripheral DREQ sources.
- `rp2350-qmi` — `DREQ_XIP_QMITX` / `DREQ_XIP_QMIRX` (and XIP stream alongside memory).
- `rp2350-hstx`, `rp2350-sha-256`, `rp2350-debug` — HSTX, SHA256, and CORESIGHT DREQs.
- `rp2350-clocks` — pacing uses `clk_sys`; a stopped `clk_adc` can fault an ADC DMA access.
- `rp2350-access-control` — bus-endpoint and DMA-manager permission checks.
- `rp2350-sio` — SIO addresses are not reachable from the DMA ports.
- `rp2350-memory` — memory-to-memory and XIP buffers; the DMA MPU is meant to mirror SRAM/XIP security.
