# BrickStash — Simulated Focus Group (2026-11-07)

> **Read first:** the screens, bugs, and API behaviour here were **really observed** on
> `https://brickstash-demo-web.onrender.com/welcome` with Playwright + Chromium. The participants, observers, quotes,
> and every score are **simulated**. Use this as a heuristic review and a set of hypotheses, not as user research.

| File | What it is |
|---|---|
| `app-inventory.md` | Every screen reached, controls, measured legibility, and unreachable or broken areas (observed) |
| `screens/` | 54 screenshots: phone 390×844 (01–39, 54), desktop 1440×900 (40–48, 60), 200% zoom (50–53) |
| `props/` | Placeholder figure images used for the upload path (drawings, not real photos) |
| `transcript.md` | The 90-minute simulated session with `[OBS]` commentary; UI lines cite screenshots |
| `task-results.md` | Participant × task outcomes, times, errors, SEQ, totals (SIMULATED) |
| `findings.md` | 26 issues ranked Critical → Cosmetic, with screens, evidence, heuristic, and fix |
| `exec-debrief.md` | C-suite takeaways, disagreements, consensus, next steps (business opinions) |
| `sus-scores.md` | Simulated SUS per participant (not real usability data) |
| `limitations.md` | What this can't tell you, confidence levels, and the 5 questions for real users |

**The three critical findings:**
- **C1:** figures added on Home only reach your account after an unexplained "Save".
- **C2:** a photo the app can't identify is a dead end and is silently skipped on Save.
- **C3:** the price can't be defended. A crack doesn't move it, and the index ($578.40) sits above the new price ($559.36) for sw0218.
