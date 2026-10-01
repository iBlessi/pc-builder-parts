# PC Builder Parts Dataset

An open, machine-readable dataset of current (2026) desktop PC components — CPUs, GPUs,
motherboards, RAM, storage, PSUs, and cases — together with the **compatibility rules** needed to
validate a build. It powers the free [TechFuelHQ PC Builder](https://techfuelhq.com/tools/pc-builder/) and is browsable
by category — with per-part verification dates — at [2026 PC part prices](https://techfuelhq.com/data/pc-part-prices-2026/),
and it's released under **CC BY 4.0** so you can fork it, build your own picker on top of it, or check
our numbers.

**This dataset carries no prices.** See [Why there are no prices](#why-there-are-no-prices) below.

## What's in it

A single file, [`pc-builder-parts.json`](pc-builder-parts.json) — 126 parts:

- `cpu`, `gpu`, `motherboard`, `ram`, `storage`, `psu`, `case` — arrays of parts. Each entry has its
  specs, a performance tier, use-cases, a short `notes` line, a `source` URL for its specification
  page, and a `last_verified` date.
- `_meta.compatibility_rules` — the rules the builder enforces: socket match, chipset support, RAM
  type, PSU headroom (sum of CPU TDP + GPU TGP + 150 W system overhead, with a 20% margin), case
  GPU-length clearance, and storage interface fit.
- `performance_tiers` — what `entry` / `mid` / `high` / `flagship` actually mean in resolution and FPS.

No API, no keys — it's just data.

## Example entry

```json
{
  "id": "cpu-r7-9800x3d",
  "vendor": "AMD",
  "model": "Ryzen 7 9800X3D",
  "cores": 8, "threads": 16,
  "socket": "AM5",
  "ram_type": "DDR5",
  "supported_chipsets": ["X870E", "X870", "B850", "B840", "X670E", "X670", "B650", "A620", "A620A"],
  "performance_tier": "flagship",
  "use_cases": ["1440p gaming", "4K gaming", "competitive gaming"],
  "notes": "Best gaming CPU on the market in 2026. 3D V-Cache transforms 1% lows. Get this if gaming is the only job.",
  "last_verified": "2026-07-24"
}
```

## Changes in v2.1.1 (2026-09-29)

Three corrections since v2.1.0. The part count is unchanged at 126, and every change is also
recorded in `_meta.note`.

- **AM5 chipset lists (2026-09-29).** Every AM5 CPU's `supported_chipsets` now lists all AM5
  chipsets AMD names: X870E, X870, B850, B840, X670E, X670, A620, A620A and B650. AMD's AM5 chipset
  page says all Socket AM5 motherboards are compatible with all Socket AM5 processors, with a BIOS
  update possibly required for Ryzen 8000 and 9000 on 600-series boards. The old four-chipset list
  made the PC Builder flag every AM5 CPU on an A620A board as incompatible.
- **Intel K-SKU `tdp_w` (2026-08-30).** Changed from Processor Base Power to Intel's published
  Maximum Turbo Power, because the PC Builder sums `tdp_w` as the power the chip actually draws.
  Non-K Intel rows were not changed.
- **`ram-ddr5-32gb-6000` note (2026-09-08).** It now separates the kit's EXPO overclocking profile
  from the CPU's stock memory support.

## Why there are no prices

Earlier versions of this dataset (through v1.1.0) carried a `price_usd` field. Those figures were
street-price observations taken substantially from Amazon.com. TechFuelHQ participates in the Amazon
Associates programme, whose terms do not permit republishing observed Amazon prices as a
redistributable dataset — and CC BY 4.0 is explicitly a redistribution licence.

All prices were therefore removed on **2026-08-01**, in two steps recorded in `_meta.price_removal`:
the 97 rows added in v2.0.0 lost `price_usd`, `price_source` and `price_asin`; the 29 pre-expansion
rows lost `price_usd` the same day, because their own v1.0 methodology note listed Amazon among its
retailer sources and no per-row attribution existed to clear any individual figure as non-Amazon.

Nothing else was removed. Every row's specifications and its `source` specification-page URL are
unchanged and remain individually verifiable.

## How specs and tiers are sourced

Specifications come from manufacturer product pages, TechPowerUp, and Tom's Hardware, each row
carrying a `source` URL and a `last_verified` date. Performance tiers are TechFuelHQ editorial
assessments from aggregate review consensus — **not** first-party benchmarks, except where a part is
anchored to a TechFuelHQ Open Bench Dataset (called out in that part's `notes`). The list is
deliberately curated, not exhaustive: every entry earns its place via the `notes` line.

## License

Copyright © 2026 TechFuel HQ — https://techfuelhq.com/

Released under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
The full, canonical legal text is in the [`LICENSE`](LICENSE) file. Use it anywhere, including
commercially — just attribute:

> TechFuelHQ PC Builder Parts Dataset (CC BY 4.0). https://techfuelhq.com/tools/pc-builder/

## Contributing

Spot a wrong spec or a dead source link? Open an issue or a pull request. Corrections with a source
are especially welcome. Please don't submit prices — see above.
