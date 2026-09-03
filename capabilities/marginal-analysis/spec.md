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

<!-- PLACEHOLDER: notes on how this method flexes per engagement — e.g. multi-product
     mix, step-fixed costs, capacity constraints — added as real cases come up. -->
