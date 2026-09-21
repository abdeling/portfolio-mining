# Northstar Mining Operations Analytics — GMG-aligned Power BI portfolio pack

This version replaces the former flat NS/PM/BD/MD/SB/OD/PR state model with a hierarchical GMG-style Time Usage Model.

**Canonical identities:** `CT = ST + UT`, `ST = AT + DT`, `AT = OT + SB`, `SB = SBO + SBE`, `OT = WT + OD`, `WT = PT + NP`.

`FactState` carries one `SystemStatusCode` per time slice. `DimStatus` maps each source/system code to a GMG terminal leaf and exposes hierarchy/flags. This avoids double-counting aggregate states such as AT, OT and WT.

All data is synthetic/simulated.
