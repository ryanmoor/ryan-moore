# Marginal Analysis

What this capability is: a reusable contribution-margin / break-even model for
evaluating a single incremental decision (a price change, a volume shift, a new
cost) — built once in `model.xlsx` per the method documented in `spec.md`, then
adapted per engagement.

## Engagements that exercised it

- [Perfect Competition](../../docs/briefs/perfect-competition-brief.md) — bed mix
  across three crops (tomatoes, carrots, mesclun) under a 64-bed cap, tiered
  labor cost, and diminishing returns per bed; `model.xlsx` rebuilt for a
  Solver-based multi-product optimization (see `spec.md`'s per-engagement
  adaptation section). Run Solver in Excel against the current file for the
  answer — it is not yet solved in the committed workbook.
