---
type: spec
capability: marginal-analysis
engagement: perfect-competition
date: 2026-09-17
status: draft          # draft | built | audited
built_with: "Claude Code, from this file"
---

# Marginal Analysis — Method Spec

## Purpose

Answer "should we do this one more thing?" (make one more unit, take one more
account, drop one more SKU) by isolating the revenue and cost that actually change
— not fully-loaded averages.

## Model design (`model.xlsx`)

- **Inputs** sheet: the assumptions that vary by engagement (price, variable cost
  per unit, fixed costs in scope, volume/units). Each assumption is a single cell
  with a named range so formulas read as English, not cell coordinates.
- **Calc** sheet: contribution margin per unit, contribution margin %, break-even
  units/revenue, and a sensitivity table (e.g. margin at ± price/volume steps).
- **Output** sheet (or top of Calc): the answer stated in one line, plus the
  sensitivity table an audience can scan.

## Named ranges (as shipped in the template)

| Name | Cell | Meaning |
| --- | --- | --- |
| `Price` | Inputs!B2 | Selling price per unit |
| `VariableCostPerUnit` | Inputs!B3 | Variable cost per unit |
| `FixedCosts` | Inputs!B4 | Fixed costs in scope for this decision |
| `Volume` | Inputs!B5 | Units under consideration |

## Formula logic

- `ContributionMarginPerUnit` = `Price - VariableCostPerUnit`
- `ContributionMarginPct` = `ContributionMarginPerUnit / Price`
- `BreakEvenUnits` = `FixedCosts / ContributionMarginPerUnit`
- `BreakEvenRevenue` = `BreakEvenUnits * Price`
- `TotalContributionAtVolume` = `ContributionMarginPerUnit * Volume`
- `NetOfFixed` = `TotalContributionAtVolume - FixedCosts`

## Per-engagement adaptation

**Perfect Competition** (multi-product mix, capacity constraints, Solver): `model.xlsx`
was rebuilt for this engagement to choose beds of three crops (tomatoes, carrots,
mesclun) rather than a single product's volume.

- **Inputs** sheet: one row per crop (max beds, revenue/bed, labor hrs/wk/bed,
  fertilizer cost/bed, diminishing-returns rate/bed), plus farm-wide scalars
  (total bed cap, fixed costs, season length, own labor hours/rate, temp labor
  worker count/hours/rate). Each cell keeps its own named range.
- **Calc** sheet: beds per crop are the decision variables (Solver's "By
  Changing Variable Cells"). Labor hours per crop follow
  `Labor(q) = q * hrs/wk/bed * SeasonWeeks * (1 + diminishing-returns/bed)^q`;
  labor cost is tiered (own hours exhausted first at the own rate, remainder
  at the temp rate); profit (`Revenue - FixedCosts - FertilizerCost - LaborCost`)
  is the objective. Constraint-check cells (total beds vs. cap, total labor
  hours vs. capacity) sit below the objective for Solver to reference directly.
- **Output** sheet: the exact Solver dialog configuration (objective cell,
  changing cells, each constraint, integer requirement, recommended solving
  method) plus live pass-through of the current Calc values — left as
  starting values, not solved, so running Solver is the next step rather than
  something this file has already done.
- Because `Labor(q)` is exponential in `q` and the wage tiers are piecewise
  (`MIN`/`MAX`), the objective is nonlinear and non-smooth; GRG Nonlinear can
  land on a local optimum depending on the starting beds, so the Output sheet
  recommends trying Evolutionary as a cross-check.

## Spec template

```
# <Capability> — model specification

## Purpose
What decision this model supports, in two sentences. What it must be able to answer.

## Inputs — the named contract
| Name | Value | Unit | Source |
|---|---|---|---|
| `TOM_PRICE` | 8800 | USD per bed | Case scenario, crop table |
| `TOM_HRS`   | 2.5  | hours per week per bed | Case scenario, crop table |

Every input gets a name, a value, a unit, and a source. You choose the names.
The requirement is that they exist and are used consistently below.

## Structure
Each sheet or region, and what it is for.

## Calculation logic
In named-range notation, never cell addresses:

  LABOR_HRS(q) = q x HRS_PER_BED x WEEKS x (1 + DIM_PCT)^q

"Column D times column E" is not a specification — it describes a spreadsheet
that does not exist yet.

## Conventions
The rules that are not visible in the formulas: costing order, allocation basis,
rounding, what happens at the boundaries. State all of them. A convention you
leave out is a convention the builder invents.

## Validation rules
The conditions the finished artifact must satisfy — check figures as acceptance
criteria, hand calculations, and structural rules ("every calculated cell
contains a formula", "no error cells").

## Outputs
Each result the model reports, by name.

## Audit findings
Added AFTER the build. For each check: what you checked, what you found, what
you did about it.
```
