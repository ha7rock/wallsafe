---
name: wallsafe
description: >-
  Use this when Codex should generate a batch of phone wallpapers from a short
  preference prompt (theme/mood) for iOS/Android, with wallpaper-safe
  composition and post-resize to real device pixels.
---
# Phone wallpaper batch (Image Gen 2.5 / Codex)

Generate **5–10 distinct phone wallpapers** from a short user preference. This is a **wallpaper pipeline**, not generic image gen: every prompt must obey wallpaper composition rules, device pixel rules, and style diversity within one theme.

## When this applies

- User wants phone wallpapers (home / lock) from a theme, mood, or soft preference
- Target is iPhone or Android phone aspect (portrait)
- Running under **Codex CLI** on a machine with ChatGPT/Codex image gen

## Codex environment reality (important)

On Codex-authenticated boxes (ChatGPT login, `OPENAI_API_KEY` often null):

1. Prefer **Codex built-in image generation** (or `codex exec` driving that tool).
2. Built-in generation may **ignore exact pixel size** and may not expose `gpt-image-2.5-flare` selection.
3. Always **post-process** with ffmpeg (or equivalent) to the device delivery size:
   `ffmpeg -y -i INPUT -update 1 -frames:v 1 -vf "scale=W:H:force_original_aspect_ratio=increase,crop=W:H" OUTPUT`
4. If a standalone `image_gen.py` CLI is available **and** a real platform API key exists:
   - `gpt-image-2` accepts custom sizes (edges multiple of 16), e.g. `1312x2864`
   - `gpt-image-2.5-flare` in the current system CLI may only allow legacy sizes `1024x1024` / `1024x1536` / `1536x1024` / `auto` — then generate portrait `1024x1536` and upscale/crop to delivery
5. Do **not** block the batch asking the user for permission to relax model/size when the environment cannot meet them — adapt, document in `manifest.json`, finish the batch.

Non-interactive Codex tip: pass the prompt as a CLI argument and redirect stdin from `/dev/null`. Avoid piping that leaves Codex stuck on `Reading additional input from stdin...`.

## Inputs (collect lightly)

| Field | Default if missing |
| --- | --- |
| Theme / mood / subject | Required — ask once if absent |
| Device | `iphone-17-pro-max` if known; else ask iOS vs Android |
| Count | `8` (clamp 5–10) |
| Mode | `both` (lock+home friendly); `lock` if user says lock screen |
| Avoid list | empty |
| Style list | auto (Style wheel) |

Same theme, **different visual languages** — never a matching sticker series.

## Device → pixels

### Delivery size

| Device key | Delivery `W×H` |
| --- | --- |
| `iphone-17-pro-max` | `1320×2868` |
| `iphone-17-pro` | `1206×2622` |
| `iphone-generic` | `1290×2796` |
| `android-fhd+` | `1080×2340` |
| `android-qhd` | `1440×3200` |

### Preferred generation size (when API allows custom)

Edges must be multiples of 16; long/short ≤ 3:1.

| Device key | Generate then crop/scale to delivery |
| --- | --- |
| `iphone-17-pro-max` | `1312×2864` → `1320×2868` |
| `iphone-17-pro` | `1200×2624` → `1206×2622` |
| `android-fhd+` | `1088×2336` → `1080×2340` |

Fallback when only legacy sizes work: generate `1024×1536`, then scale/crop to delivery.

## Wallpaper composition (non-negotiable)

Bake into every prompt:

1. Full-bleed smartphone wallpaper, portrait, single frame — not a mockup, bezel, or UI screenshot
2. **Top ~12–15%** calm / low-detail (Dynamic Island, status, lock clock)
3. **Bottom ~15–20%** avoid critical subject (Home Indicator + dock)
4. **`lock` / `both`:** keep upper-center relatively open for large clock
5. **No text** by default (no letters, logos, watermarks)
6. Avoid whole-frame high-frequency noise so icons stay readable
7. Clear depth; full bleed; no borders, Polaroid, collage grid

## Prompt template (English for the image model)

```text
Smartphone wallpaper, portrait full-bleed, {aspect} phone background.
Theme: {theme}.
Style: {one style only}.
Scene: {1–2 concrete sentences}.
Composition: main interest mid-to-lower band; top status/island band and bottom dock band simpler; lock-clock-friendly open upper center.
Lighting / mood: {from preference}.
Constraints: no text, no logos, no watermark, no phone frame, no UI chrome, no collage, not a sticker sheet.
```

## Style wheel

Pick N different styles (examples): photoreal ambient, flat vector, soft watercolor, gouache/poster, 3D soft clay, minimal color-field, 35mm film still, ink wash/sumi, retro halftone, dreamy bokeh close-up.

Change camera distance, palette, and composition each time.

## Workflow

1. Parse inputs; resolve device + delivery size.
2. Choose N styles.
3. Generate N images via best available Codex/Image path.
4. Resize/crop each to **exact delivery** resolution.
5. Save: `wallpapers/{device}/{theme-slug}/{nn}-{style-slug}.png` + `manifest.json` (theme, styles, tool/model used, sizes, prompts, limitations).
6. Show a short index; offer refinement on favorites only.

## Acceptance checks

Regen if: mockup/borders, busy under clock/island or dock, text/watermark, near-duplicate of another in the batch.

## Example

Device iPhone 17 Pro Max, theme cats, count 6 → six styled wallpapers at `1320×2868`.
