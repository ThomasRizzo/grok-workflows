---
name: rp2350-m33-coprocessors
description: "Lookup for the RP2350 Cortex-M33 coprocessors, outline 3.6, PDF pages 97-116. Use for GPIOC, DCP, RCP, CPACR or NSACR enable, coprocessor numbers, and FPU ports. /rp2350-m33-coprocessors"
---

# RP2350 Cortex-M33 coprocessors

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF pages 97–116. Outline 3.6. Cortex-M33 coprocessors.

Run `pdftotext -layout -f 97 -l 116` on that PDF. The window is under 25 pages. Answer only from that extract. Paraphrase; cite the PDF page index, not the printed footer. Page 97 starts in the debug AP register list before 3.6. This window ends in 3.6.3.7.7 (`rcp_panic`). Outline 3.6.4 (FPU) and the start of 3.7 are on PDF page 117; if the question is the FPU, also run `pdftotext -layout -f 117 -l 117`. Do not invent an MMIO base address, pin function, or clock: these blocks are coprocessor-port instructions.

Depends on `/rp2350-sio` (GPIOC targets SIO GPIO registers), `/rp2350-access-control` (Non-secure GPIO masks), `/rp2350-cortex-m33` (CPACR, NSACR, CPACR_NS), and `/rp2350-bootrom` plus `/rp2350-trng` (core 0 salts RCP from the TRNG on the early boot path). RISC-V cores do not use these ports; do not route this block through `/rp2350-hazard3`.

Facts read from pages 97–117:

- Three device-specific coprocessors, plus the built-in single-precision FPU; enable in CPACR (Secure) or NSACR and CPACR_NS (Non-secure); PDF 97
- GPIOC is coprocessor port 0; one instruction can sample 48 GPIOs or write SIO GPIO output or output-enable; PDF 97
- Non-secure GPIOC uses ACCESSCTRL `GPIO_NSMASK0` and `GPIO_NSMASK1`; denied pins ignore writes and read as zero; PDF 97
- DCP: two instances per Cortex-M33; coprocessor 4 is the current security state; coprocessor 5 is the Non-secure instance and Secure-only; PDF 100
- DCP state is six 32-bit words; status is 9 bits, including the engaged flag; PDF 101
- SDK assembler macros live in `dcp_instr.inc.S` and emit `p4` (example `mcrr p4,#1,...` for WXUP); PDF 104
- RCP: one instance per Cortex-M33, coprocessor 7; a fault on one core faults the other; PDF 108
- RCP salt is 64 bits per core; core 0 writes both; invalid salt or a second salt write faults; bootrom seeds both from the TRNG; PDF 109
- Non-secure RCP: NSACR bit 7 then CPACR_NS bit 7; Non-secure cannot raise RCP faults or read the salt; PDF 109
- Default RCP delay is 0–127 cycles from salt bits 63:40; the `*2` opcode variants skip the delay; PDF 112
- RCP 32-bit boolean true is `0xa500a500` and false is `0x00c300c3`; other patterns are poison; PDF 114
- FPU is the standard Arm single-precision unit on coprocessor ports 10 and 11; SDK `float` uses it, `double` uses the DCP; PDF 117
