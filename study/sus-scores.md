# SUS Scores — SIMULATED

> ⚠️ **These are not real usability data.** A model filled in every response, in character, after a simulated session.
> SUS needs real people. These numbers summarize the personas' *simulated* experience and must not be quoted as
> a usability benchmark, compared to industry averages, or shown to investors as evidence.

System Usability Scale: 10 items, 1 = strongly disagree … 5 = strongly agree. Odd items are positive, even items negative.
Score = ((Σ(odd − 1) + Σ(5 − even)) × 2.5).

| Participant | Q1 use frequently | Q2 unnecessarily complex | Q3 easy to use | Q4 need tech support | Q5 well integrated | Q6 inconsistent | Q7 learn quickly | Q8 cumbersome | Q9 confident | Q10 learn a lot first | **SUS (SIMULATED)** |
|---|---|---|---|---|---|---|---|---|---|---|---|
| P1 Rob | 2 | 4 | 3 | 3 | 3 | 2 | 3 | 3 | 3 | 3 | **47.5** |
| P2 Tanya | 3 | 3 | 3 | 4 | 3 | 3 | 3 | 3 | 2 | 4 | **42.5** |
| P3 Luis | 2 | 3 | 4 | 2 | 3 | 3 | 4 | 2 | 2 | 2 | **57.5** |
| P4 Margaret | 2 | 4 | 2 | 5 | 2 | 3 | 2 | 4 | 2 | 5 | **22.5** |
| P5 Dev | 4 | 2 | 4 | 2 | 4 | 2 | 5 | 2 | 4 | 2 | **77.5** |
| P6 Keisha | 2 | 3 | 3 | 3 | 3 | 3 | 3 | 2 | 2 | 3 | **47.5** |
| P7 Tom | 2 | 4 | 3 | 3 | 2 | 3 | 3 | 3 | 2 | 3 | **40.0** |
| P8 Aiko | 2 | 2 | 4 | 2 | 4 | 3 | 4 | 2 | 4 | 2 | **67.5** |
| P9 Brandon | 1 | 4 | 4 | 2 | 3 | 3 | 4 | 3 | 2 | 2 | **50.0** |
| P10 Sofia | 3 | 3 | 4 | 2 | 3 | 3 | 4 | 2 | 3 | 2 | **62.5** |
| **Mean** | | | | | | | | | | | **51.5 (SIMULATED)** |

Range 22.5–77.5 (SIMULATED). Median 48.75 (SIMULATED).

## Reading the pattern (not the numbers)

- **The spread tracks the personas' goals more than the UI.** Dev (fun, low stakes) scores highest. Margaret (low vision, low tech comfort) scores lowest. The pros (Luis, Sofia, Aiko) land in the middle: they found the basics easy (Q3, Q7) but don't want to use it often (Q1) because their jobs-to-be-done aren't supported.
- **Q9 "confident using the system" is low for 7 of 10.** In the transcript this ties to three observed causes: the Save-to-inventory step (`25-home-save-toast.png`, `27-inventory-empty.png`), the unnamed-photo dead end (`14-home-fig-card-unnamed.png`), and doubts about the price (`20-home-vader-card-index-price.png`).
- **Q5 "well integrated" is low for the people who hit the Home ↔ Inventory split** (P1, P2, P4, P6, P7).

A model can't feel confusion, so a simulated SUS will be overconfident in either direction. Run real SUS with 5–8 real users per segment before citing any score.
