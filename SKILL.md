---
name: wallsafe
description: >-
  Use this when Codex should generate a batch of phone wallpapers where content
  quality comes first — aesthetics, diversity, and creative range — from a short
  preference prompt, then deliver wallpaper-safe composition at real device pixels.
---
# wallsafe

Generate **5–10 phone wallpapers** from a short preference. The point of this skill is **content quality**: taste, variety, and creative range within one theme. Device safe-zones and exact pixels are the **delivery floor**, not the creative brief.

## North star (quality first)

Optimize for, in this order:

1. **Aesthetics** — intentional light, palette, materials, negative space; feels designed, not stock
2. **Diversity** — each image is a different visual language / craft, not a near-duplicate
3. **Creativity** — surprising but coherent scenes; specific moments, not generic “cat wallpaper” tropes

Reject a batch that is technically sized correctly but looks flat, samey, or cliché.

## When this applies

- User wants a set of phone wallpapers from a theme, mood, or soft preference
- They care how the set *looks and varies*, not only that images exist
- Target is iPhone or Android portrait delivery

## Creative brief (do this for every batch)

### Aesthetics

For each image, decide and state in the prompt:

- Light: direction, softness, time of day / OLED-friendly contrast
- Palette: limited, intentional (avoid muddy rainbow)
- Materials / medium: match the chosen style (film grain, pigment, clay, ink, vector edge)
- Atmosphere: one clear mood word (quiet, humid, sharp, nostalgic…)

Prefer **one strong subject + breathing room** over busy illustration soup.

### Diversity (style wheel)

Pick N **different** styles. Never reuse the same medium twice in one batch.

Examples: photoreal ambient · flat vector · soft watercolor · gouache/poster · 3D soft clay · minimal color-field · 35mm film still · ink wash/sumi · retro print/halftone · dreamy bokeh close-up

Also vary across the batch:

- Camera distance (extreme close / medium / wide environmental)
- Palette temperature (warm vs cool)
- Subject scale and placement (still respect safe bands below)

**Anti-pattern:** eight stickers of the same pose with different filters.

### Creativity

- Concrete scenes (“silver tabby on wet slate at blue hour”), not abstract “beautiful cat”
- One twist per image (weather, era, material, framing) without breaking the theme
- Avoid famous IP, logo marks, and meme templates

## Delivery floor (wallpaper craft — required, not the headline)

Still bake into every prompt:

1. Full-bleed smartphone wallpaper, portrait, single frame — no phone mockup / bezel / UI chrome
2. **Top ~12–15%** calmer (status, Dynamic Island, lock clock)
3. **Bottom ~15–20%** quieter (Home Indicator + dock)
4. Mode `lock` / `both`: keep upper-center relatively open for the large clock
5. No text / logos / watermarks by default
6. Avoid whole-frame noise that kills icon readability

These constraints protect aesthetics on a real lock screen; they do not replace the creative brief.

## Inputs (collect lightly)

| Field | Default if missing |
| --- | --- |
| Theme / mood / subject | Required — ask once if absent |
| Aesthetic lean (optional) | e.g. quiet OLED, soft daylight, high contrast |
| Device | `iphone-17-pro-max` if known; else ask iOS vs Android |
| Count | `8` (clamp 5–10) |
| Mode | `both`; `lock` if user says lock screen |
| Avoid list | empty |
| Style list | auto from style wheel |

## Device → pixels

| Device key | Delivery `W×H` |
| --- | --- |
| `iphone-17-pro-max` | `1320×2868` |
| `iphone-17-pro` | `1206×2622` |
| `iphone-generic` | `1290×2796` |
| `android-fhd+` | `1080×2340` |
| `android-qhd` | `1440×3200` |

Preferred generate sizes when custom API sizes work (edges ×16): e.g. `1312×2864` → crop/scale to `1320×2868`. Legacy fallback: `1024×1536` then scale/crop.

## Codex environment reality

On ChatGPT/Codex auth (`OPENAI_API_KEY` often null):

1. Prefer Codex built-in image generation
2. It may ignore exact pixels — always ffmpeg to delivery size
3. Do not stall asking permission to adapt model/size — adapt, note in `manifest.json`, finish

Non-interactive: pass prompt as CLI arg; stdin from `/dev/null`.

## Prompt template (English)

```text
Smartphone wallpaper, portrait full-bleed.
Theme: {theme}.
Creative intent: {aesthetic + one concrete creative beat}.
Style / medium: {one style only}.
Scene: {1–2 specific sentences — place, action, atmosphere}.
Light & palette: {clear choices}.
Composition: subject readable mid-to-lower band; top and bottom bands quieter for clock/dock; strong negative space where it helps taste.
Constraints: no text, logos, watermark, phone frame, UI, collage; not a sticker sheet; distinct from other images in this batch.
```

## Workflow

1. Parse preference → write a one-line creative north star for the batch
2. Choose N styles for maximum *visual* diversity
3. Author N prompts (aesthetics + creativity first; safe bands included)
4. Generate; post-resize to exact delivery
5. Save `wallpapers/{device}/{theme}/{nn}-{style}.png` + `manifest.json`
6. Quality gate before handoff (below)

## Acceptance checks (quality gate)

Regen if:

- Looks generic / stock / samey next to siblings
- Weak light or muddy palette
- Style diversity failed (two frames read as the same medium)
- Mockup/borders, text, or busy under clock/island/dock
- Near-duplicate crop of another image in the batch

## Example

Theme cats, iPhone 17 Pro Max, count 6 → six creatively distinct, aesthetically intentional wallpapers at `1320×2868`, not a matching pack.
