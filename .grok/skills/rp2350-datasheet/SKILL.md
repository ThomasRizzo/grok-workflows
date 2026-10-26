---
name: rp2350-datasheet
description: "Route RP2350 datasheet questions to the one matching child skill. Use when the question is about the RP2350 datasheet or /rp2350-datasheet."
---

# RP2350 datasheet router

This skill only routes. It does not answer from the datasheet.

Open the one child skill whose topic, outline number, or PDF page range matches the question. The skill file is `.grok/skills/<skill_name>/SKILL.md`. Read that file and follow it. Do not follow more than one child skill for a single question. If two ranges could apply, pick the narrower match. If none match, say so. Do not invent pages, registers, or behavior from this list.

Catalog fields below are skill name, outline number, title, kind, and PDF page range. PDF page numbers are datasheet start pages, not printed footer numbers.

| Skill | Outline | Title | Kind | PDF pages |
| --- | --- | --- | --- | --- |
| rp2350-introduction | 1 | Introduction | section | 14-22 |
| rp2350-system-bus | 2 | System bus | section | 23-32 |
| rp2350-sio | 3.1 | SIO | peripheral | 34-80 |
| rp2350-debug | 3.5 | Debug | section | 84-96 |
| rp2350-m33-coprocessors | 3.6 | Cortex-M33 coprocessors | section | 97-116 |
| rp2350-cortex-m33 | 3.7 | Cortex-M33 processor | section | 117-226 |
| rp2350-hazard3 | 3.8 | Hazard3 processor | section | 227-309 |
| rp2350-memory | 4 | Memory | section | 312-326 |
| rp2350-bootrom | 5 | Bootrom | peripheral | 327-400 |
| rp2350-core-regulator | 6.3 | Core voltage regulator | peripheral | 406-414 |
| rp2350-powman | 6.4 | Power management (POWMAN) registers | peripheral | 415-445 |
| rp2350-power-reduction | 6.5 | Power reduction strategies | section | 446-450 |
| rp2350-resets | 7 | Resets | section | 451-467 |
| rp2350-clocks | 8 | Clocks | peripheral | 468-538 |
| rp2350-gpio | 9 | GPIO | peripheral | 539-734 |
| rp2350-access-control | 10.6 | Access control | peripheral | 740-784 |
| rp2350-glitch-detector | 10.9 | Glitch detector | peripheral | 787-789 |
| rp2350-pio | 11 | PIO | peripheral | 792-871 |
| rp2350-uart | 12.1 | UART | peripheral | 872-892 |
| rp2350-i2c | 12.2 | I2C | peripheral | 893-943 |
| rp2350-spi | 12.3 | SPI | peripheral | 944-963 |
| rp2350-adc | 12.4 | ADC and Temperature Sensor | peripheral | 964-973 |
| rp2350-pwm | 12.5 | PWM | peripheral | 974-1010 |
| rp2350-dma | 12.6 | DMA | peripheral | 1011-1058 |
| rp2350-usb | 12.7 | USB | peripheral | 1059-1094 |
| rp2350-system-timers | 12.8 | System timers | peripheral | 1095-1105 |
| rp2350-watchdog | 12.9 | Watchdog | peripheral | 1106-1109 |
| rp2350-always-on-timer | 12.10 | Always-on timer | peripheral | 1110-1113 |
| rp2350-hstx | 12.11 | HSTX | peripheral | 1114-1122 |
| rp2350-trng | 12.12 | TRNG | peripheral | 1123-1131 |
| rp2350-sha-256 | 12.13 | SHA-256 accelerator | peripheral | 1132-1136 |
| rp2350-qmi | 12.14 | QSPI memory interface (QMI) | peripheral | 1137-1164 |
| rp2350-sysctrl | 12.15 | System Control Registers | peripheral | 1165-1180 |
| rp2350-otp | 13 | OTP | peripheral | 1181-1324 |
| rp2350-electrical-mechanical | 14 | Electrical and mechanical | section | 1325-1346 |
| rp2350-errata | D | Errata | section | 1352-1368 |
