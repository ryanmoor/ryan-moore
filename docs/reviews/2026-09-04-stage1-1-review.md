<!-- PR TARGET: https://github.com/ryanmoor/ryan-moore | Stage 1.1 -->
# Stage 1.1 review — engagement brief

**Brief:** [`docs/briefs/perfect-competition-brief.md`](https://github.com/ryanmoor/ryan-moore/blob/main/docs/briefs/perfect-competition-brief.md)

> Checked 2026-09-04. Your repository changed more in one day than anyone else's has in the whole course — see your Stage 0 review, which moved a long way. This stage did not move with it, and it is the cheapest of the three still open.

### What is in the file right now

docs/briefs/perfect-competition-brief has the four section headings — The Problem, Assumptions, Hypothesis, Falsifiers — and nothing under any of them. The frontmatter reads status: in process and hypothesis: TBD.

It is also missing its file extension. The deliverable path is docs/briefs/perfect-competition-brief.md; without the .md it is not a Markdown file, GitHub will not render it, and the automated path check does not see it. That rename is ten seconds and it should happen regardless of when the writing does.

### What the stage asks for

About a page, in your own words, committed before you build anything.

- The problem. What is being decided, by whom, and what it costs to decide it badly. What is fixed, what you choose, and what limits the choice.

- What you are assuming, and which of those you would most want to test with more time.

- Your hypothesis. Three real numbers — beds of tomatoes, carrots, mesclun — and the mechanism you think decides it. You are not graded on being right. A hedged prediction that would survive any outcome is the only kind that is worthless.

- How you would know you were wrong. The specific result that would falsify it. "If the model shows a different mix" tests nothing; "if the model plants more than 14 tomato beds, I underestimated how fast the 10 percent labor penalty compounds" is a real test.

### The shape of the case

64 beds, a 36-week season, $20,000 of fixed costs. The farmer is paid $50,000 and works 720 field hours; up to four temporary workers are available at $25,000 each for 1,440 hours each. The farm cannot move prices — it takes what the market gives.

Tomatoes earn $8,800 a bed, carrots $2,094, mesclun $2,700. The caps are 20, 20 and 30, which sum to 70 against 64 beds, so all three cannot be maxed and something has to give.

What makes it interesting is that labor compounds: each additional bed of a crop raises the hours every bed of that crop needs, at 10 percent per bed for tomatoes, 2.5 for carrots, 1.25 for mesclun. So the crop that earns most per bed also gets expensive fastest, and the question is where those cross.

### Why this one comes before the model, and it is not a formality

Stage 1.3 asks you to explain why your prediction and your model disagreed. That is only writable if a real prediction was committed before the model existed, and your commit history is the proof of which came first. A hypothesis written after the workbook runs is worth nothing even when it is correct.

You have a specification and a workbook in the repository already — which is ahead of most of the cohort on sequencing effort and behind on sequencing order. Write the page first. It is an hour.

### How this reached you, and how to fix that

This arrived as a Lamaku comment and an email rather than as a pull request on your own files, because I do not have push access to your repository. You are one of three people in that position and it is the one thing costing you real feedback quality — a review anchored to the line it is about is worth much more than a comment box, and your Stage 1.2 work is now at the point where that matters.

To fix it: your repository on github.com, then Settings, then Collaborators, then Add people, then adamwstauffer. It takes about thirty seconds and every future review arrives in the repository.

---

### How to work this review

Treat this PR the way an analyst treats feedback from a senior reviewer — a review is a proposal to engage with, not a checklist to rubber-stamp.

1. **Read it yourself first.** Form your own view before you change anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM.** Paste this review and your brief into your assistant and ask it to (a) explain anything you are unsure of, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change.
3. **Then write the changes yourself.** For a brief this matters more than usual: a hypothesis you did not generate cannot be honestly compared against your model in Stage 3, and that comparison is the entire point of writing the brief first.
4. **Close the loop.** Reply in this thread with what you changed and what you pushed back on, then commit and push.

*One standing rule: do not revise your hypothesis to match what your model later tells you. If the model contradicts the brief, that is a finding, not an error.*

*Your score and the per-criterion breakdown are in your Lamaku comment, not here — this repository is public.*

— Adam
