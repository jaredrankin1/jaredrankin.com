# BrickStash — App Inventory (observed, not simulated)

> Everything in this file was **actually observed** on 2026-11-07 with headless Chromium (Playwright) against
> `https://brickstash-demo-web.onrender.com/welcome`. Viewports: phone 390×844, desktop 1440×900, and 720×450 CSS px @2× DPR
> as a 200%-zoom stand-in for P4. The simulated session in `transcript.md` may only use UI listed here.

## Setup and data used

| Item | Value |
|---|---|
| Test account | `focusgroup-sim-20261107@example.com`, business name "Focus Group Test Shop" (created on the demo's own `/signup`; `example.com` can't receive mail). The password is in the run notes, not in this repo. |
| Analytics consent | Always chose **"No thanks"**, except in the first anonymous load, so the study adds as little as possible to BrickStash's own analytics. One session clicked nothing, so the banner stayed. |
| Data created | 1 bin ("Bin 1"), 1 photo-added fig ("Minifig 1", from a placeholder image), 1 search-added fig (sw0218 Darth Vader – Chrome Black), graded 6.5, saved to the account as **BC0022** (item id 42), price set to $405.49. |
| Prop images | `study/props/*.jpg` are **hand-drawn placeholder figures**, not photos of real minifigs (see §Props). |
| Network caveat | The study sandbox blocked `jaredrankin.com`, where the background-removal model (`silueta.onnx`) is hosted. The app logged `cutout model unavailable, using the plain-background cutout` and fell back. **Photo-cutout quality was therefore not tested as real users would see it.** |
| Server warmth | No Render cold start was observed. The service was warm for the whole run. Real page load to interactive was ~0.5–1.7 s (timings in run logs exclude fixed waits). |

## App shell

- Expo / React Native Web shell at `/welcome/*` with a **bottom tab bar on every viewport**: Home · Inventory · Storefront · Market (tab labels are 11.5 px).
- The **Home tab is an iframe** of `/front-door`: a Matter.js physics canvas plus HTML overlays.
- An analytics consent card ("Help us improve BrickStash? … Nothing is shared or sold. Details / No thanks / Allow") appears on first visit in each storage context, including again on the separate `/signup` web page. It covers the lower ~20% of the phone screen.
- **Desktop (1440×900)** uses the same single-column phone layout, stretched to full width (screens 40–46).
- **200% zoom** reflows cleanly, with no clipping in the shell (screens 50–53).

## Screens reached

| # | Screen / state | Screenshot(s) | Visible text & controls | Notes |
|---|---|---|---|---|
| 1 | Home, signed out, first visit | `screens/01-home-anon-consent-banner.png`, `02-home-anon-after-consent.png` | "BrickStash" wordmark, **Sign in** button, ⚙ gear, a lone blue **+** on a blank canvas, tab bar, consent card | **No tagline, no copy, no explanation of what the app does.** The canvas aria-label reads: "A full-screen field of objects drifting… a cutout of every minifig you photograph. Tap the plus to add one." |
| 2 | Settings ("Modes") tray | `09-home-settings-modes.png` | MODES toggles: Dark, Gravity, Tidy (on), Phone, Medieval, Disco, Space, Sound (on) | Novelty/visual modes only. No account, units, currency, or data settings. Sound is on by default. |
| 3 | Add wheel | `10-home-add-wheel.png`, `17-home-add-wheel-remembers-last.png` | Radial menu: **Fig** (top), **Bin** (right), **Set – COMING SOON** (bottom), **Search** (left); the centre repeats the last-used action | The centre label changes (Fig → Bin) depending on last use. Arrow keys and Enter work. |
| 4 | Fig → camera | `11-home-camera-unavailable.png` | Black viewfinder, "The camera isn't available here. Choose a photo of a minifig instead." **Choose file**, **Cancel** | Headless/desktop has no camera. On phones the markup includes "Plain background, light from one side, a hand's length away", **Take photo**, **Flash off**. |
| 5 | Photo processing | `12-home-photo-snip.png` | Large cutout preview, status "Snip, snip…" | Cutout used the plain-background fallback (see network caveat). |
| 6 | Fig on canvas | `13-home-fig-and-bin-on-canvas.png` | Sticker-style cutout drifting among a bin and the + | Physics re-shuffles positions on each load. |
| 7 | Fig card, unidentified | `14-home-fig-card-unnamed.png` | "Minifig 1 · Added Nov 7, 2026 · Not named yet. Retake it on a plain background with the fig filling the frame." **Grade**, **Remove** | **No way to name or search-match an unidentified photo fig.** Bins have a rename pencil; figs don't. |
| 8 | Grade sheet, unnamed fig | `15-home-grade-unnamed-defaults.png` | "BrickCheck grade 9.1 Great (GR) · Name the fig to see a price"; sliders Surface / Form / Print clarity / Color, all preset to **8** | A 9.1 grade appears before the user has assessed anything. |
| 9 | Bin card | `16-home-bin-card.png` | "Bin 1 ✎ · 0 things in this bin · Delete" | Bin created instantly with the toast "New bin: Bin 1." |
| 10 | Search dialog | `18-home-search-dialog-results.png` | "Add a fig" title, **Cancel**, input placeholder "Name or BrickLink id, like sw0001a", status "8 found. Tap one to add it.", rows with image, name, BrickLink ID, and theme | Search for "Darth Vader" → 8 results (one result, sw1515, had no image). **Home search returns at most 8 results**, with no "more" option. Generic names like "darth vader", "batman", "luke skywalker", and "clone trooper" all hit the cap of 8, so many variants are unreachable without the exact BrickLink ID. Word order matters: "chrome vader" → **0**, "sw0218" → 1. "spaceman" → only col013 (CMF); "space man" → twn323; "classic space" → 8. (Market search is a different endpoint and returns up to 24.) |
| 11 | Search-added fig on canvas | `19-home-search-fig-added.png` | Catalogue image cutout of Vader | |
| 12 | Fig card, identified | `20-home-vader-card-index-price.png` | "Darth Vader - Chrome Black · sw0218 · Added Nov 7, 2026 · **$578.40** · Index price, before grading · used $398.51 · new $559.36 · Added from the BrickCheck MMi catalogue." **Grade**, **Remove** | **The index price ($578.40) is higher than the "new" price ($559.36).** |
| 13 | Grade sheet, identified | `21-home-grade-default-9_1.png`, `22-home-grade-adjusted-6_5.png`, `23-home-grade-crack-checklist.png` | Score, label, and live price; four 1–10 sliders with icons and helper text; **CRACK CHECKLIST** (collapsible Hairline / Moderate / Stress mark × 11 locations: left/right arm, left/right heel, front/back/left/right torso, head, helmet, accessory); **Save grade**, **Cancel** | Defaults 8/8/8/8 → **9.1 Great, $578.49**. Setting 5/7/4/6 → **6.5 Seasoned, $405.49**, and the sprite visibly "wears" in preview. One moderate front-torso crack → **no change** in score or price (confirmed with `/api/front-door/grade`). Three cracks → composite 6.28, price unchanged. API extremes: all 10s → "New/Mint (MI)", ×1.2, **$694.49**; all 1s → "Poor (P)", ×0.6, $347.49. |
| 14 | Card after grading | `24-home-grade-saved-card.png` | "$405.49 · BrickCheck 6.5 · Seasoned (S) · index $578.40", **Regrade**, **Remove** | |
| 15 | Home **Save** | `25-home-save-toast.png` | Toast: "Added 1 item to your inventory, ready to price. Graded as BC0022." | **The only action that moves Home items to the account.** The unnamed "Minifig 1" was silently skipped. The bin was not mentioned. The Save element's fallback `href` is `/signup`. |
| 16 | Fresh browser, same account, before Save | `26-home-fresh-browser-before-save-empty.png` | Empty canvas with + | Until Save is pressed, Home items exist **only in that browser's local storage**. |
| 17 | Drag fig into bin | `54-home-drag-fig-into-bin.png` | Vader disappears into the bin, tooltip "Bin 1" | Works, but only by dragging, and nothing on screen suggests it. |
| 18 | Inventory, empty | `27-inventory-empty.png` | Title, ⚙ "View and sort", Account, search "Search names, IDs, serials", segmented **In stock / To price**, "No stock yet · Add a fig on Home to start." | |
| 19 | View and sort sheet | `28-inventory-view-sort.png` | VIEW: Items / Bins / Groups · SORT: Value ↑↓, Date added ↑↓, Grade ↑↓ · TILE SIZE: Large / Medium / Small / Tiny · **Done** | No filters (theme, condition, bin), only view, sort, and size. |
| 20 | Inventory, To price | `29-inventory-to-price-date-unknown.png`; desktop `42-desktop-inventory-to-price.png` | "1 item", group header **"Date unknown"**, tile with grade badge 6.5 (desktop also shows $405.49 badge) | The fig was added today, yet it's grouped under "Date unknown". There's a large blank gap above the first group on phone. |
| 21 | Item detail | `30-item-detail.png`, `31-item-detail-price-field.png`; desktop `43-desktop-item-detail.png`; zoom `52-zoom200-item-detail.png` | Header shows the ID ("sw0218"); name; price "—"; "To price · Used"; BRICKCHECK BC0022, GRADE 6.5, LABEL Seasoned (S); INDEX $578.40, USED AVG $398.51, NEW AVG $559.36; MMI COMPASS radar + bars; **PRICE** field (0.00) + **Save price**; **QUANTITY** (1) + **Save qty**; **SHELVES** "Not on a shelf yet." | The graded price ($405.49) is **not** prefilled; the user must type it. After **Save price**, the item **stays in To price** (status `pending_review`). No visible save confirmation was observed. No photo of the owner's actual fig (`photo_count: 0`). Crack disclosure is stored server-side but not shown on this screen. |
| 22 | In stock after pricing | `32-inventory-in-stock-still-empty.png` | "No stock yet" | No control in the app moves an item from To price to In stock. |
| 23 | Inventory search by BrickCheck serial | `33-inventory-search-serial-no-match.png` | "Nothing matches · Try fewer letters, or the BrickLink ID." | Searching "BC0022" fails, despite the "serials" placeholder. Name and BrickLink ID searches work (verified via API: "Vader", "sw0218", "chrome" → 1 result in To price). |
| 24 | Storefront | `34-storefront-coming-soon.png` | "Coming soon · Connect your shop here and list straight from your inventory." | Placeholder. |
| 25 | Market | `35-market.png`; desktop `44-desktop-market.png` | Black hero "BrickCheck minifig index · 2026-10-16 · **168.0** · ▲ 0.1% this month · 300 figs"; "Search the market"; TOP MOVERS SINCE MARCH 2026 (15 rows, % change); MOST VALUABLE (15 rows, $); footer "Market data from MMI, shown at the Collector level." | **Thumbnails are blank white squares** (the app never requests the images). "Most valuable" tops out at $53.23 (index constituents only), while the catalogue prices sw0218 at $578.40. |
| 26 | Market search | `36-market-search-spaceman.png` | RESULTS: "Spaceman, Series 1 (Minifigure Only without Stand and Accessories) · col013 · $7.74" | "spaceman" misses the "Classic Space – …" figures. "classic space" returns 24. |
| 27 | Market fig detail | `37-market-detail-zane-dx.png`; desktop `45-desktop-market-detail.png`; zoom `53-zoom200-market-detail-compass.png` | Back arrow; header shows the **ID** (njo0018); image, name, **$53.23**, "Used $30.01 New $99.84"; "The index price is the market average every BrickCheck grade multiplies: a 9.6–9.9 fig prices at ×1.10, a 10 at ×1.20."; MMI COMPASS radar (Liq, Mom, TVM, Scar, Prem, Theme, Vel, Supply) + bars; "0 to 100 on each axis… Higher is stronger." | The Supply value "3: oversupplied" wraps one or two letters per line. "Supply 3 = oversupplied" contradicts "Higher is stronger" when read literally. **No "add to my stash" action.** |
| 28 | Account, signed out | `03-account-signin.png` | SIGN IN Email / Password, **Sign in**, **Create a free account**, "Accounts are created on the web for now. Come back here and sign in once yours is set up."; SERVER address field + **Save server**; LEGAL AND SUPPORT (Terms, Privacy, Contact support@reusebricks.com); footer "Prototype build… (docs/native-app.md, Phase 0)." | A dev-facing server URL field and internal doc reference are visible to end users. |
| 29 | Wrong password | `04-account-wrong-password.png` | "Invalid email or password." in red, **below** the sign-in card | Easy to miss. While the consent card is open, it is hidden behind it. |
| 30 | Sign-up (web, opens in a new browser tab) | `05-signup-web.png`, `06-signup-short-password-error.png`, `07-signup-success.png`; desktop `48-desktop-signup-web.png` | Red lowercase "brickstash" wordmark (differs from the app's black "BrickStash"); **BUSINESS NAME** "What your shop is called"; EMAIL; PASSWORD "At least 12 characters — this account holds your cost basis and your storefront keys."; **Create account**; success: "You're all set. If that address can be registered, your account is ready. Sign in with the email and password you just chose." | No email verification. A rejected password **clears the password field**. The user must manually return to the app tab and sign in. |
| 31 | Account, signed in | `38-account-signed-in.png`; desktop `46-desktop-account.png`; zoom `51-zoom200-account.png` | "SIGNED IN · focusgroup-sim-…@example.com · **Your inventory and bins come from this account.**" **Sign out**; SERVER; LEGAL AND SUPPORT; **Delete account** | The copy implies Home items sync automatically. They only sync after **Save** (row 15). Delete account was not pressed. |
| 32 | 404 | `39-404-welcome-trailing-slash.png` | "404 — MISSING PART · We couldn't find that page… Back to the dashboard" | **Reloading Home 404s**: the app rewrites the URL to `/welcome/`, and a direct load of `/welcome/` returns 404 (`/welcome` without the slash is 200). Deep links `/welcome/inventory`, `/market`, `/account` load fine. |
| 33 | Desktop Home / wheel / grade | `40-desktop-home.png`, `41-desktop-add-wheel.png`, `47-desktop-grade-sheet.png` | Same as phone, wider canvas | The grade sheet is a ~340×520 floating panel; **Save grade is below its internal fold**. Helper text is ~11 px. |

## Measured legibility (for P4)

Heuristic measurement from computed styles (screens 35, 30, 23). Gradient backgrounds can skew the automated contrast numbers, so treat these as approximate.

- Tab bar labels **11.5 px**. Market and item metadata (IDs, themes, section labels) **12–12.5 px**. Grade-sheet helper text and the "CRACK CHECKLIST" label **~11 px**.
- Grade label "Seasoned (S)" ≈ **4.2:1** contrast (just under WCAG AA 4.5:1 for 13 px text). The orange "1 ticked" count ≈ **3:1**.
- Body text generally passes AA contrast. The problem is **size**, not colour.
- Crack-checklist checkboxes are **18×18 px** targets.

## Unreachable, missing, or broken (as reached through `/welcome`)

| Area | Status | Evidence |
|---|---|---|
| **Sets** | "COMING SOON" on the Add wheel | `10-home-add-wheel.png` |
| **Storefront** | "Coming soon" placeholder | `34-storefront-coming-soon.png` |
| **Export / share / report** | Not present anywhere in `/welcome` | Account, Inventory, and Market screens (27–38) |
| **Bulk entry / import** | Not present. Each fig is one wheel tap plus a photo or search. | 10, 18 |
| **Naming an unidentified photo fig** | Not possible; dead end | `14-home-fig-card-unnamed.png` |
| **Condition fields beyond wear and cracks** | No sealed/new toggle, no "missing accessory / incomplete", no "not genuine / counterfeit", no "mixed parts". The item is stored as "Used". | 23, 30 |
| **Part-level inventory** | Not present. Figures are the only unit; "Part" was replaced by Search on the wheel (code comment). | 10 |
| **Multi-user / client separation** | Not present in `/welcome` | 38 |
| **Moving an item to In stock** | No control | 31, 32 |
| **Market thumbnails** | Blank | 35 |
| **Home reload** | 404 at `/welcome/` | 39 |
| **Photo-cutout ML model** | Blocked by the *study sandbox*, not necessarily broken for users | run logs |
| **Desktop back office** (`/inventory`, `/quick-add`, `/scan`, `/review`, `/bins`, `/stats`, `/market`, `/info`, `/settings`, "Load sample inventory", "Reports", "Alfred · AI assistant", BrickLink/Shopify sync filters, banner "DEMO / STAGING — not connected to the live store") | **Exists, but is not linked from `/welcome`**; `/` redirects to `/welcome` on desktop and phone. Captured once as `60-unlinked-web-inventory-desktop.png`. **Out of scope for participants** under the "only reachable UI" rule. | 60 |

## Props (`study/props/`)

Placeholder JPEGs drawn in SVG and rendered with Playwright. They are **not** photos of real figures, so photo identification was never expected to match them, and it didn't (`14-home-fig-card-unnamed.png`).

| File | Stands in for |
|---|---|
| `M2-mint-licensed.jpg` | M2 popular licensed figure, mint |
| `M3-classic-spaceman.jpg` | M3 1980s spaceman, light wear |
| `M4-worn-castle-knight.jpg` | M4 heavy wear |
| `M5-cracked-torso.jpg` | M5 cracked torso |
| `M8-knockoff.jpg` | M8 knockoff |
| `M9-yellowed-white.jpg` | M9 yellowed white |

Only `M2-mint-licensed.jpg` was actually uploaded. In the transcript, the other photo uploads are narrated as producing the same observed outcome (unnamed fig, screen 14). That is an **assumption**: real photos of real figures may identify correctly.
