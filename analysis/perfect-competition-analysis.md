---
type: analysis
engagement: perfect-competition
capability: marginal-analysis
date: 2026-10-04
brief: docs/briefs/perfect-competition-brief.md
model: capabilities/marginal-analysis/model.xlsx
memo: docs/decisions/perfect-competition-memo.md
---

# Perfect Competition — analysis

## 1. Why tomatoes stop at 10 beds when they’re the money crop

Revenue for each bed of tomatoes is $8,800–roughly 4.2x carrots and 3.2x mesclun, as stated in our hypothesis. However, labor hours required are 3x carrots and 2x mesclun, and diminishing returns are 4x carrots and 8x mesclun. In other words, costs begin (and remain) higher and rise faster. F19 and F20 show standalone MC rising from $8,249 (10th bed) to $9,391 (11th bed), same as MC in the solved mix (K19 and K20). In other words, the price line sits between MC of bed 10 and MC of bed 11. At 10 beds, price is $551 higher than MC (I19), but at bed 11, MC is $591 higher (I20). We also see in `tomatoes-mc.png` that both MC curves cross the price curve between bed 10 and 11.

## 2. Which constraints bind—and what relaxing one is worth

At our 10/20/30 solved mix, we used 60 of 64 beds and 5,277 of 6,480 labor hours, so these constraints are not binding. Our binding constraints are the bed caps for carrots and mesclun. Price still exceeds MC for carrots and mesclun at their caps ($405, I54; $280, I93) (see `carrots-mc.png` and `mesclun-mc.png`), and the value of relaxing the caps can be found by calculating P - MC for each additional bed beyond their respective caps. Then, we can decide whether to exceed the caps by planting the four unused beds. For example, the value (Price - MC) of carrot beds 21-24 would be ~$352, $298, $242 and $183, respectively. Bed 31 of mesclun would be ~$246. So, taking the four highest of these values, we could increase our profit by ~$1,138 (I55 + I56 + I57 + I94) by relaxing the cap and planting three additional carrot beds. If there are any costs we haven’t yet accounted for, the shadow price is the most it’s worth paying to relax each cap by a bed. If we were to plant these additional beds, our 64 bed cap would become a binding constraint.

## 3. The tomato MC dip at 6 beds

MC reflects what you pay for inputs, not just how much of them you use. In this case, this means both what we pay for labor as well as the number of hours. The MC dip (`tomatoes-mc.png`) at 6 beds is a feature of the calculations being done in a “standalone” fashion, as though tomatoes were the only crop being planted. All 720 hours of the farmer’s labor at a higher wage are exhausted first, then the hourly wage shifts to the lower temp worker’s rate. We see MC drop from bed 5 to bed 6 ($7,661, F14; $4,906, F15) because we exhaust the farmer’s available hours during bed 5 (724.73, B14), and MC of bed 6 is calculated at the lower wage. All three figures include a second MC curve for the 10/20/30 solved mix where labor is pooled across all three crops, which removes this dip feature from all three curves. K14 and K15 show no dip in MC (solved mix) for tomatoes. Diminishing returns still applies regardless of the hourly wage. Each extra tomato bed adds more hours, about 168, 198, 232 and 271 for beds 4–7 (B13−B12 through B16−B15). The extra hours for each additional bed kept rising while MC fell from bed 5 to bed 6.

## 4. Why grow crops that lose money on their own

MC tells us how many beds to plant, and AVC tells us whether to plant at all. Carrots and mesclun “lose money” on their own: carrots −$16,489 (J54), mesclun −$11,922 (J93), because all $20,000 of fixed cost would still apply–fixed cost remains constant whether zero beds or all 64 are planted–and would surpass any difference between price and AVC. The short-run shutdown rule states that a firm should stop production when price falls below AVC. At 20 beds of carrots, price is $2,094 vs $1,918.45 AVC (G54), so carrots contribute 20 x ($2,094 - $1,918.45) or about $3,511 (J54), and at 30 beds mesclun, price is $2,700 vs $2,430.74 AVC (G93) so mesclun contributes 30 x ($2,700 - $2,430.74) or about $8,078 (J93). Therefore we should not shut down on account of either crop, and this ‘price - AVC’ gap is what each crop contributes toward covering the $20,000 fixed cost. The three figures show standalone AVC curves for each crop. For tomatoes, AVC crosses price right around bed 16 (G25), for carrots it remains below price the entire graph, and for mesclun it rises above price at beds 13-14, then falls again. Because these AVC values were calculated using the farmer’s higher wage first, it is a stricter test than the real mix, and the mesclun bump (G76 and G77) is irrelevant. At the quantity we actually plant (30), AVC is below price.

## Against the Stage 1 hypothesis

The model’s result matched my stage 1 hypothesis, and my basic reasoning was sound. I predicted that carrots and mesclun would be planted to their respective caps, and 10 beds of tomatoes would be optimal because MC of the 11th bed would surpass revenue. This turned out to be exactly what the model found, and MC for tomato beds 9-11 turned out to be the same for both standalone (F18:F20) and solved mix (K18:K20). I got the calculations correct by using the temp worker wage, even though I did not state why. I also stated that there were 14 beds available for tomatoes. This is true–there are 14 beds left after carrots and mesclun are planted–however, tomatoes end up stopping at 10 with four beds left empty. Lastly, I cited ratios rather than MC values for carrots and mesclun as my reason for predicting they would both be capped.
