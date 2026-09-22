---
type: spec
capability: marginal-analysis
engagement: perfect-competition
date: 2026-09-20
status: draft            # draft | built | audited
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
| `LaborHrsPerBedCarrots` | 2.5/3 | hrs per week per bed | Case scenario, crop table |
| `RevenuePerBedMesclun` | 2700 | USD per bed | Case scenario, crop table |
| `LaborHrsPerBedMesclun` | 1.25 | hrs per week per bed | Case scenario, crop table |
| `MaxBedsTomatoes` | 20 | max beds tomatoes | Case scenario, crop table |
| `MaxBedsCarrots` | 20 | max beds carrots | Case scenario, crop table |
| `MaxBedsMesclun` | 30 | max beds mesclun | Case scenario, crop table |
| `FertilizerPerBedTomatoes` | 880 | USD per bed | Case scenario, crop table |
| `FertilizerPerBedCarrots` | 440 | USD per bed | Case scenario, crop table |
| `FertilizerPerBedMesclun` | 880 | USD per bed | Case scenario, crop table |
| `DimReturnsTomatoes` | 10 | percent per additional bed | Case scenario, crop table |
| `DimReturnsCarrots` | 2.5 | percent per additional bed | Case scenario, crop table |
| `DimReturnsMesclun` | 1.25 | percent per additional bed | Case scenario, crop table |
| `SeasonWeeks` | 36 | weeks | Case scenario |
| `FixedCosts` | 20000 | USD per season | Case scenario |
| `TotalBedCap` | 64 | max beds all crops | Case scenario |
| `OwnLaborHours` | 720 | farmer's max field hrs | Case scenario |
| `OwnWageRate` | 34.72 | USD per hr | Case scenario |
| `TempWorkerCount` | 4 | temp workers | Case scenario |
| `TempHoursPerWorker` | 1440 | max field hrs per temp worker | Case scenario |
| `TempWageRate` | 17.36 | USD per hr | Case scenario |

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
- At `q = 0` for any crop, `Labor(q) = 0` — the formula zeroes out on its own,
  no special-case needed.

## Validation rules

- Every calculated cell on `Calc` and `Output` contains a formula referencing
  named ranges, not a literal number or a raw cell address — except the three
  decision-variable cells (`BedsTomatoes`, `BedsCarrots`, `BedsMesclun`),
  which are Solver's inputs.
- No error cells (`#REF!`, `#DIV/0!`, `#VALUE!`) at the starting values (all
  beds at 0) or at any feasible integer point inside the constraints.
  Hand check: `BedsTomatoes`=1, 1 * 2.5 * 36 * 1.1 = `LaborHoursTomatoes` = 99 hours
- Hand check: at 0/0/0 beds, `Profit = -FixedCosts = -$20,000`.
- Hand check: at 10/20/30 beds (Tomatoes/Carrots/Mesclun), `TotalLaborHours`
  ≈ 5,276.8 hrs (inside `TotalLaborCapacity`) and `Profit` ≈ $42,775 —
  confirmed against an independent script outside the workbook, not against
  a Solver run.
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

## Audit findings

- **Checked**: an independent Python replica of the `Calc` formulas
  (`Labor(q)`, the tiered wage split, `Profit`) evaluated at four sample bed
  combinations (0/0/0, 10/20/30, 20/20/30, 14/20/30).
  **Found**: internally consistent — the all-zero case matches `-FixedCosts`
  exactly; feasible mixes produce positive profit; 20/20/30 (70 beds, over
  the 64-bed cap) produces a large negative profit, driven by the
  diminishing-returns term compounding on the extra tomato beds, as expected.
  **Did**: no change needed; used as the formula-correctness check ahead of
  running Solver, since the workbook is not yet solved.
- **Checked**: whether the workbook recalculates cleanly outside Excel
  (LibreOffice headless), as an availability check before Solver setup.
  **Found**: LibreOffice headless failed to load and convert both this
  workbook and the single-product template it replaced, so the failure is
  environment-specific, not caused by this build.
  **Did**: flagged to the user; recommend opening the `.xlsx` directly in
  Excel with the Solver add-in enabled to confirm before relying on it, since
  that has not been independently verified here.
- **Checked**: named-range coverage across every formula cell on `Calc`.
  **Found**: consistent with the Inputs table above — no formula references
  a raw cell address, and no named range is unused.
  **Did**: no change needed.
