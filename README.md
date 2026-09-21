# Northstar Mining Operations Analytics — GMG-Aligned Power BI Portfolio

A portfolio project showcasing an end-to-end mining-operations analytics build in Power BI: a
synthetic dataset for a fictional mine ("Northstar"), a semantic model implementing the
[Global Mining Guidelines (GMG) Time Usage Model](https://gmggroup.org/), and a finished
Power BI report built from it.

> All data is synthetic / simulated. No client or proprietary data is used anywhere in this repo.

## What this project demonstrates

- Modeling a real industry standard (GMG Time Usage Model) in a star schema, instead of an
  ad-hoc flat status model.
- A single source-of-truth fact table (`FactState`) storing one terminal system status per
  equipment × 30-minute slice, with Calendar/Scheduled/Available/Operating/Working Time all
  derived via DAX measures rather than stored as overlapping events — avoiding double-counting.
- A full measure library (`_measures` table) covering availability, utilization, working
  efficiency, production rate, reliability/maintenance, delay & standby, and a
  schedule → availability → utilization → efficiency → rate variance bridge.
- A shippable Power BI project in `.pbip` format (report + semantic model as source-controllable
  TMDL/JSON), ready to open directly in Power BI Desktop.

## Time Usage Model

`FactState` never stores CT/ST/AT/OT/WT as separate rows — they are aggregate measures derived
from `DimStatus`-flagged terminal leaf codes. The hierarchy reconciles exactly:

```
CT = ST + UT
ST = AT + DT
AT = OT + SB
SB = SBO + SBE
OT = WT + OD
WT = PT + NP
```

| Code | Meaning |
|------|---------|
| CT | Calendar Time |
| ST | Scheduled Time |
| UT | Unscheduled (non-scheduled) Time |
| AT | Available Time |
| DT | Down Time |
| OT | Operating Time |
| SB | Standby (SBO = Operating Standby, SBE = External Standby) |
| WT | Working Time |
| OD | Operating Delay |
| PT | Productive Time |
| NP | Non-Productive Time (still working time — not merged into delay) |

Key headline metrics on the synthetic fleet: Physical Availability (AT/ST) **90.14%**,
Use of Availability (OT/AT) **68.24%**, Working Efficiency (WT/OT) **80.76%**, Productive
Share of Working (PT/WT) **91.16%**, Productive Efficiency (PT/OT) **73.62%**. All six GMG
reconciliation identities resolve to 0.0 h (see `Northstar_GMG_PowerBI_Portfolio_Pack/data/validation_summary.json`).

## Repository layout

```
livrable/                              Finished Power BI project (.pbip) — open portfolio.pbip in Power BI Desktop
  portfolio.Report/                    Report definition (pages, visuals, static resources)
  portfolio.SemanticModel/             Semantic model definition (TMDL tables, relationships, measures)

Northstar_GMG_PowerBI_Portfolio_Pack/  Source pack used to build the model above
  data/                                Synthetic star-schema CSVs (Dim*/Fact*) + validation summary
  docs/                                Data dictionary, DAX measure library, dashboard design notes
  model/                               TMDL measures file + Power BI model setup instructions
  README.md                            Pack-specific notes on the GMG hierarchy
```

## Getting started

1. Open `livrable/portfolio.pbip` in Power BI Desktop (2024+, PBIP support enabled) to explore
   the finished report and model directly.
2. To rebuild from source: import the CSVs in `Northstar_GMG_PowerBI_Portfolio_Pack/data/`
   following `Northstar_GMG_PowerBI_Portfolio_Pack/model/PowerBI_Model_Setup.txt` for
   relationships, then apply `Northstar_GMG_PowerBI_Portfolio_Pack/model/Northstar_GMG_All_Measures.tmdl`
   in TMDL view for the full measure library.
3. See `Northstar_GMG_PowerBI_Portfolio_Pack/docs/Northstar_GMG_Data_Dictionary.xlsx` for full
   table/column documentation and `docs/Dashboard_GMG_Changes.md` for the report's page-by-page
   design rationale.

## Report pages

- **Executive Fleet Performance** — Physical Availability, Use of Availability, Working
  Efficiency, and the full Time Usage cascade.
- **Time Usage** — state-mix breakdown by GMG leaf code, SBO vs SBE comparison, reconciliation
  check.
- **Production Performance** — tonnes per productive/working hour, schedule-to-actual variance
  bridge.
- **Reliability / Maintenance** — downtime breakdown, component/failure/work-order drill-downs.
- **Delays & Standby** — operating delay and standby splits, challengeable reason classification.
- **Equipment Detail** — per-unit status timeline.
- **Production Opportunity** — estimated recoverable tonnes from released standby/delay time
  (labelled as an estimate, not an actual).
