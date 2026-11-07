# Findings — ranked by severity

> **How to read this.** Each finding separates **Observed** (what the real app did during the Playwright run, with
> screenshots or API responses as proof) from **Simulated reaction** (which personas hit it and what they said).
> The observed part is real. The reactions, quotes, and "who hit it" are SIMULATED. Severity is our judgment of
> impact if real users behave as the personas did. Business opinions are in `exec-debrief.md`, not here.
>
> Heuristics are Nielsen's 10: H1 Visibility of system status · H2 Match with the real world · H3 User control & freedom ·
> H4 Consistency & standards · H5 Error prevention · H6 Recognition over recall · H7 Flexibility & efficiency ·
> H8 Aesthetic & minimalist design · H9 Help users recover from errors · H10 Help & documentation.

## Summary

| ID | Severity | Finding | Heuristics |
|---|---|---|---|
| C1 | Critical | Home items don't reach the account until an unexplained "Save" is pressed; until then they live only in that browser | H1, H5, H4 |
| C2 | Critical | A photo fig the app can't identify is a dead end (no way to name or match it) and is silently left out of Save | H3, H9, H1 |
| C3 | Critical | Valuation can't be defended: cracks barely move the grade or price, the index sits above "new", Mint prices 24% over new, and the grade defaults to 9.1 | H2, H1, H5 |
| M1 | Major | No way to record incomplete, counterfeit, mixed-part, sealed, or loose figures, which leads to misrecords | H2, H5 |
| M2 | Major | Home search returns 8 results max, no synonyms, word-order sensitive, so the wrong figure gets added silently | H5, H7 |
| M3 | Major | Home doesn't say what the app is or where to start; unlabelled + and gear | H6, H10, H2 |
| M4 | Major | No import, export, report, or bulk entry in the app under test | H7 |
| M5 | Major | Inventory status is a one-way street: priced items stay in "To price", the graded price isn't prefilled, and items show "Date unknown" | H1, H4 |
| M6 | Major | Sign-up leaves the app for a differently branded web tab, asks hobbyists for a "Business name", and wipes the password on error | H4, H9, H2 |
| M7 | Major | Low-vision barriers: 11–12.5 px text, sub-AA labels, the error shown outside the form, Save below the fold in the desktop grade panel | H1, H8, H9 |
| M8 | Major | Reloading Home gives a 404 (`/welcome/`) | H9 |
| M9 | Major | Desktop is the phone layout stretched to 1440 px; no table or dense view | H7, H8 |
| m1 | Minor | Market thumbnails are blank | H1, H8 |
| m2 | Minor | "Most valuable" lists only index constituents (max $53) | H2 |
| m3 | Minor | Inventory search can't find BrickCheck serials, despite the "serials" placeholder | H4 |
| m4 | Minor | Market compass jargon; Supply axis inverted versus "Higher is stronger"; value text wraps | H2, H8 |
| m5 | Minor | Grade labels are jargon ("Seasoned (S)", "Great (GR)") | H2 |
| m6 | Minor | Developer-facing content on the Account screen (server URL, internal doc path) | H8, H5 |
| m7 | Minor | The gear holds novelty "Modes" only, and Sound is on by default | H4 |
| m8 | Minor | Can't add a figure to your stash from the Market detail page | H7 |
| m9 | Minor | Drag-figure-into-bin works but nothing hints at it | H6 |
| m10 | Minor | The consent card reappears on `/signup` and covers content | H8 |
| c1 | Cosmetic | Detail headers show the ID (sw0218) instead of the name | H4 |
| c2 | Cosmetic | Pennies disagree: index $578.40 vs graded $578.49 at the same value | H4 |
| c3 | Cosmetic | Large blank gap above the first Inventory group on phone | H8 |
| c4 | Cosmetic | Web sign-up branding (red "brickstash") differs from the app (black "BrickStash") | H4 |

---

## Critical

### C1 — Home items don't reach the account until "Save" is pressed
- **Screens:** `13-home-fig-and-bin-on-canvas.png`, `25-home-save-toast.png`, `26-home-fresh-browser-before-save-empty.png`, `27-inventory-empty.png`, `38-account-signed-in.png`
- **Observed:** Figures, grades, and bins added on Home stay in browser local storage. A fresh browser signed into the same account showed an empty Home (26) and the dashboard API reported 0 items, until the bottom-left **Save** button was pressed. Save then reported "Added 1 item to your inventory, ready to price. Graded as BC0022." (25). Meanwhile the Account screen says "Your inventory and bins come from this account." (38). The Save button has no count, badge, or hint.
- **Simulated reaction:** P2, P4, P1, P6, P7 all expected Inventory to show their figures (27). P2 and P4 failed T5. P2: *"But I did! I added two!"* P4: *"Have I lost them?"* COO: *"Every support inbox will fill with 'I lost my figures'."*
- **Fix:** Sync each item to the account as soon as it's added (keep local storage as an offline cache). Until then, at minimum: label the button "Save 3 to Inventory", show an unsaved badge, and warn before the user leaves or signs out. Make the Account copy match whatever behaviour ships.

### C2 — An unidentified photo fig is a dead end, and Save skips it silently
- **Screens:** `14-home-fig-card-unnamed.png`, `15-home-grade-unnamed-defaults.png`, `25-home-save-toast.png`
- **Observed:** After a photo the app can't identify, the card says "Not named yet. Retake it on a plain background…" and offers only **Grade** and **Remove** (14). There's no control to name it or search for a match; bins have a rename pencil, figures don't. Its grade sheet says "Name the fig to see a price" (15). On Save, the unnamed fig was **not** synced and nothing said so ("Added 1 item" when there were 2 figs, 25).
- **Caveat:** the test photo was a drawn placeholder, so failing to identify it was expected. How often real photos fail is unknown (see `limitations.md`). The **missing recovery path** is real either way.
- **Simulated reaction:** P1, P2, P4, P5, P7 hit it. P1: *"A box that says 'Not right? Search for it.' Every app does that."* P3: *"So they just… don't count? Nothing told me that."* P2 read "Not named yet" as *"it's fake"*.
- **Fix:** Add "Find it in the catalogue" on the unnamed card, opening Search with the photo attached, and attach the user's photo to the record when they pick a match. Include unnamed figs on Save as "Needs ID", or explicitly say "2 figs need a name before they can be saved."

### C3 — The valuation can't be defended
- **Screens:** `20-home-vader-card-index-price.png`, `21-home-grade-default-9_1.png`, `22-home-grade-adjusted-6_5.png`, `23-home-grade-crack-checklist.png`, `37-market-detail-zane-dx.png`
- **Observed (UI + API, `/api/front-door/grade`, sw0218):**
  - The card shows "**$578.40** Index price, before grading · used $398.51 · **new $559.36**". The index is above new (20).
  - The grade sheet opens at sliders 8/8/8/8 = **9.1 "Great"**, priced, before the user has assessed anything (21).
  - Sliders 5/7/4/6 → 6.5, $405.49. Adding **one moderate front-torso crack → unchanged** (6.5, $405.49). Three cracks → composite 6.28, **price unchanged** (×0.7 band).
  - All 10s → "New/Mint", ×1.2 = **$694.49** (24% above the stated new price). All 1s → "Poor", ×0.6 = $347.49.
  - The source shown is "BrickCheck MMi catalogue" / "Market data from MMI, shown at the Collector level." No sale count, date range, or venue appears on the card. The multiplier explanation appears only on the Market detail screen (37), not on the card.
- **Simulated reaction:** P3, P6, P10, P1, P9 rated trust 2–3/7. P3: *"A moderate torso crack is a parts-only fig for me. It should crater the price, and it does nothing."* P10: *"If I quoted a customer $578 and they looked up $559 new… I've lost the customer."* Novices (P2, P5) trusted it *because it looked official*, which is the riskier outcome.
- **Fix:** (1) Make each crack severity move the price, and show by how much. (2) Cap graded loose prices at or below the new average, or explain on screen why they exceed it. (3) Start sliders unset ("Rate each") instead of 8. (4) Put "Based on N sales, <dates>, <venues>" and the multiplier sentence on the card itself. (5) Add a visible "estimate, not an appraisal" line where the price appears.

## Major

### M1 — No way to record incomplete, counterfeit, mixed-part, sealed, or loose figures
- **Screens:** `23-home-grade-crack-checklist.png`, `30-item-detail.png`
- **Observed:** Condition capture is four wear sliders plus cracks (3 severities × 11 locations). There's no field for missing accessories, not genuine, mixed parts, sealed/new, or loose joints. Items are stored with condition "Used" (30). The only accessory option is "accessory" *in the crack list*.
- **Simulated reaction:** All 10 failed T3. **P7 added a knockoff as a genuine figure with a price; P10 recorded a missing wand as "Hairline crack – accessory"** (a false disclosure). P8: *"These aren't edge cases for me. Half my figures are customs."*
- **Fix:** Add Completeness (complete / missing: [list]), Authenticity (genuine / suspected fake / not checked), Custom/mixed (with a component list), and New-sealed / Used. Let these flags drive or suppress pricing (a suspected fake → no price shown).

### M2 — Home search: 8-result cap, no synonyms, word-order sensitive
- **Screens:** `18-home-search-dialog-results.png`, `36-market-search-spaceman.png`
- **Observed:** Home search (`/api/front-door/search`) returns at most **8** results with no "more". "darth vader", "batman", "luke skywalker", and "clone trooper" all hit the cap. "chrome vader" → **0**. "spaceman" → only col013 (a 2010 CMF), while the 1980s figures are "Classic Space – …". "space man" → twn323.
- **Simulated reaction:** **P2 added the wrong figure (col013 for sp005) and believed she'd succeeded.** P8 took one wrong turn. P9: *"If mine weren't in the first eight, I'd need the ID."*
- **Fix:** Paginate or "show more"; add token-order-independent matching; add synonyms ("spaceman" → Classic Space, "stormtrooper" → Stormtrooper variants); add year and theme filters to tell variants apart; add an "Is this yours?" confirmation comparing the user's photo with the catalogue image.

### M3 — Home doesn't explain itself
- **Screens:** `01-home-anon-consent-banner.png`, `02-home-anon-after-consent.png`, `50-zoom200-home.png`, `09-home-settings-modes.png`
- **Observed:** Signed-out Home is a wordmark, Sign in, a gear, a blue **+** with no label, and a consent card. "Minifig" appears only in the canvas aria-label. The gear opens novelty modes, not settings.
- **Simulated reaction:** 0/10 could say what the app does in the 5-second test; after exploring, 8/10 landed on "prices" and none on "grading". P4: *"Words on the buttons… a 'start here'."*
- **Fix:** One line of copy on the empty canvas ("Photograph or search a minifig to grade and value it"), a labelled "+ Add a fig", and a sample stash or demo figure on first load. A dismissible tip that the playful canvas is the *add* surface and Inventory is the *list*.

### M4 — No import, export, report, or bulk entry
- **Screens:** `27-inventory-empty.png`, `28-inventory-view-sort.png`, `38-account-signed-in.png`, `17-home-add-wheel-remembers-last.png`
- **Observed:** None of these exist in `/welcome`. The fastest repeat entry is the wheel's centre button remembering the last action (17), then one search per figure. A desktop back office with "Reports", "Quick add", and "Scan · beta" exists at `/inventory` but has **no link from the app** (`60-unlinked-web-inventory-desktop.png`, not tested).
- **Simulated reaction:** T6 0/5. P1: *"Without them, it's a toy I'd have to re-type my life into."* P3 projected ~15 min for a $30 lot. P6 needs an insurance PDF.
- **Fix:** Short term, link the back office from Account/Inventory on desktop. Then: CSV/BrickLink XML import, CSV + PDF export (with the owner's photos, grades, values, date, and source), and a "lot" mode that keeps search open and adds repeatedly.

### M5 — Inventory status and pricing don't behave as users expect
- **Screens:** `29-inventory-to-price-date-unknown.png`, `30-item-detail.png`, `31-item-detail-price-field.png`, `32-inventory-in-stock-still-empty.png`, `42-desktop-inventory-to-price.png`
- **Observed:** The synced item lands in **To price** under "**Date unknown**" (it was added the same day). The item page shows price "—" and a Price field of **0.00**, even though the grade sheet priced it at $405.49. After **Save price** (server shows `bl_price 405.49`), the item **stays in To price**, and there's no control to move it to In stock. No save confirmation was observed. The desktop tile shows a "$405.49" badge while still labelled "To price".
- **Simulated reaction:** P6 (assisted) and P1. P6: *"It's still under 'To price'. So what does 'priced' mean?"*
- **Fix:** Prefill the graded price. Move the item to In stock once a price is saved, or rename the tab to the real state ("Needs review"). Confirm saves inline. Use `created_at` for grouping.

### M6 — Sign-up is a detour with the wrong framing for hobbyists
- **Screens:** `03-account-signin.png`, `05-signup-web.png`, `06-signup-short-password-error.png`, `07-signup-success.png`
- **Observed:** "Create a free account" opens `/signup` in a **new browser tab** with different branding (05). The first field is **BUSINESS NAME**. The password hint mentions "cost basis and your storefront keys". A too-short password **clears the field** (06). Success reads "If that address can be registered, your account is ready…" (07). The user must go back to the app tab and sign in manually.
- **Simulated reaction:** P5 and P2. P5: *"That was a lot of hops for a free app."* P2: *"Do I need a business to use this?"*
- **Fix:** Sign up in-app, or return to the app automatically signed in. Make Business name optional, or ask "Collector or seller?" first. Keep the password on error. Keep the anti-enumeration wording but make it a clear next step.

### M7 — Low-vision and older-user barriers
- **Screens:** `04-account-wrong-password.png`, `47-desktop-grade-sheet.png`, `50-zoom200-home.png`, `52-zoom200-item-detail.png`, `53-zoom200-market-detail-compass.png`
- **Observed (approximate, from computed styles):** tab labels 11.5 px; metadata 12–12.5 px; grade helper text and "CRACK CHECKLIST" ~11 px; "Seasoned (S)" ≈ 4.2:1 contrast; "1 ticked" ≈ 3:1; checkboxes 18×18 px. "Invalid email or password." renders **below the card**, outside the form (04). On desktop the grade sheet is a ~340×520 panel with **Save grade below its internal fold** (47). The + and gear are icon-only. At 200% zoom the layout itself reflows well (50–53).
- **Simulated reaction:** P4 needed 5 assists and failed T5; SUS 22.5 (SIMULATED). P4: *"I'm not stupid; it's just not for me, is it?"*
- **Fix:** 14 px minimum for informational text, 44 px targets, inline errors next to fields, a sticky Save/Cancel footer in the grade sheet, text labels on primary icons, and an AA contrast pass on grade labels and counts.

### M8 — Reloading Home gives a 404
- **Screens:** `39-404-welcome-trailing-slash.png`
- **Observed:** The app rewrites the URL to `/welcome/` on load. A direct GET of `/welcome/` returns **404** ("404 — MISSING PART"); `/welcome` returns 200. Reloading or bookmarking Home therefore fails. `/welcome/inventory`, `/market`, and `/account` deep links work.
- **Simulated reaction:** P4 (logged as a technical interruption, ~40 s).
- **Fix:** Serve the SPA for `/welcome/` (or redirect it to `/welcome`) on the server.

### M9 — Desktop is the phone UI stretched
- **Screens:** `40-desktop-home.png`, `42-desktop-inventory-to-price.png`, `44-desktop-market.png`, `46-desktop-account.png`
- **Observed:** Bottom tab bar with labels ~360 px apart, full-width list rows, one ~480 px inventory tile in a 1440 px window, no table or columns.
- **Simulated reaction:** P1 (*"Where's my table?"*), P10, P6.
- **Fix:** A max-width content column and a responsive grid/table at ≥1024 px, or route desktop users to the existing back-office table view.

## Minor

### m1 — Market thumbnails are blank
- **Screens:** `35-market.png`, `44-desktop-market.png`. **Observed:** White placeholder squares. The browser makes no image requests on Market, though the API returns `image_url` per row; detail pages do load images (37). **Hit by:** P3, P9, P5. **Fix:** Render the `image_url`. (H1, H8)

### m2 — "Most valuable" isn't the most valuable
- **Screens:** `35-market.png` vs `20-home-vader-card-index-price.png`. **Observed:** The list tops out at $53.23 (300 index constituents); the catalogue prices sw0218 at $578.40. **Hit by:** P2 (*"So the most valuable minifig in the world is fifty bucks?"*). **Fix:** Retitle it "Most valuable in the BrickCheck 300", or source it from the full catalogue. (H2)

### m3 — Serial search fails
- **Screens:** `33-inventory-search-serial-no-match.png`. **Observed:** The placeholder says "Search names, IDs, serials". "BC0022" (the number in the Save toast) → "Nothing matches"; name and BrickLink ID searches work. **Hit by:** P10. **Fix:** Index `bc_number`. (H4)

### m4 — Compass jargon and an inverted axis
- **Screens:** `37-market-detail-zane-dx.png`, `53-zoom200-market-detail-compass.png`. **Observed:** Axis labels "Liq, Mom, TVM, Scar, Prem, Theme, Vel, Supply". The legend says "Higher is stronger", but "Supply 3: oversupplied" and "74: limited" read in the opposite direction. The value column wraps "3: oversupplied" to 1–2 characters per line. **Hit by:** P1, P2. **Fix:** Full labels, per-axis tooltips, consistent direction, and a wider value column. (H2, H8)

### m5 — Grade labels are jargon
- **Screens:** `22-home-grade-adjusted-6_5.png`, `24-home-grade-saved-card.png`. **Observed:** "Seasoned (S)", "Great (GR)", "New/Mint (MI)", "Poor (P)". **Hit by:** P2 (*"Seasoned, like a steak?"*), P4. **Fix:** Plain-language subtitles ("Seasoned: clearly played with, all parts present"). (H2)

### m6 — Developer-facing content on Account
- **Screens:** `03-account-signin.png`, `38-account-signed-in.png`. **Observed:** An editable "SERVER" URL field and "Prototype build… (docs/native-app.md, Phase 0)". **Hit by:** P8, P9. **Fix:** Hide behind a developer flag. (H8, H5)

### m7 — The gear isn't settings
- **Screens:** `09-home-settings-modes.png`. **Observed:** Gravity, Medieval, Disco, Space, and Sound (on by default); no text size, currency, or account options. **Hit by:** P4 (looked for text size), P5 (loved it). **Fix:** Rename it "Modes" with a playful icon, and put real settings under Account. (H4)

### m8 — No "add to stash" from Market
- **Screens:** `37-market-detail-zane-dx.png`. **Observed:** The detail page has a price, an explanation, and the compass, but no action. **Hit by:** P5, P6. **Fix:** "Add to my stash" and "Add to wishlist" buttons. (H7)

### m9 — Bins only work by drag
- **Screens:** `54-home-drag-fig-into-bin.png`, `16-home-bin-card.png`. **Observed:** Dragging a fig onto a bin files it ("Bin 1" tooltip). The bin card only offers rename and delete; the fig card has no "Move to bin". **Hit by:** P8 (found it by accident; delighted). **Fix:** "Move to bin…" on the fig card, plus a one-time hint. (H6)

### m10 — The consent card reappears and covers content
- **Screens:** `01-home-anon-consent-banner.png`, `05-signup-web.png`, `08-home-signed-in-consent-banner.png`. **Observed:** Asked again on the `/signup` tab and again in each new storage context. It covers the bottom ~20% of the phone screen, including the Terms links on sign-up. **Hit by:** P2, P4, P5. **Fix:** Share the choice across app and web; make it smaller and non-overlapping. (H8)

## Cosmetic
- **c1** Detail page headers show the ID ("sw0218", "njo0018") instead of the name (`30-item-detail.png`, `37-market-detail-zane-dx.png`).
- **c2** Pennies disagree: the card's index is $578.40 while the 9.1 / ×1.0 grade shows $578.49 (`20-…`, `21-…`).
- **c3** Large empty band between "1 item" and the first group on phone (`29-inventory-to-price-date-unknown.png`).
- **c4** Two wordmarks: black "BrickStash" (app) vs red lowercase "brickstash" (web sign-up, 404) (`01-…`, `05-…`, `39-…`).

## What worked (also observed)

- **The grading preview.** The sprite visibly scuffs and yellows as sliders move (`22-home-grade-adjusted-6_5.png`). It was the best-received moment for novices *and* the skeptic (P2, P5, P8, P9).
- **BrickLink IDs as first-class input** in Search, with theme shown per result (`18-home-search-dialog-results.png`). The ID-first path took under a minute.
- **The add wheel**: large targets, keyboard support (arrow keys + Enter), and the centre remembers your last action (`10-…`, `17-…`).
- **The Market detail page explains the multiplier in one sentence** (`37-market-detail-zane-dx.png`), more transparency than most price sites give.
- **The crack checklist is stored** as a disclosure on the account record (`crack_disclosure` in the item API), a good base for listings and insurance.
- **Performance:** no cold starts, no errors, ~0.5–1.7 s loads throughout the run.

## Technical notes for the CTO (observed; not usability findings)

- `/front-door` ships hidden markup and code from jaredrankin.com (Résumé drawer, LinkedIn/Copy/Case-study buttons, "The interactive view couldn't load" fallback). Harmless, but it's leftover shared code in a product page.
- `?bsdebug=1` exposes `window.BS_debug` (bodies, figs, bins) in production. Low risk, but worth gating.
- Inventory item IDs are sequential integers (`/welcome/item/42`). **Not tested:** whether `/api/mobile/item/<id>` enforces tenant ownership. Worth a quick authorization check; this study deliberately did not probe other accounts.
- The background-removal model is loaded from `jaredrankin.com/brickstash/silueta.onnx`. If that domain is blocked (corporate networks, as in this sandbox), the app falls back silently. Consider serving it from the app's own origin.
