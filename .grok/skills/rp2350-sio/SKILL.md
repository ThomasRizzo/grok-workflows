---
name: rp2350-sio
description: "Look up the RP2350 SIO block (single-cycle IO, outline 3.1, PDF pages 34-80) for CPUID, GPIO bitbang, spinlocks, inter-core FIFOs, doorbells, interpolators, TMDS, and the RISC-V platform timer. Use when the question is SIO_BASE, Secure/Non-secure SIO banks, FIFO or doorbell IRQs, GPIO_OUT, or /rp2350-sio."
---

# RP2350 SIO

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

PDF pages 34-80. Outline 3.1. SIO.

Run `pdftotext -layout -f N -l M` on that range, in windows of at most 25 pages. Answer only from that extract. Paraphrase; cite the PDF page. Do not paste register-description tables.

Read overview on pages 34-41, then the register list from page 50. GPIO pin mux and function select are not in this block: use `rp2350-gpio`. Non-secure GPIO filtering is `ACCESSCTRL` `GPIO_NSMASK0`/`GPIO_NSMASK1`: use `rp2350-access-control`. `MTIME` can use the system tick generator (section 8.5): use `rp2350-clocks`. Alias hazard RP2350-E2 is in `rp2350-errata`.

## Facts

- `SIO_BASE` `0xd0000000` selects the Secure or Non-secure bank from the access; Secure code can see the Non-secure bank at `SIO_NONSEC_BASE` `0xd0020000`.
- One SIO, one bus interface per core; FIFOs, doorbells, spinlocks, and IRQ outputs are duplicated Secure/Non-secure. GPIO registers are shared.
- `CPUID` reads 0 on core 0 and 1 on core 1 (same values as Hazard3 `MHARTID`).
- 32 spinlocks per bank, `SPINLOCK0`–`SPINLOCK31` at `0x100`–`0x17c`. Same-cycle claim: core 0 wins. RP2350-E2: writes above offset `0x180` alias the spinlocks.
- Two mailboxes per bank, 32-bit by 4 deep. `FIFO_ST` `0x050`, `FIFO_WR` `0x054`, `FIFO_RD` `0x058`. `SIO_IRQ_FIFO` 25; `SIO_IRQ_FIFO_NS` 27.
- 8 doorbell flags each way. `DOORBELL_OUT_SET` `0x180`, `DOORBELL_OUT_CLR` `0x184`, `DOORBELL_IN_SET` `0x188`, `DOORBELL_IN_CLR` `0x18c`. `SIO_IRQ_BELL` 26; `SIO_IRQ_BELL_NS` 28.
- Fast GPIO: `GPIO_IN`/`GPIO_OUT`/`GPIO_OE` and SET/CLR/XOR for GPIO0–31 from `0x004`; `GPIO_HI_*` covers GPIO32–47 plus QSPI and USB pins. Same-cycle OUT/OE writes: core 0 then core 1.
- Two interpolators per core (`INTERP0` from `0x080`, `INTERP1` from `0x0c0`) and one TMDS encoder per core (`TMDS_CTRL` `0x1c0` through `TMDS_POP_DOUBLE_L2` `0x1e4`). `PERI_NONSEC` `0x190` assigns them to the Non-secure bank.
- RISC-V 64-bit platform timer is Secure-bank only, shared counter `MTIME`/`MTIMEH` `0x1b0`/`0x1b4`, per-core `MTIMECMP`/`MTIMECMPH` `0x1b8`/`0x1bc`, `MTIME_CTRL` `0x1a4` (tick generator or system clock). Drives `SIO_IRQ_MTIMECMP`.
- `RISCV_SOFTIRQ` `0x1a0`. Integer divider from RP2040 is absent; that address space is reserved.
- Register list: outline 3.1.11, PDF page 50. Detail registers continue through PDF page 80.
