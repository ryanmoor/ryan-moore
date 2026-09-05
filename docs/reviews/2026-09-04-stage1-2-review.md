<!-- PR TARGET: https://github.com/ryanmoor/ryan-moore | Stage 1.2 -->
# Stage 1.2 review — spec, build, audit

**Spec:** [`capabilities/marginal-analysis/spec.md`](https://github.com/ryanmoor/ryan-moore/blob/main/capabilities/marginal-analysis/spec.md)

> Graded 2026-09-04, first pass on this stage. You went from nothing to a repository with a specification, a workbook and twelve of the fourteen canonical paths in a single day, and your Stage 0 moved a long way on the strength of it. What is in capabilities/marginal-analysis/ is a general method template rather than this engagement's model, and that is what this review is about.

| Criterion | Where it stands |
|---|---|
| Spec completeness — inputs, structure, calculation flow | As a capability template this is decent work: a stated purpose, a three-sheet model design, a named-range table with each name's cell and meaning, and formula logic written out. The craft is right. What it does not contain is this engagement. There are no beds, no crops, no prices, no bed caps, no 36-week season, and no labor function — the whole model is price minus variable cost per unit, break-even units, and a sensitivity table. A builder handed this could not produce the farm model, because the farm is not in it. |
| Spec validation rules | There is no validation section. Nothing states what the finished workbook must reproduce, no hand-calculated anchor, no acceptance figure, no tolerance, no error-cell rule. This is the largest single gap in the submission and the cheapest to close. |
| Workbook satisfies the contract | model.xlsx is real, formula-driven and uses named ranges throughout — Price, VariableCostPerUnit, FixedCosts, Volume, and derived contribution-margin and break-even cells. It is built from its specification, which is the right discipline. It models a product priced at $25 with $14 of variable cost, $8,000 of fixed costs and a volume of 1,500 units. None of those are case figures and the case's decision does not appear in the file. |
| Audit note | No audit section. Correct with no engagement build behind it. |

> The spec-side criteria are summarised above. Held rather than entered — the stage is not due until 6 September, and the work needed is a redirection rather than a rebuild.

### What you built is useful — it is just the other half of the structure

The repository standard has two levels and you built the outer one. capabilities/marginal-analysis/ is meant to hold the reusable method — what marginal analysis is, when you reach for it, how you generally set it up — and that is exactly what your spec.md and model.xlsx describe.

The other level is the engagement: this specific farm, its 64 beds and three crops, and the specification and workbook that answer its question. That is what Stage 1.2 grades, and it is the part that is missing.

So nothing you built is wasted. Your break-even template belongs in the capability, and what you need to add beside it is the model that answers the perfect-competition case.

### What the engagement specification has to contain

- Inputs with values. 36 weeks, 64 beds, $20,000 fixed costs, the farmer at $50,000 for 720 field hours, up to four temporary workers at $25,000 for 1,440 hours each. Then per crop: bed cap, price per bed, labor hours per bed-week, fertilizer per bed, and the diminishing-returns rate. Tomatoes 20 / $8,800 / 2.50 / $880 / 10%; carrots 20 / $2,094 / 0.833 / $440 / 2.5%; mesclun 30 / $2,700 / 1.25 / $880 / 1.25%.

- The labor function, written out: LABOR_HRS(q) = q x hours-per-bed-week x 36 x (1 + rate)^q. The exponent on q is the thing most often got wrong — it means the compounding applies to the whole crop, not just the newest bed.

- The costing order. The farmer's 720 hours are consumed first across all crops; temporary labor covers the remainder. Rates are derived — 50,000 over 1,440 and 25,000 over 1,440 — not the rounded $34.72 and $17.36.

- The sheet structure, the three decision cells, and the Solver setup.

- Validation rules with numbers in them: one tomato bed is 99 hours, ten are 2,334.37, the optimal mix is 10 / 20 / 30, and season profit is $42,762 within $5.

### Two days, and what I would do with them

- Today: the inputs table with real values and the labor function. Commit it before you touch a workbook — the commit order is part of what the stage grades.

- Also today, and it is ten seconds: rename docs/briefs/perfect-competition-brief to perfect-competition-brief.md, then write the page. Your Stage 1.1 review has the full brief for that, and it has to be committed before the model exists.

- Tomorrow: build the workbook from the committed specification, run Solver from two different starting points, and write the audit findings underneath.

That is a real amount of work for two days. If it is not achievable alongside everything else, tell me which parts you want to prioritise — you have shown this week that you can move fast when you sit down to it, and I would rather agree a plan than grade a gap.

### How this reached you

By email and Lamaku rather than as a pull request, because I do not have push access to your repository. You now have a specification and a workbook, which is precisely the point where line-level review starts being worth much more than a comment box — a review anchored to the cell it is about is a different thing entirely.

Your repository on github.com, then Settings, then Collaborators, then Add people, then adamwstauffer. Thirty seconds, and every future review lands in the repository next to the file it is about.

---

### How to work this review

Treat this PR the way an analyst treats feedback from a senior reviewer — a review is a proposal to engage with, not a checklist to rubber-stamp.

1. **Read it yourself first.** Form your own view before you change anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM.** Paste this review and your spec into your assistant and ask it to (a) explain anything you are unsure of, and (b) argue the *other side*.
3. **Then correct the spec, not the workbook.** When a check fails, you fix the specification and regenerate, so the document keeps describing what was actually built.
4. **Close the loop.** Reply in this thread with what you changed and what you pushed back on, then commit and push.

*Your score and the per-criterion breakdown are in your Lamaku comment, not here — this repository is public.*

— Adam
