# ASO App Store Screenshots (Cursor) Reference

Keep this file as the long form reference. The main `SKILL.md` should stay concise.

## Benefit phrasing checklist

- Start with an action verb: TRACK, SEARCH, BUILD, SAVE, LEARN, SHARE
- Focus on what the user gets, not implementation
- Make it concrete and specific
- Prefer outcomes over features
- Keep headlines short enough to fit with wide side margins

Recommended format:

- Line 1: `ACTION VERB`
- Line 2: `BENEFIT DESCRIPTOR`

## Screenshot critique rubric

Rate each simulator screenshot:

- **Great**: Immediately understandable, rich content, visually compelling
- **Usable**: Demonstrates the benefit but could be stronger
- **Retake**: Empty state, sparse content, unclear, distracting chrome

Common retake reasons:

- Empty states, placeholder content, error screens
- Lists with only 1 to 2 rows when the feature implies more
- Login, onboarding, or settings screens as primary marketing material
- Status bar clutter or developer indicators
- UI too dense to read at thumbnail size

## Enhancement prompt templates

Use these when generating polished marketing renders with the scaffold as a reference image.

### Template: first screenshot (establish style)

Copy and adapt the bracketed sections:

```
This reference image is a scaffold for an iOS App Store screenshot. Transform it into a polished, high budget App Store marketing screenshot.

KEEP EXACTLY:
- The headline wording and its placement
- The app screen shown on the phone
- The background colour as a flat solid colour

IMPROVE:
- Replace any placeholder phone with a photorealistic modern iPhone mockup
- Add realistic lighting and subtle shadows for depth
- Keep the overall layout consistent with the scaffold

OPTIONAL BREAKOUT:
- Only if an obvious UI panel directly supports the headline, enlarge that full panel so it pops out in front of the phone.
- Do not rotate the panel.
- Add a soft shadow under the panel.
- If no relevant panel exists, do not add a breakout.

Do not add extra words, logos, or watermarks.
```

### Template: subsequent screenshots (match established style)

If you have an already approved screenshot, include it as a second reference image and instruct matching its style.

```
You are producing the next image in a cohesive App Store screenshot set.

REFERENCE 1: the scaffold defines layout, text placement, and which app screen to show.
REFERENCE 2: the approved screenshot defines the exact style to match, especially the phone rendering and overall polish.

REQUIREMENTS:
- Match the approved style very closely.
- Keep the background a flat solid colour.
- Keep the headline wording and placement from the scaffold.
- Do not add extra copy, logos, or watermarks.

OPTIONAL BREAKOUT:
- Only if an obvious UI panel directly supports the headline, enlarge that full panel as a breakout in front of the phone with a soft shadow.
- Otherwise, no breakout.
```

## Crop and resize (macOS)

If an image is not exactly the required App Store Connect dimensions, crop to the correct aspect ratio then resize.

Example loop for iPhone 6.7 inch (1290 by 2796):

```bash
TARGET_W=1290 && TARGET_H=2796 && \
for INPUT in screenshots/**/v1*.jpg screenshots/**/v2*.jpg screenshots/**/v3*.jpg; do
  [ -f "$INPUT" ] || continue
  OUTPUT="${INPUT%.jpg}-resized.jpg"
  cp "$INPUT" "$OUTPUT"
  W=$(sips -g pixelWidth "$OUTPUT" | tail -1 | awk '{print $2}')
  H=$(sips -g pixelHeight "$OUTPUT" | tail -1 | awk '{print $2}')
  CROP_W=$(python3 -c "print(round($H * $TARGET_W / $TARGET_H))")
  OFFSET_X=$(python3 -c "print(round(($W - $CROP_W) / 2))")
  sips --cropOffset 0 $OFFSET_X --cropToHeightWidth $H $CROP_W "$OUTPUT"
  sips -z $TARGET_H $TARGET_W "$OUTPUT"
  echo "--- $OUTPUT ---"
  sips -g pixelWidth -g pixelHeight "$OUTPUT"
done
```

Adjust `TARGET_W` and `TARGET_H` for other sizes:

- 1242 by 2688 (iPhone 6.5 inch)
- 1290 by 2796 (iPhone 6.7 inch)
- 1320 by 2868 (iPhone 6.9 inch)

