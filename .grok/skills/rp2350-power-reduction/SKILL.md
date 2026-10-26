---
name: rp2350-power-reduction
description: "RP2350 power reduction strategies lookup (outline 6.5, PDF pages 446–450) for SLEEP, DORMANT, WAKE_EN/SLEEP_EN clock gates, SRAM and XIP power-down, and GPIO or AON wake. Trigger on sleep, dormant, clock gating, MEMPOWERDOWN, DORMANT_WAKE, or /rp2350-power-reduction."
---

# RP2350 Power reduction strategies

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`
PDF pages: 446–450 (pdftotext indexes, not the printed footer). Outline: 6.5. Power reduction strategies.

Run `pdftotext -layout -f N -l M` on that range, at most 25 pages per call. This range is 5 pages, so one window is enough. Answer only from that extract. Paraphrase; cite the PDF page. Do not paste register tables, example source, or datasheet sentences into the reply.

Subsections to open by question:

- 6.5 intro and 6.5.1 Top-level clock gates — PDF 446
- 6.5.2 SLEEP state — PDF 447
- 6.5.3 DORMANT state and 6.5.3.1 waking — PDF 447
- 6.5.4 Memory periphery power down — PDF 448
- 6.5.5 Full memory power down — PDF 448
- 6.5.6 Programmer’s model (sleep, then dormant) — PDF 448–450

Facts read from this PDF (not a register map):

- States named SLEEP and DORMANT; core power domains are outline 6.2.1 (PDF 446)
- Clock-gate sets: WAKE_ENx from WAKE_EN0, SLEEP_ENx from SLEEP_EN0; same bits (PDF 446)
- SLEEP requires both processors asleep and DMA idle; two GPIO banks (PDF 447)
- DORMANT register is in the active ROSC (outline 8.3) or XOSC (outline 8.2) (PDF 447)
- Wake names: TIMER.ALARM, DORMANT_WAKE, DORMANT_WAKE_INTE0 (PDF 447)
- Example sleep clock name on RP2350: CLK_REF_POWMAN; UART pair CLK_SYS_UART0 and CLK_PERI_UART0 (PDF 447–449)
- Periphery power-down: MEMPOWERDOWN in SYSCFG (outline 12.15.2); XIP CTRL.POWER_DOWN (PDF 448)
- SRAM window named for periphery down: SRAM0–SRAM9, 0x20000000–0x20081fff, plus USB DPRAM (PDF 448)
- Full-down domains: SRAM0 0x20000000–0x2003ffff (banks 0–3); SRAM1 0x20040000–0x20081fff (banks 4–9) (PDF 448)
- Third domain name: XIP (XIP cache and boot RAM), tied to the switched core domain (PDF 448)
- Power-number pointer: outline 14.9.7.2; memory list pointer: outline 4 (PDF 448)
- hello_dormant example pin: GPIO 10, high, XOSC dormant wake (PDF 449)

Sibling skills, only where this section defers or the mode depends on that block:

- Clock endpoints, XOSC, ROSC, and PLL stop/restart: `rp2350-clocks`
- AON Timer alarm wake (outline 12.10): `rp2350-always-on-timer`
- GPIO bank DORMANT_WAKE enables: `rp2350-gpio`
- DMA must be idle before SLEEP: `rp2350-dma`
- SRAM map, XIP cache, boot RAM: `rp2350-memory`
- SYSCFG MEMPOWERDOWN: `rp2350-sysctrl`
- POWMAN IRQ is optional for AON wake; CLK_REF_POWMAN: `rp2350-powman`
- Both processors (WFI/WFE, deep sleep): `rp2350-cortex-m33` and `rp2350-hazard3`
- Low-power current figures: `rp2350-electrical-mechanical`
