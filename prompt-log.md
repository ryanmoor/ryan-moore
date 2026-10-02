# Prompt Log

Running record of consequential AI session related to these projects — one row per session/task,
newest entries at the top.

## Format

| Date | Tool | Asked | Produced | What I did with it |
| --- | --- | --- | --- | --- |
| YYYY-MM-DD | e.g. Claude | One-line summary of what I asked for | One-line summary of what the tool produced | Kept as-is / edited / discarded / used as a starting point, etc. |

## Log

| Date | Tool | Asked | Produced | What I did with it |
| --- | --- | --- | --- | --- |
| 2026-09-22 | Claude | Commit my uploaded Excel-solved model.xlsx and merge to main; set spec.md frontmatter to date 2026-09-21 / status audited; address Adam's feedback to derive the three rounded inputs (carrot hours = 2.5/3; both wage rates = salary / hours instead of $34.72 and $17.36) in spec.md and model.xlsx | Solved workbook committed and merged; frontmatter updated; spec.md Inputs and model.xlsx Inputs sheet changed to `LaborHrsPerBedCarrots = LaborHrsPerBedTomatoes/3`, new `Salary` input (25,000), `OwnWageRate = Salary/OwnLaborHours`, `TempWageRate = Salary/TempHoursPerWorker`; merged to main | Confirmed the derivations before the edits; wrote the commit messages; separately added a validation hand check to spec.md and changed carrot hours in the brief to 2.5/3 myself |
| 2026-09-21 | Claude | Fill out capabilities/marginal-analysis/spec.md from the course spec template; apply my own frontmatter, header, Purpose and Inputs text; fill out Structure and Calculation logic; open the workbook in Excel and run Solver | spec.md rewritten into the template sections (Purpose, Inputs, Structure, Calculation logic, Conventions, Validation rules, Outputs, Audit findings, the last from a Python replica check); model.xlsx sent to me; could not run Solver — LibreOffice headless failed to load the workbook | Wrote Purpose and Inputs myself; told it to keep 17.36 and the original input names; ran Solver in Excel myself (solved workbook committed 9/22) |
| 2026-09-18 | Claude | Build an Excel/Solver workbook from the case info showing the optimal beds of each crop to maximize profit; add frontmatter and the course spec template outline to spec.md | model.xlsx rebuilt in place (Inputs/Calc/Output sheets, named ranges, Solver setup on Output, beds left unsolved); spec.md frontmatter, a Perfect Competition per-engagement section and the template outline; marginal-analysis README lists the engagement; merged to main | Chose the file name (model.xlsx) and location; used the template outline as the starting point for the 9/21 spec work |
| 2026-09-17 | Claude | Reviewed successive revisions of the perfect-competition brief's Hypothesis/Falsifiers against Adam's feedback; verified the marginal-cost-vs-revenue arithmetic behind the tomato bed count each round; answered conceptual questions on why the case is titled "perfect competition" and on price-taking vs. aggregate price formation | Round-by-round critique (adequacy vs. feedback, typos/grammar, arithmetic checks) delivered in chat only; no edits to the brief itself | Incorporated across revisions; committed final hypothesis/falsifiers to main |
| 2026-09-14 | Claude | Synced working branch to the committed perfect-competition brief; critiqued the brief (implicit assumptions, unsupported claims, likely client questions, hypothesis falsifiability) | Branch fast-forwarded/pushed; critique delivered in chat only, no edits to the brief itself | Added assumptions that were left out of the first draft |
| 2026-09-13 | Claude | Fixes per Adam's feedback | capabilities/README.md, docs/README.md, .md extension on the perfect-competition brief | Edits to Repository per Adam's feedback |

<!-- Add new rows above this line, newest first. -->
