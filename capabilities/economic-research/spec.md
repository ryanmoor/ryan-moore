---
type: spec
capability: economic-research
engagement: research-paper
date: 2026-10-09
status: built            # draft | built | audited
built_with: "Claude Code, from this file"
---

# Economic Research — model specification

## Purpose
These graphs, re-created from Kamepalli et al. (2022) pg 34, Figures 1a and 1b, show the number of early-stage social media startups funded by venture capital, and total venture capital investment dollars going to early-stage financing between 2001 and 2019. This data, consistent with a "kill zone" around incumbent platforms, supports our decision to recommend stricter review requirements of proposed M&A deals for aggregators like Meta and Google.

## Inputs — the named contract
| Name | Value | Unit | Source |
|---|---|---|---|
| `Year` | 2001–2019 | calendar year | Kamepalli, Rajan & Zingales (2022), *Kill Zone*, NBER WP 27146, Fig. 1, p. 34 |
| `DEALS(Year)` | 19 values, [data/kill-zone.md](../../data/kill-zone.md) | early-stage start-ups financed by a VC, social media space, per year | same; underlying data PitchBook; values read from the figure image |
| `INVEST_USD_M(Year)` | 19 values, [data/kill-zone.md](../../data/kill-zone.md) | USD millions (nominal, early-stage financings), per year | same |

## Structure
- `Data` sheet: one row per year, 2001–2019 (19 rows). Columns: `Year`,
  `DEALS`, `INVEST_USD_M`, and `Note` (marks the extrapolated endpoints; see
  Conventions).
- `Fig1a` chart: line chart of `DEALS` against `Year`.
- `Fig1b` chart: line chart of `INVEST_USD_M` against `Year`.

## Calculation logic
None. Both series are inputs plotted as given; no transformation, deflation
or normalization is applied:

  Fig1a = line(Year, DEALS(Year)),        Year = 2001 … 2019
  Fig1b = line(Year, INVEST_USD_M(Year)), Year = 2001 … 2019

## Conventions
- **Source of the values.** The paper prints no numbers for Figure 1; the
  values are read off the two raster images embedded on printed p. 34
  (PDF p. 35; 200 dpi, about 6 deals and about $8M per pixel), at each
  year's gridline. They are approximations of PitchBook data, not PitchBook
  data.
- **Rounding.** Values are rounded to the nearest 10 (deals) and the nearest
  $10M (investment). Read precision is about ±10 deals and ±$15M.
- **Endpoints.** At 2001 and 2019 the line meets the chart frame and tick
  marks, so those two values are extrapolated from the line's slope just
  inside the frame and carry a wider error (about ±25 deals, ±$40M).
- **Units.** `DEALS` is a count of early-stage start-ups financed by a
  venture capitalist in the social media space in that year;
  `INVEST_USD_M` is the dollar amount of those early-stage financings, in
  millions of USD, nominal (the paper does not say it is inflation-adjusted).
- **Axes.** Recreated charts use the original ranges: x 2001–2019; y 0–3,000
  (Fig1a) and 0–4,000 (Fig1b).
- **Chart format.** Chart title 18 pt bold; axis titles 11 pt bold. Titles
  sit outside the plot area, and the plot area is inset (13% left, 17% top,
  about 5% right and 19% bottom) so no title or axis label overlaps the
  gridlines or tick labels.
- **Labeling.** Each chart's caption credits Kamepalli, Rajan and Zingales
  (2022), Figure 1, data from PitchBook, and states that the values are
  read from the published figure. No repository URL appears in any caption.

## Validation rules
- `Data` has exactly 19 rows (2001–2019) with no blank cells in `DEALS` or
  `INVEST_USD_M`.
- `DEALS` peaks in 2014 at about 2,220; `INVEST_USD_M` peaks in 2016 at about
  $3,070M.
- Overlay check: each recreated chart, scaled to the original axes and laid
  over the image on printed p. 34 (PDF p. 35), traces the original line to
  within the read precision above at every year.
- Every chart has a title, axis labels with units, and the source caption.

## Outputs
- `Fig1a`: Number of deals, 2001–2019.
- `Fig1b`: Dollar amount financed (in million $), 2001–2019.

## Audit findings

### 1. Overlay check (Validation rules)
- **Checked:** the 19 values for each series, read from the `Data` sheet of
  `model.xlsx`, drawn as straight-line segments on the original Figure 1
  images extracted from the paper (printed p. 34, PDF p. 35), at the
  original's own pixel scale (about 6 deals and about $8M per pixel). The
  vertical gap between the two lines was measured at every pixel column
  (769 for Fig1a, 783 for Fig1b). The overlay image is not kept in the
  repository because it contains the paper's copyrighted figure.
- **Found:** both charts are within the spec's tolerance at every year.
  - Fig1a: largest gap at a year 8 deals (2011); endpoints 2 (2001) and 7
    (2019); median gap along the whole line 2.3 deals; largest anywhere 10.9,
    just after 2014.
  - Fig1b: largest gap at a year $15M (2018), exactly at the tolerance;
    endpoints $5M (2001) and $3M (2019); median gap $3.2M; largest anywhere
    $23.3M, just after 2018.
  - The larger between-year gaps fall on steep segments, where a sub-pixel
    horizontal offset shows as a large vertical gap (about $34M per pixel on
    the 2018–2019 drop); the tolerance applies at each year.
  - The 2018 dollar value sits at a sharp corner, where line thickness makes
    the reading less precise: this measurement gives about $2,215M, the
    original reading $2,199M; the workbook has $2,200M.
  - No feature of the original falls between the annual points; straight
    lines between the 19 values reproduce both curves.
  - Limitation: the values were read from the same images, so this confirms
    the reading, rounding and transcription, not the underlying PitchBook
    figures, which the paper does not publish.
- **Did:** no action.

### 2. Visual comparison (Validation rules)
- **Checked:** `kill-zone-1a.png` and `kill-zone-1b.png` compared by eye,
  year by year, against the original Figure 1 on printed p. 34 (PDF p. 35).
- **Found:** it matches.
- **Did:** no action.
