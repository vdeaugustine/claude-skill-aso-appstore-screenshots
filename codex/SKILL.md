---
name: aso-appstore-screenshots
description: Generate high-converting App Store screenshots for an app by discovering user-facing benefits, rating simulator screenshots, pairing each benefit with the strongest screen, and producing polished App Store-ready exports with a deterministic scaffold plus AI enhancement.
---

# ASO App Store Screenshots

Guide the user through creating a cohesive, high-converting App Store screenshot set for an iOS app.

Keep momentum high, but do not skip confirmation gates for benefits, screenshot pairings, or final picks.

## Workflow

Run these phases in order unless saved project state says work was already completed and the user wants to resume:

1. Recall saved state
2. Benefit discovery
3. Screenshot analysis and pairing
4. Generation
5. Showcase

## State Persistence

Codex should not rely on conversation memory for this skill. Persist progress in project-local files so the user can resume in a later session.

Use this directory in the user's app project:

```text
.codex/aso-appstore-screenshots/
```

Use these files:

- `benefits.md`
- `screenshots.md`
- `generation.md`

If the directory does not exist, create it when first saving state.

At the start of every run, read any of these files that exist and present a short status summary:

- confirmed benefits
- screenshot assessments
- confirmed pairings
- brand color
- generated finals

Then ask whether to resume, revise a phase, or regenerate a specific screenshot. If no saved state exists, start at Benefit Discovery.

## Benefit Discovery

Only run this phase if no confirmed benefits exist or the user explicitly wants to redo them.

### 1. Analyze the app

Inspect the codebase and product materials to understand:

- what the app actually helps users do
- who it is for
- the core user problem
- premium or differentiated features
- the strongest visual screens likely to convert

Read only the files needed to build a concrete product model. Prefer app UI, onboarding, models, copy, README, and metadata.

### 2. Ask targeted questions

After analysis, summarize your current understanding and ask only the missing questions that materially affect messaging, such as:

- target audience
- primary reason someone downloads the app
- competitor context
- strongest user outcomes

### 3. Draft benefits

Draft 3 to 5 screenshot headlines. Each must:

- start with a strong action verb
- focus on the user outcome, not internal implementation
- be specific enough to sell the app at thumbnail size
- be short enough to fit comfortably in the top safe area

Present them in order with one line of rationale each.

### 4. Refine

Do not move on until the user confirms the final benefit list. Push for specific, conversion-oriented phrasing when the user drifts toward generic headlines.

### 5. Save

Write `.codex/aso-appstore-screenshots/benefits.md` with:

- app name
- bundle id if available
- target audience
- confirmed ordered benefits
- product context and positioning notes
- wording preferences the user expressed

## Screenshot Analysis And Pairing

Only run this phase once confirmed benefits exist.

### 1. Collect screenshots

Ask the user for simulator screenshot paths, a directory, or a glob. Review every provided image with the available image viewing tools.

### 2. Rate each screenshot

Rate each screenshot as `Great`, `Usable`, or `Retake`.

For every image, record:

- file path
- what the screen shows
- what works visually
- what hurts conversion
- verdict

Be direct about empty states, sparse data, debug chrome, poor status bars, weak visual hierarchy, and screens that do not read at App Store thumbnail size.

### 3. Coach retakes

For every `Retake`, give exact instructions:

- which screen to capture
- what data state it should show
- how full or visually rich the screen should be
- status bar guidance
- light/dark mode consistency

### 4. Pair benefits to screenshots

Recommend the strongest screenshot for each benefit. Prefer relevance, clarity, and visual impact. Avoid reusing a screenshot unless there is no better option.

Do not proceed to generation until the user confirms the pairings.

### 5. Save

Write `.codex/aso-appstore-screenshots/screenshots.md` with:

- every screenshot reviewed and its rating
- analysis notes
- confirmed benefit-to-screenshot pairings
- retake guidance still outstanding

## Generation

Only run this phase after benefits and pairings are confirmed.

### Preconditions

Before generating, verify that an image generation or image editing tool is actually available in the current Codex environment. If it is not available, stop and explain that this skill can still do benefit discovery and screenshot pairing, but AI enhancement needs an installed image tool.

Also verify local prerequisites as needed:

- `python3`
- Pillow for `compose.py`
- `/Library/Fonts/SF-Pro-Display-Black.otf`

If a prerequisite is missing, say exactly what is missing and stop before generation.

### Display size

Default to iPhone 6.7 inch screenshots at `1290 x 2796` unless the user specifies another App Store Connect slot.

Supported portrait sizes:

- iPhone 6.5 inch: `1242 x 2688`
- iPhone 6.7 inch: `1290 x 2796`
- iPhone 6.9 inch: `1320 x 2868`

### Brand color

Do not make the user choose by default. Propose a single strong background color after analyzing:

- app tint or theme colors in code
- dominant UI palette in screenshots
- product category and tone

State the chosen color and brief reasoning. Let the user override it.

Save the confirmed choice into `generation.md` before creating scaffolds.

### Layout rules

All screenshots in the set must be visually consistent:

- same background color
- same headline treatment
- same device treatment
- same overall composition style

Use this format:

- line 1: large uppercase action verb
- line 2: smaller uppercase descriptor
- centered device mockup
- phone pushed high enough to feel dynamic
- bottom of device bleeding off the canvas

Keep headline text safely inside the center of the canvas so later crop operations do not cut it off.

### Stage 1: deterministic scaffold

Use `compose.py` from this skill directory to create the scaffold image for each confirmed benefit and screenshot pairing.

Output working files into the user's project root:

```text
screenshots/
  01-benefit-slug/
    scaffold.png
```

The scaffold is an internal intermediate. Do not ask the user to approve it.

### Stage 2: AI enhancement

Generate 3 enhanced variations for each benefit from the scaffold.

For the first approved screenshot in the set:

- use the scaffold as the layout anchor
- preserve the exact headline wording
- preserve the app screenshot content
- preserve the background color
- upgrade the device rendering and overall polish
- only add breakout elements when a specific UI panel clearly reinforces the headline

For later screenshots in the set:

- use the new scaffold as the layout anchor
- use the first approved screenshot as the style template
- keep the same overall aesthetic across the set

### Crop and resize

AI image tools rarely output exact App Store Connect dimensions. After each batch of 3 variations, crop and resize them to the exact required size before showing anything to the user.

Crop width from the sides and preserve the top composition so headline placement survives.

### Review loop

Show only the resized outputs to the user as `Version 1`, `Version 2`, and `Version 3`.

If the user wants changes:

- keep using the scaffold as the layout anchor
- keep using the first approved final as the style template
- use the user's preferred version for creative direction
- regenerate 3 new options
- crop and resize again before review

### Finalization

When the user picks a winner, copy the resized approved image into:

```text
screenshots/final/
```

Use ordered filenames:

- `01-benefit-slug.jpg`
- `02-benefit-slug.jpg`

Update `.codex/aso-appstore-screenshots/generation.md` after each approved screenshot with:

- target display size
- brand color
- benefit headline
- working folder
- approved version
- final output path

## Showcase

Once multiple final screenshots exist, offer to generate a side-by-side showcase with `showcase.py`.

Use it for review, portfolio presentation, or a repo preview image.

## Operating Rules

- Be opinionated about conversion quality.
- Prefer a smaller set of strong screenshots over a larger weak set.
- Do not let the workflow drift into generic marketing copy.
- Do not move past a confirmation gate silently.
- Keep saved state files concise and structured so future sessions can resume quickly.
- When generation tools are unavailable, still complete the strategic parts of the workflow instead of failing early.
