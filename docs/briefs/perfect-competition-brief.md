---
type: brief
engagement: perfect-competition
capability: marginal-analysis
date: 2026-09-04
status: in process
hypothesis: TBD
---

# Perfect Competition

## The Problem
I am preparing to plant my 1.5 acre farm for the upcoming growing season. I have three crops available (tomatoes, carrots, mesclun) and have space for up to 64 beds. My goal this season is to maximize profit by planting the optimal number of beds of each crop. The optimal mix is not immediately obvious, so I need to use a model to help answer the question. Based on previous experience, adding an additional bed increases the amount of work necessary to tend the other beds that are already planted. In economic terms, the problem is rising marginal cost and diminishing returns, or “diseconomies of scale.” The purpose of this engagement is to determine the optimal bed mix by evaluating the inputs (fixed and variable costs), constraints and impact of each additional bed on the total cost of production, ending up with a recommendation of which crops to plant and how many beds of each.

## Assumptions
There are fixed costs of $20,000 for the season regardless whether I plant just one bed or all 64. I cannot change the market price for any of the three crops, so revenue per bed is known and fixed (see table below). The growing season is fixed at 36 weeks. If I were to plant the max number of beds of each crop, the total would be 70 which exceeds the max of 64 for which I have space. Here are the assumptions the model will be based on for each crop:

| Crop | Max beds | Revenue/bed | Labor hrs/wk/bed | Fertilizer cost/bed | Diminishing returns/bed |
| --- | --- | --- | --- | --- | --- |
| Tomatoes | 20 | $8,800 | 2.50 | 880 | 10.00% |
| Carrots | 20 | $2,094 | 0.833 | 440 | 2.50% |
| Mesclun | 30 | $2,700 | 1.25 | 880 | 1.25% |

I can only work a maximum of 720 total hours ($34.72/hr) so if more labor is needed, I can hire up to four temporary workers for $17.36/hr and a max of 1,440 hours each. Lastly, the equation for calculating labor cost for q beds of each crop is:  Labor(q) = q*(hrs/wk/bed)*36*(1+dim)^q.

## Hypothesis
The diminishing returns from planting tomatoes is 4x greater than carrots and 8x greater than mesclun. Revenue for tomatoes is 4.2x carrots and 3.2x mesclun. Labor for tomatoes is 3x carrots and 2x mesclun. Therefore, my hypothesis is that profit will be maximized by planting the max beds of carrots and mesclun (20 and 30, respectively), and planting tomatoes in the remainder of available beds (14).

## Falsifiers
My hypothesis can be proven wrong if any combination of beds other than 20 carrots, 30 mesclun and 14 tomatoes proves optimal for maximizing profit.
