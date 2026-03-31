---
name: aso-appstore-screenshots
description: Generate high-converting App Store screenshots for an iOS app by discovering core benefits, pairing simulator screenshots, and producing App Store Connect sized images. Use when the user mentions App Store screenshots, ASO screenshots, screenshot set, App Store Connect images, or wants marketing screenshots for iOS.
---

## Goal

Guide the user through creating a cohesive App Store screenshot set using a repeatable workflow: Benefit Discovery, Screenshot Pairing, Generation, and Showcase.

## Project State (Cursor replacement for memory)

Persist state in the user project so the workflow is resumable.

- Create a folder at `./aso-appstore-screenshots/`
- Write and update these files:
  - `./aso-appstore-screenshots/benefits.md`
  - `./aso-appstore-screenshots/screenshot-analysis.md`
  - `./aso-appstore-screenshots/pairings.md`
  - `./aso-appstore-screenshots/generation.md`

At the start of every run, read those files if they exist and show a short status summary, then continue from the next incomplete phase.

## Phase 1: Benefit Discovery

Only run this phase if benefits are not confirmed in `benefits.md` or the user asks to redo it.

- Explore the codebase to understand:
  - What the app does, who it is for, what is differentiated
  - What screens exist and what the user can do
  - Monetization, onboarding, and the premium value
- Ask targeted questions to fill gaps the code does not answer.
- Draft 3 to 5 benefits in this format:
  - `ACTION VERB` plus `BENEFIT DESCRIPTOR`
  - Specific, user outcome focused, and conversion oriented
- Iterate until the user explicitly confirms the benefit list and order.
- Save to `./aso-appstore-screenshots/benefits.md`:
  - App context, target audience, confirmed benefits (ordered)
  - Any wording preferences and competitor notes

## Phase 2: Screenshot Pairing

- Ask the user for simulator screenshot paths or a directory.
- View each screenshot and rate it as Great, Usable, or Retake:
  - What it shows, what works, what does not, and why
- Recommend which screenshot best matches each benefit:
  - Relevance, visual impact, clarity at thumbnail size, and uniqueness
- Iterate until the user confirms pairings.
- Save:
  - Analysis to `./aso-appstore-screenshots/screenshot-analysis.md`
  - Pairings to `./aso-appstore-screenshots/pairings.md`

## Phase 3: Generation

### Output structure

Write outputs to:

- `./screenshots/01-benefit-slug/` working files
- `./screenshots/final/` approved App Store ready files

### Target dimensions

Ask which App Store Connect portrait size is needed, default to iPhone 6.7 inch.

- iPhone 6.5 inch: 1242 by 2688
- iPhone 6.7 inch: 1290 by 2796
- iPhone 6.9 inch: 1320 by 2868

### Step A: Create deterministic scaffolds (local)

Use the repo script `./compose.py` to render pixel perfect scaffolds.

- Requirements:
  - `pip install Pillow`
  - SF Pro Display Black at `/Library/Fonts/SF-Pro-Display-Black.otf`
  - Device frame template at `./assets/device_frame.png` (generate with `python3 ./generate_frame.py` if missing)

Generate scaffolds for each benefit and paired screenshot into each benefit folder as `scaffold.png`.

### Step B: AI enhancement (optional)

If the user wants polished marketing renders beyond the scaffold, generate 3 variants per scaffold using the image generator with the scaffold as a reference image.

- Keep the headline wording and layout consistent across the set.
- Keep the background a clean, solid brand colour.
- Do not introduce extra copy or watermarks.

Prompt templates and strict consistency rules are in `reference.md`.

### Step C: Enforce exact App Store dimensions

If any AI generated output is not exact App Store Connect dimensions, crop and resize to the exact target size before showing the user. Keep text within a generous horizontal safe area.

### Step D: Approve and promote to final

After the user chooses a winner for each benefit, copy the chosen file to `./screenshots/final/NN-benefit-slug.png` or `.jpg` and record the selection in `./aso-appstore-screenshots/generation.md`.

## Phase 4: Showcase

After at least 1 final screenshot exists, generate a side by side preview using `./showcase.py` and save to `./screenshots/showcase.png`.

## Additional guidance

Open `reference.md` for:

- Benefit phrasing checklist
- Screenshot critique rubric
- Enhancement prompt templates
- Crop and resize command snippet for macOS

