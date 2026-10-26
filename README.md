# grok-workflows

Grok workflows and the lookup skills they produce. The first workflow turns a long component datasheet into one skill per section or peripheral.

## Datasheet skills

`datasheet-skills` reads a PDF outline, labels chapters, and writes a lookup skill for each selected block. Each skill tells an agent which PDF pages to extract. It does not copy the datasheet.

Run it from a Grok session in this repo:

```text
/datasheet-skills
```

Works for microcontrollers and for peripheral ICs (ADC, DAC, sensors, drivers, memories). Pass `args.pdf` (path to a `.pdf`) and `args.part` (a short slug such as `rp2350` or `ads1115`). Optional args:

| Arg | Default | Meaning |
| --- | --- | --- |
| `device_class` | `auto` | `mcu`, `peripheral`, or `auto`; picks the block vocabulary |
| `interfaces` | `[]` | Host interfaces to emphasize, e.g. `["i2c"]` |
| `extra_blocks` | `[]` | Heading names that must become their own skills |
| `skills_dir` | `.grok/skills` | Where skills are written |
| `max_skills` | 36 | Cap on section and peripheral skills (1–48) |
| `split_pages` | 90 | Split a top-level section longer than this |
| `min_pages` | 2 | Drop exploded sections shorter than this |

Page numbers are PDF indexes (`pdftotext -f`), not the printed page numbers.

Phases: Outline, Select, Write, Verify, Index. After Select, the run pauses with a plan in `catalog.md`. Resume to write and verify the skills. The flow diagram is [`.grok/workflows/datasheet-skills.html`](.grok/workflows/datasheet-skills.html). The script is [`.grok/workflows/datasheet-skills.rhai`](.grok/workflows/datasheet-skills.rhai).

## RP2350 skills

`.grok/skills/` holds the skills generated for the RP2350. `rp2350-datasheet` is the router. Open that skill, or name a child such as `/rp2350-uart`, and the agent follows the matching page range in the vendored PDF.

The PDF in this repo:

`RP-008373-DS-3-rp2350-datasheet.pdf`

That file is the RP2350 Datasheet by Raspberry Pi Ltd, included here unchanged. It is licensed under [Creative Commons Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)](https://creativecommons.org/licenses/by-nd/4.0/). Portions are copyright Synopsys, Inc. and Arm Limited, used with permission. The official copy is published at [pip.raspberrypi.com](https://pip.raspberrypi.com/documents/RP-008373-DS-rp2350-datasheet.pdf) (the older datasheets.raspberrypi.com link redirects there).

The workflow, the flow diagram, and the skill files are separate from that datasheet. They cite page ranges and do not replace the PDF.
