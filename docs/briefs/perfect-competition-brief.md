---
type: brief
engagement: perfect-competition
capability: marginal-analysis
date: 2026-09-04
status: committed
hypothesis: 20 beds carrots, 30 beds mesclun, 10 beds tomatoes
---

# Perfect Competition

## The Problem
You are preparing to plant your 1.5 acre farm for the upcoming growing season. You have three crops available (tomatoes, carrots, mesclun) and have space for up to 64 beds. Your goal this season is to maximize profit by planting the optimal number of beds of each crop. The optimal mix is not immediately obvious; you need a model to help answer the question. Based on previous experience, adding an additional bed increases the amount of work necessary to tend the other beds that are already planted. In economic terms, the problem is rising marginal cost (of labor) and diminishing marginal returns (also labor), or “diseconomies of scale.” The purpose of this engagement is to determine the optimal bed mix by evaluating the inputs (fixed and variable costs), constraints and impact of each additional bed on the total cost of production, ending up with a recommendation of which crops to plant and how many beds of each.

## Assumptions
We will assume the yield and revenue per bed are constant for each crop regardless of how many beds we plant. There are fixed costs of $20,000 for the season regardless of whether you plant just one bed or all 64. The growing season is fixed at 36 weeks. The table below shows our assumptions for each crop. We will assume these hold regardless of which beds hold which crop or how many beds are planted. Beds must be fully planted with a single crop for the entire season; no partial or split beds.

| Crop | Max beds | Revenue/bed | Labor hrs/wk/bed | Fertilizer cost/bed | Diminishing returns/bed |
| --- | --- | --- | --- | --- | --- |
| Tomatoes | 20 | $8,800 | 2.50 | $880 | 10.00% |
| Carrots | 20 | $2,094 | 2.5/3 | $440 | 2.50% |
| Mesclun | 30 | $2,700 | 1.25 | $880 | 1.25% |

You can work a maximum of 720 total hours ($34.72/hr) and if more labor is needed, you can hire up to four temporary workers for $17.36/hr at a max of 1,440 hours each. We will exhaust your 720 labor hours first before hiring additional help. Lastly, the equation for calculating total labor hours for q beds of each crop is:

Labor(q) = q(hrs/wk/bed)36(1+dim)^q

## Hypothesis
Because of the particularly low labor cost of carrots, and the low diminishing returns of mesclun, I hypothesize we will max both crops, meaning that marginal cost will not overtake marginal revenue before we plant 20 and 30 beds, respectively. That leaves 14 beds available for tomatoes. Revenue for tomatoes is 4.2x carrots and 3.2x mesclun, but labor is 3x carrots and 2x mesclun and diminishing returns is 4x carrots and 8x mesclun. In other words, costs are higher and also rise faster. The key to maximizing profit for the entire farm will be determined by how many beds of tomatoes we can plant before marginal cost overtakes marginal revenue. 

| # of beds (q) | q(hrs/wk/bed)36(1+dim)^q | Marginal labor cost | Total cost (labor + fertilizer) |
| --- | --- | --- | --- |
| q = 9 | $1,909 | $6,363 | $7,243 |
| q = 10 | $2,334 | $7,368 | $8,248 |
| q = 11 | $2,824 | $8,510 | $9,390 |

The table above shows that total bed cost crosses revenue ($8,800) from bed 10 to bed 11; therefore, I predict that 10 beds will be optimal. My full hypothesis is that we will maximize profit with 20 beds of carrots, 30 beds of mesclun and 10 beds of tomatoes.

## Falsifiers
If the optimal number of tomato beds is less than 10, then the compounding effect on labor pushes marginal cost higher faster than I expected. If it’s more than 10, then it rises slower than expected, perhaps due to the lower wage of temp labor.

If optimal beds of carrots or mesclun are anything other than the max, then my assumptions about the impact of low labor cost and diminishing returns (relative to tomatoes) failed to account for the total picture.

