# Limitations

> This study is a **model-generated simulation** wrapped around a **real walkthrough** of the app. The walkthrough
> facts (screens, API responses, bugs) are real and reproducible. Everything about *people* (who struggled, what they
> said, scores, trust, willingness to pay) is invented by a model and needs real users to confirm or refute.

## What the simulation can't tell you

1. **Whether real people struggle where the personas did.** Personas are stereotypes written in advance, and the model knows the findings while playing them, so struggles get pinned on known issues (confirmation bias by construction).
2. **Any number.** Success rates, times, error counts, SEQ, trust, and SUS are all invented. They show *relative* friction between tasks at best. They aren't comparable to benchmarks and shouldn't be cited.
3. **Willingness to pay or segment size.** Price points came from personas whose motives were defined in the brief.
4. **Real photo identification.** The study couldn't test it: the test photos were **drawings** (`study/props/`), the phone camera couldn't be exercised headless, and the background-removal model was **blocked by the study sandbox's network** (the app used its plain-background fallback). Every "Not named yet" outcome after a photo (P2, P4, P5, P7 and the T3 photo attempts) is an **assumption** copied from one placeholder upload.
5. **Physical handling.** Reading tiny printing, checking neck stamps for fakes, judging cracks under light: the personas narrate this, but no one did it.
6. **Group dynamics.** Who dominated, who changed their mind, and how the room split are scripted.
7. **Long-term use.** Nothing here covers week-two retention, collections of 100+ figures, sync across devices over time, or any real reseller workflow end to end.
8. **The desktop back office.** `/inventory`, `/quick-add`, `/scan`, Reports, and the "Alfred" assistant exist but aren't linked from `/welcome`. The "no invented UI / reachable only" rule kept them out of scope, so **"no export" and "no bulk" mean "not in the app under test"**, not "not in BrickStash".
9. **Real accounts and data.** One throwaway account stood in for all ten seats. "Load sample inventory" was never pressed.

## Findings by confidence

**High (directly observed and reproducible; no users needed to confirm the behaviour itself):**
- C1 local-only Home items until Save; the Account copy contradicts it (fresh-browser test, dashboard API = 0 before Save).
- C3 mechanics: one crack → no change; index $578.40 > new $559.36; Mint ×1.2 = $694.49; default 9.1 (API responses recorded).
- M2 search cap of 8, "chrome vader" → 0, "spaceman" → CMF only.
- M5 priced item stays in To price; "Date unknown". M8 `/welcome/` 404. m1 blank thumbnails. m3 serial search.
- M1 absence of completeness, authenticity, mixed, and sealed fields.

**Medium (real UI, but how much it matters depends on real people):**
- M3 the 5-second-test failure. Real users may arrive with context (an ad, a referral) that the cold test lacks.
- M6 sign-up friction. M7 legibility. M9 desktop layout.
- C3 *impact*: whether pros actually distrust the number, and whether novices over-trust it.

**Lowest confidence (mostly simulated):**
- **C2 frequency**: how often real photos of real figures fail to identify. The missing recovery path is real; how often users hit it is unknown.
- That **P2 would pick the wrong spaceman and not notice**: plausible, but a single scripted event.
- **P7 adding a knockoff as genuine**: depends on behaviour no one observed.
- **The grading preview as a "signature moment"** that wins over skeptics: the model may over-credit a visually distinctive feature.
- All **willingness-to-pay** figures and the **segment priority** debate.
- **SUS** (`sus-scores.md`) in its entirety.

## The 3–5 questions that most need real users

1. **Does photo identification work on real figures, in real lighting, on real phones?** And when it fails, what do people do? (Decides how much C2 matters and whether "camera-first" is the right front door.) *Method:* 8 users × 5 real figures each, live camera, success rate plus recovery behaviour.
2. **Do store owners and resellers trust the BrickCheck price enough to quote it to a customer?** What source, sample-size, and date detail would change that? (Decides whether C3 is a blocker for paid tiers.) *Method:* 5–6 store/reseller interviews with their own inventory; side-by-side with BrickLink sold data.
3. **Do people understand that Home is where you add and Inventory is where your collection lives, and do they lose data across devices?** (C1 and the canvas-vs-list debate.) *Method:* unmoderated diary study over 1–2 weeks on two devices.
4. **Which segment converts: hobbyists who enjoy it, or stores and resellers who need throughput?** What's the first thing each would pay for (insurance PDF, lot intake, listing)? *Method:* fake-door or concierge tests of 2–3 conditional offers.
5. **Can a non-expert (parent or inheritor) correctly identify a specific variant from search results, and do they notice when they're wrong?** (M2 risk.) *Method:* 6 novices, each with 5 figures that have look-alike variants; measure wrong-match rate.

## Brief §0 items that were defaulted (please confirm or correct)

- Study goals, test accounts, core flows, out-of-scope areas, the minifig kit (mapped to real IDs in `transcript.md`), and the deliverable audience were **not supplied**. Defaults are listed at the top of `transcript.md`.
- Out of scope by assumption: Storefront ("coming soon"), Sets ("coming soon"), the unlinked desktop back office, **Delete account** (not pressed), and changing the **Server** setting (not touched).
