---
type: spec
capability: marginal-analysis
engagement: perfect-competition
date: 2026-09-21
status: audited            # draft | built | audited
built_with: "Claude Code, from this file"
---

# Marginal Analysis - model specification

## Purpose

This model shows marginal cost for each additional bed of each crop, measured
against the known revenue per bed of each crop. This allows us to answer the
question, "Will planting this additional bed of this crop increase or
decrease our profit?"

## Inputs

| Name | Value | Unit | Source |
| --- | ---| --- | --- |
| `RevenuePerBedTomatoes` | 8800 | USD per bed | Case scenario, crop table |
| `LaborHrsPerBedTomatoes` | 2.5 | hrs per week per bed | Case scenario, crop table |
| `RevenuePerBedCarrots` | 2094 | USD per bed | Case scenario, crop table |
| `LaborHrsPerBedCarrots` | LaborHrsPerBedTomatoes/3 | hrs per week per bed | Derived: 2.5/3 |
| `RevenuePerBedMesclun` | 2700 | USD per bed | Case scenario, crop table |
| `LaborHrsPerBedMesclun` | 1.25 | hrs per week per bed | Case scenario, crop table |
| `MaxBedsTomatoes` | 20 | max beds tomatoes | Case scenario, crop table |
| `MaxBedsCarrots` | 20 | max beds carrots | Case scenario, crop table |
| `MaxBedsMesclun` | 30 | max beds mesclun | Case scenario, crop table |
| `FertilizerPerBedTomatoes` | 880 | USD per bed | Case scenario, crop table |
| `FertilizerPerBedCarrots` | 440 | USD per bed | Case scenario, crop table |
| `FertilizerPerBedMesclun` | 880 | USD per bed | Case scenario, crop table |
| `DimReturnsTomatoes` | 0.10 | decimal fraction per additional bed (0.10 = 10%) | Case scenario, crop table |
| `DimReturnsCarrots` | 0.025 | decimal fraction per additional bed (0.025 = 2.5%) | Case scenario, crop table |
| `DimReturnsMesclun` | 0.0125 | decimal fraction per additional bed (0.0125 = 1.25%) | Case scenario, crop table |
| `SeasonWeeks` | 36 | weeks | Case scenario |
| `FixedCosts` | 20000 | USD per season | Case scenario |
| `TotalBedCap` | 64 | max beds all crops | Case scenario |
| `OwnLaborHours` | 720 | farmer's max field hrs | Case scenario |
| `Salary` | 25000 | USD per season | Case scenario |
| `OwnWageRate` | Salary/OwnLaborHours | USD per hr | Derived: 25000/720 |
| `TempWorkerCount` | 4 | temp workers | Case scenario |
| `TempHoursPerWorker` | 1440 | max field hrs per temp worker | Case scenario |
| `TempWageRate` | Salary/TempHoursPerWorker | USD per hr | Derived: 25000/1440 |

## Structure

- **Inputs**: the crop table (one row per crop: max beds, revenue/bed, labor
  hrs/wk/bed, fertilizer cost/bed, diminishing-returns rate/bed) and the
  farm-wide scalars listed above.
- **Calc**: `BedsTomatoes`/`BedsCarrots`/`BedsMesclun` (Solver's "By Changing
  Variable Cells"), labor hours per crop, total labor hours, tiered labor
  cost, revenue, fertilizer cost, total cost, `Profit` (the objective), and
  the constraint-check cells (`TotalBedsPlanted`, `TotalLaborCapacity`) that
  Solver's constraints reference directly.
- **Output**: the exact Solver dialog configuration (objective cell,
  changing cells, every constraint, integer requirement, recommended solving
  method) plus a live pass-through of the current `Calc` values.
- **MC Schedule**: one block per crop (Tomatoes rows 7–29, Carrots 32–58,
  Mesclun 61–97), one row per bed count from `q = 0` to that crop's
  `MaxBeds`, plus four rows past the cap for carrots (beds 21–24) and
  mesclun (beds 31–34) in grey italics, with the crop planted alone
  (standalone curve). Columns: Beds
  (q), Labor hours, Labor cost, Fertilizer cost, Variable cost, MC
  standalone, AVC, Price, Price − MC, Standalone profit, MC in solved mix.
  Three line charts to the right (from column M; MC standalone, AVC, Price
  and a dashed MC in solved mix against q, one per crop) read directly from
  these cells. Columns B–J are independent of the `Calc` decision cells —
  changing `Inputs` moves them, changing the bed mix does not. Column K
  (MC in solved mix) reads `TotalLaborHours` and the crop's own labor hours
  from `Calc`, so it follows whatever mix is on `Calc`.

## Calculation logic

For each crop, in named-range notation:

    LaborHoursTomatoes = BedsTomatoes * LaborHrsPerBedTomatoes * SeasonWeeks * (1 + DimReturnsTomatoes)^BedsTomatoes
    LaborHoursCarrots  = BedsCarrots  * LaborHrsPerBedCarrots  * SeasonWeeks * (1 + DimReturnsCarrots)^BedsCarrots
    LaborHoursMesclun  = BedsMesclun  * LaborHrsPerBedMesclun  * SeasonWeeks * (1 + DimReturnsMesclun)^BedsMesclun

    TotalLaborHours    = LaborHoursTomatoes + LaborHoursCarrots + LaborHoursMesclun
    OwnLaborHoursUsed   = MIN(TotalLaborHours, OwnLaborHours)
    TempLaborHoursUsed  = MAX(TotalLaborHours - OwnLaborHours, 0)
    TempLaborCapacity   = TempWorkerCount * TempHoursPerWorker
    LaborCost           = OwnLaborHoursUsed * OwnWageRate + TempLaborHoursUsed * TempWageRate

    Revenue        = BedsTomatoes*RevenuePerBedTomatoes + BedsCarrots*RevenuePerBedCarrots + BedsMesclun*RevenuePerBedMesclun
    FertilizerCost = BedsTomatoes*FertilizerPerBedTomatoes + BedsCarrots*FertilizerPerBedCarrots + BedsMesclun*FertilizerPerBedMesclun
    TotalCost      = FixedCosts + FertilizerCost + LaborCost
    Profit         = Revenue - TotalCost

    TotalBedsPlanted    = BedsTomatoes + BedsCarrots + BedsMesclun
    TotalLaborCapacity  = OwnLaborHours + TempLaborCapacity

`MC Schedule`, for each crop and each bed count `q` (one row per `q`):

    LaborHours(q)    = q * LaborHrsPerBed<Crop> * SeasonWeeks * (1 + DimReturns<Crop>)^q
    LaborCost(q)     = MIN(LaborHours(q), OwnLaborHours) * OwnWageRate + MAX(LaborHours(q) - OwnLaborHours, 0) * TempWageRate
    VariableCost(q)  = LaborCost(q) + q * FertilizerPerBed<Crop>
    MC(q)            = VariableCost(q) - VariableCost(q - 1)        (blank at q = 0)
    AVC(q)           = VariableCost(q) / q                          (blank at q = 0)
    Price            = RevenuePerBed<Crop>                          (blank at q = 0: no beds, no price)
    Price - MC(q)                                                   (blank at q = 0)
    StandaloneProfit(q) = q * RevenuePerBed<Crop> - VariableCost(q) - FixedCosts

    OtherHours       = TotalLaborHours - LaborHours<Crop>           (the other two crops, at their Calc bed counts)
    FarmLaborCost(h) = MIN(h, OwnLaborHours) * OwnWageRate + MAX(h - OwnLaborHours, 0) * TempWageRate
    MCInSolvedMix(q) = FarmLaborCost(OtherHours + LaborHours(q)) - FarmLaborCost(OtherHours + LaborHours(q - 1))
                       + FertilizerPerBed<Crop>                     (blank at q = 0)

## Conventions

- Own labor hours are exhausted before any temp hours are counted, and the
  720-hour threshold applies to *total* labor across all three crops
  combined, not per crop.
- Beds are whole numbers and single-crop for the entire season — no partial
  or split beds. Enforced as an integer constraint on the decision variables,
  not just a stated assumption.
- Fixed costs are incurred in full regardless of the mix, including the
  all-zero mix — never prorated or waived below some bed count.
- Revenue and fertilizer cost per bed are constant per bed regardless of mix
  or bed count; only labor hours scale nonlinearly, via `Labor(q)`.
- Hours beyond `TotalLaborCapacity` (`OwnLaborHours + TempLaborCapacity` =
  6,480 hrs) are not modeled as a cost — they are infeasible. A hard
  constraint keeps Solver from crossing that boundary rather than pricing it.
- No rounding is applied internally; beds are integers by constraint, and
  dollar figures carry full precision and are only rounded for display
  (`$#,##0` formats).
- Diminishing-returns rates are stored as decimal fractions (`0.10`, not
  `10`) because the labor formula uses them directly in `(1 + rate)^q`. On
  `Inputs` they display as decimals to four places, so the cell shows
  `0.1000` and holds `0.1`. Entering `10` would compute `(1 + 10)^q`, which is
  `11^q` rather than `1.1^q`.
- At `q = 0` for any crop, `Labor(q) = 0` — the formula zeroes out on its own,
  no special-case needed.
- `MC Schedule` columns B–J are a standalone curve: each crop is assumed to
  use all of `OwnLaborHours` before any temp hours, as if it were the only
  crop. That is what produces the MC dip where a crop's own hours run out
  (tomatoes between bed 5 and bed 6, carrots 16→17, mesclun 13→15). Column K
  prices the same bed inside the `Calc` mix, where labor is pooled across
  crops. At 10/20/30 the other two crops alone are past `OwnLaborHours`, so
  every marginal hour in column K is paid at `TempWageRate` and there is no
  dip. For a crop's rows past its own 720-hour point the two MC columns are
  equal; below that point they are not.
- `MC Schedule` has a fixed number of rows per crop, sized to the current
  `MaxBeds` (20 / 20 / 30) plus four rows past the carrot and mesclun caps.
  Raising a cap on `Inputs` does not add rows. The rows past a cap are
  hypothetical — beds the model does not allow — and show what one more bed
  would earn if that cap were relaxed (its shadow price). The charts stop
  at each cap.

## Validation rules

- Every calculated cell on `Calc` and `Output` contains a formula referencing
  named ranges, not a literal number or a raw cell address — except the three
  decision-variable cells (`BedsTomatoes`, `BedsCarrots`, `BedsMesclun`),
  which are Solver's inputs.
- Every calculated cell on `MC Schedule` references named ranges for inputs.
  The only raw cell addresses are same-block references to the row's own
  `q` and to the previous row's variable cost (for MC) — a schedule cannot
  be built without them. The `q` column holds literal integers by design.
- Acceptance (`MC Schedule`): tomato MC = $8,248.59 at q = 10 and
  $9,390.72 at q = 11; tomato standalone profit at q = 20 = −$84,334.37
  (equal to the 20/0/0 Solver start in Audit findings); carrot Price − MC
  at q = 20 = $405.05; mesclun Price − MC at q = 30 = $279.90. The carrot
  and mesclun figures equal the profit lost by dropping one bed from
  10/20/30 (to 10/19/30 and 10/20/29).
- Acceptance (`MC Schedule`, rows past the cap): carrot Price − MC at
  q = 21 = $352.49 (MC $1,741.51) and mesclun Price − MC at q = 31 =
  $246.47 (MC $2,453.53) — the value of relaxing each cap by one bed.
- Acceptance (`MC Schedule`, column K, at 10/20/30 on `Calc`): up to each
  cap, carrot MC in solved mix never exceeds $1,688.95 (q = 20) and mesclun
  never exceeds $2,420.10 (q = 30), both below price; tomato MC in solved mix equals the
  standalone MC from q = 6 on ($8,248.59 at q = 10).
- No error cells (`#REF!`, `#DIV/0!`, `#VALUE!`) at the starting values (all
  beds at 0) or at any feasible integer point inside the constraints.
  Hand check: `BedsTomatoes`=1, 1 * 2.5 * 36 * 1.1 = `LaborHoursTomatoes` = 99 hours
- Hand check: at 0/0/0 beds, `Profit = -FixedCosts = -$20,000`.
- Acceptance: at 10/20/30 beds (Tomatoes/Carrots/Mesclun), `Profit` =
  $42,761.66 ± $0.01 and `TotalLaborHours` = 5,277.22 ± 0.01 hrs (inside
  `TotalLaborCapacity`). The same values come from Solver's saved solution in
  the workbook and from an independent script outside it.
- `TotalBedsPlanted` must never exceed `TotalBedCap` (64) at any point Solver
  evaluates; each crop's beds must never exceed its own `MaxBeds`.
- `TotalLaborHours` must never exceed `TotalLaborCapacity` (6,480 hrs) at any
  point Solver evaluates.
- The three decision-variable cells must be constrained to integer and ≥ 0
  in the Solver model itself, not left to convention.

## Outputs

- `Profit` — the objective Solver maximizes.
- `BedsTomatoes`, `BedsCarrots`, `BedsMesclun` — the decision.
- `TotalBedsPlanted`, `TotalLaborHours` — feasibility checks against the bed
  cap and labor capacity.
- `Revenue`, `FertilizerCost`, `LaborCost`, `TotalCost` — the profit
  breakdown.
- `Output` restates all of the above and states the Solver dialog
  configuration needed to solve for them.
- `MC Schedule` — standalone MC, AVC, Price − MC and standalone profit per
  bed for each crop, MC in the solved mix, plus the three MC-vs-price charts.

## Audit findings

  | Start T/C/M | Profit from starting combination | End T/C/M from Solver | What It Means |
  | --- | --- | --- | --- |
  | 10/20/30 | $42,761.66 | 10/20/30 | Confirms our target maximized profit |
  | 0/0/0 | -$20,000 | 10/20/30 | Negative profit from planting zero beds equals fixed costs; Solver confirmed optimal T/C/M |
  | 20/0/0 | -84,334.37 | 10/20/30 | High negative profit from max tomatoes only; demonstrates runaway compounding marginal cost that has far surpassed revenue; also this combination requires 12,109.5 hours of labor, which surpasses the limit of 6,480 allowed by the model. |

- **Checked**: named-range coverage across every formula cell on `Calc`.
  **Found**: consistent with the Inputs table above — no formula references
  a raw cell address, and no named range is unused.
  **Did**: no change needed.

  Finding #1: Solver's result on the saved model
- **Checked**: the Solver model saved on `Calc` (maximize `Profit`, seven constraints, whole-number beds ≥ 0) and the solution saved in the workbook.
  **Found**: 10 / 20 / 30 beds, `Profit` $42,761.66, `TotalLaborHours` 5,277.22 of 6,480 hours available, 60 of 64 beds. That meets the acceptance rule.
  **Did**: Confirmed hypothesis of 10/20/30 optimal bed mix.

  Finding #2: The second two additional starting points
- **Checked**: Solver started from 0/0/0 and 20/0/0
  **Found**: See table above for `Profit` and Solver result for each combination
  **Did**: Confirmed 10/20/30 optimal bed mix.

  Finding #3
- **Checked**: the full search, which scored every whole-bed mix with an independent script.
  **Found**: 10/20/30 is the single best of 9,726 allowed mixes. Carrots (20) and mesclun (30) are at their maximum bed counts. Tomatoes stop at 10: a 9th→10th
  tomato bed adds profit, but the 11th loses $590.72 compared with 10/20/30. Four beds and about 1,203 labor hours go unused.
  **Did**: The last four beds stay empty because if planted, compounding marginal labor would surpass marginal revenue, thereby decreasing overall profit. This confirms our hypothesis of 10 tomato beds with Carrots and Mesclun at their max. 

  Finding #4: `MC Schedule` sheet
- **Checked**: every formula on the new `MC Schedule` sheet, recalculated
  outside Excel with the Python `formulas` engine (LibreOffice headless still
  fails to load the workbook), against the independent script.
  **Found**: no error cells; tomato MC $8,248.59 (q = 10) and $9,390.72
  (q = 11); tomato MC drops from $7,660.86 (q = 5) to $4,906.28 (q = 6);
  carrot Price − MC $405.05 at q = 20; mesclun Price − MC $279.90 at q = 30;
  tomato standalone profit at q = 20 −$84,334.37, matching the 20/0/0 row
  above. All match the script.
  **Did**: added the sheet and three charts. In the first build the chart
  and axis titles overlapped the axis numbers; rebuilt the charts with titles
  set outside the plot area and the bed-count axis at the bottom. The fixed
  charts were checked visually in Excel.

  Finding #5: MC in the solved mix
- **Checked**: the standalone carrot and mesclun charts against the
  recommendation that both crops stop at their caps with MC below price.
  **Found**: the standalone curves show carrot MC above price at beds 11–16
  and mesclun MC above price at beds 7–13, because each crop pays the
  farmer's wage until its own 720 hours run out. In the 10/20/30 mix those
  hours are already used, so every marginal hour is paid at the temp wage.
  **Did**: added column K (MC in solved mix) and a dashed fourth line on
  each chart; renamed column F to "MC standalone"; dropped "(standalone)"
  from the chart titles; set every bed number to show on the x-axis.
  Recalculated outside Excel with the Python `formulas` engine: no error
  cells; carrot column K peaks at $1,688.95 and mesclun at $2,420.10.

  Finding #6: value of relaxing the carrot and mesclun caps
- **Checked**: whether the schedule can show what one more carrot or
  mesclun bed would be worth.
  **Found**: it could not — rows stopped at each cap, and Price − MC at the
  cap ($405.05, $279.90) is what the last allowed bed earns, not what the
  next one would. An earlier Claude session presented those two figures as
  the value of one more bed; that was wrong.
  **Did**: rebuilt the sheet with four grey-italic rows past each cap
  (carrots 21–24, mesclun 31–34; mesclun block moved down four rows, now
  61–97); blanked Price at q = 0 for each crop. Recalculated with the Python
  `formulas` engine: no error cells; bed 21 carrots earns $352.49 over MC,
  bed 31 mesclun $246.47.
