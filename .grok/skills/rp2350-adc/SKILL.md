---
name: rp2350-adc
description: "Look up the RP2350 ADC and temperature sensor (outline 12.4, PDF pages 964-973): channels, GPIO pins, clk_adc, FIFO, DMA, and registers. Trigger on ADC, AINSEL, temperature sensor, ADC_BASE, or /rp2350-adc."
---

# RP2350 ADC and Temperature Sensor

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF page range: 964–973 (`pdftotext -f` / `-l` indexes, not the printed footer numbers).
Outline: 12.4 ADC and Temperature Sensor.

Run `pdftotext -layout` on that range, in windows of at most 25 pages (this range fits in one window). Answer only from that extract. Paraphrase; cite the PDF page index. Do not paste register tables or datasheet sentences into the answer.

Shared pins use GPIO pad control, `clk_adc` comes from the clock generators (USB PLL is the usual source), the block stays in reset until released, and streaming uses DMA `DREQ_ADC`. ENOB and absolute limits are in the electrical specifications (section 14.9, Table 1685). When those details matter, use `rp2350-gpio`, `rp2350-clocks`, `rp2350-resets`, `rp2350-dma`, and `rp2350-electrical-mechanical`.

Facts (confirm in the extract; each is one short lookup, not a description):

- Heading `12.4. ADC and Temperature Sensor` is on PDF page 964.
- One ADC; base `0x400a0000` (`ADC_BASE`); register list on PDF page 971.
- `clk_adc` 48 MHz; 96 cycles per conversion; 500 kS/s (PDF page 968).
- QFN-60: GPIO26–29 = channels 0–3; temperature = channel 4 (PDF page 967).
- QFN-80: GPIO40–47 = channels 0–7; temperature = channel 8 (PDF page 967).
- 12-bit SAR, 9.2 ENOB, eight-entry sample FIFO (PDF page 964).
- Offsets: CS `0x000`, RESULT `0x004`, FCS `0x008`, FIFO `0x00c`, DIV `0x010`, INTR `0x014`, INTE `0x018`, INTF `0x01c`, INTS `0x020`.
- DMA: `ADC_DREQ` / `DREQ_ADC`; enable with FCS.DREQ_EN (PDF page 970).
- Temperature bias is CS.TS_EN; AINSEL 4 (QFN-60) or 8 (QFN-80) (PDF page 970).
- Shared ADC pins: pad IE low and OD high (section 9.11.3, PDF page 966).
- Maximum ADC input follows IOVDD, not ADC_AVDD (PDF pages 966–967).
- Release the ADC reset before CS.EN; CS.READY marks analogue power-up done (PDF page 968).
