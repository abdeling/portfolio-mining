# Dashboard changes for the GMG-aligned model

Use the same visual design, but update the Time Usage semantics as follows.

## Executive Fleet Performance
- Keep Physical Availability = AT / ST.
- Keep Use of Availability = OT / AT.
- Replace the old 80.7% **Productive Efficiency** KPI with **Working Efficiency = WT / OT** (about 80.8% on the synthetic fleet).
- If desired, add **Productive Share of Working = PT / WT** and **Productive Efficiency = PT / OT** as secondary diagnostics.
- The Time Usage cascade must reconcile the hierarchy: CT = ST + UT; ST = AT + DT; AT = OT + SB; SB = SBO + SBE; OT = WT + OD; WT = PT + NP.

## Time Usage
- State-mix visuals should use `DimStatus[GMGLeafCode]`: PT, NP, OD, SBO, SBE, DT, UT.
- Add explicit SBO vs SBE comparison by crew / area.
- Keep a 100% reconciliation card using `[TUM Reconciliation %]`.

## Production Performance
- Keep tonnes / Productive h as a diagnostic.
- Add tonnes / Working h as the rate used by the variance bridge.
- The variance bridge uses Schedule → Availability → Utilisation → Working Efficiency → Rate → Actual.

## Reliability / Maintenance
- Downtime is GMG leaf DT.
- Planned Maintenance, Breakdown and Maintenance Delay are `DimStatus[StatusFamily]` details inside DT.
- Component / failure / work-order drill-downs remain unchanged.

## Delays & Standby
- Split Standby into Operating Standby (SBO) and External Standby (SBE).
- Operating Delay remains OD.
- Non-Productive Time (NP) is working time and should not be merged into OD.
- `DimReason[ControlClass]` is an operational classification and should remain visibly challengeable in the portfolio.

## Equipment Detail
- Timeline uses `FactState[SystemStatusCode]` for detailed labels and `DimStatus[GMGLeafCode]` for color/rollup.
- Never store CT/ST/AT/OT/WT as separate fact rows; they are aggregate measures.

## Production Opportunity
- A released standby/delay hour is not automatically a productive hour.
- The supplied opportunity measure first applies Working Efficiency (WT/OT), then tonnes / Working h.
- Keep the page labelled `ESTIMATE — NOT AN ACTUAL`.
