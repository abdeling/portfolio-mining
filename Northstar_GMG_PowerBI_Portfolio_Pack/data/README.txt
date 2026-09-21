NORTHSTAR MINING OPERATIONS ANALYTICS — GMG-ALIGNED SYNTHETIC DATA

ALL DATA IS SYNTHETIC / SIMULATED. NO CLIENT DATA.

Time Usage Model
----------------
The FactState table stores exactly ONE terminal system status for each unit × 30-minute slice.
Calendar Time (CT), Scheduled Time (ST), Available Time (AT), Operating Time (OT) and Working Time (WT) are NOT stored as events. They are derived from DimStatus flags / GMG hierarchy.

GMG hierarchy implemented
CT = ST + UT
ST = AT + DT
AT = OT + SB
SB = SBO + SBE
OT = WT + OD
WT = PT + NP

Terminal GMG leaf codes used in FactState via DimStatus:
UT, DT, SBO, SBE, OD, NP, PT

Power BI table names
--------------------
DimDate, DimEquipment, DimShift, DimArea, DimComponent, DimReason, DimStatus, DimActivity
FactState, FactProduction, FactWorkOrder, FactFailure, FactPM, FactCondition, FactRoster, FactPlan

Key relationship
----------------
DimStatus[SystemStatusCode] 1-* FactState[SystemStatusCode]
DimReason[ReasonID] 1-* FactState[ReasonID]
All other relationships are documented in docs/Northstar_GMG_Data_Dictionary.xlsx.

Validation
----------
Calendar hours: 315,360.0
PA: 90.14%
UoA: 68.24%
Working efficiency WT/OT: 80.76%
Productive share PT/WT: 91.16%
Productive efficiency PT/OT: 73.62%
All six GMG reconciliation identities resolve to 0.0 h.
