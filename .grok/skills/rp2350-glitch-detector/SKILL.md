---
name: rp2350-glitch-detector
description: "RP2350 glitch detector (GLITCH_DETECTOR, outline 10.9, PDF pages 787-790): use for arming, disarm, sensitivity, TRIG_STATUS, TRIG_FORCE, OTP flags, and glitch-detect reset. Trigger on glitch detector, setup/hold margin, HAD_GLITCH_DETECT, or /rp2350-glitch-detector."
---

# RP2350 glitch detector

PDF: `./RP-008373-DS-3-rp2350-datasheet.pdf`

Outline 10.9. Body starts on PDF page 787 and the last register table ends on PDF page 790, before outline 10.10. The index window 787–789 stops inside the register descriptions; extract PDF pages 787–790.

Run `pdftotext -layout -f 787 -l 790` on that PDF (one window; under 25 pages). Answer only from that extract. Paraphrase; cite the PDF page. Do not paste register tables or datasheet sentences into the answer.

Sibling skills to open only for the dependency named here: `rp2350-otp` (arm and sensitivity OTP flags), `rp2350-powman` (`CHIP_RESET` / `POWMAN_CHIP_RESET`), `rp2350-resets` (PSM, section 7.3), `rp2350-watchdog` (scratch registers cleared by a detector reset).

Facts read from this PDF (names, counts, addresses, pages only):

- Heading on PDF page 787: 10.9. Glitch detector
- One instance; control registers at `0x40158000`
- Four detectors: DET0–DET3
- Offsets: ARM `0x000`, DISARM `0x004`, SENSITIVITY `0x008`, LOCK `0x00c`, TRIG_STATUS `0x010`, TRIG_FORCE `0x014`
- OTP flags named: `GLITCH_DETECTOR_ENABLE`, `GLITCH_DETECTOR_EN`, `GLITCH_DETECTOR_SENS`
- ARM bits 15:0 reset `0x5bad` (decimal 23469 = do not arm); any other value arms
- DISARM yes value decimal 56495; ignored when ARM is yes
- SENSITIVITY: DEFAULT `222` means do not use OTP default; each DET* has a matching DET*_INV
- Armed trigger holds switched-core reset about 120 microseconds via the PSM
- Reset reason flag: `CHIP_RESET.HAD_GLITCH_DETECT` (also recorded in `POWMAN_CHIP_RESET`)
- Delay line: 75% to 120% of the minimum system clock period, steps of 15%
- LOCK bits 7:0: any nonzero value blocks writes to ARM, DISARM, SENSITIVITY, and LOCK
