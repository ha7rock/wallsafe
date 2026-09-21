# wallsafe

**Batch phone wallpapers with taste — aesthetics, diversity, creativity first.**

A Codex skill that turns a short preference into a set of wallpapers you can actually choose from: intentional looks, different craft languages, same theme. Safe-zones and true device pixels are the delivery floor.

> 中文：关注**生成内容品质**——审美、多样性、创意。遮挡安全区与真机分辨率是交付底线，不是卖点本身。

---

## Why it exists

Anyone can ask a model for “cat wallpaper.” Most batches come back as near-duplicates: same pose, same lighting, different filters.

wallsafe is opinionated about **quality of the set**:

1. **Aesthetics** — light, palette, materials, negative space
2. **Diversity** — each frame is a different medium / visual language
3. **Creativity** — specific scenes and twists, not stock tropes

Then it still ships like a product: lock-clock and dock bands respected, finished at real resolution (e.g. iPhone 17 Pro Max `1320×2868`).

---

## Gallery — theme: cats · iPhone 17 Pro Max

Same subject. Six crafts. Built to compare side by side.

| Photoreal ambient | Flat vector | Soft watercolor |
| :---: | :---: | :---: |
| <img src="examples/thumbs/01-photoreal-ambient.jpg" width="180" alt="Photoreal ambient" /> | <img src="examples/thumbs/02-flat-vector.jpg" width="180" alt="Flat vector" /> | <img src="examples/thumbs/03-soft-watercolor.jpg" width="180" alt="Soft watercolor" /> |

| 3D clay | Minimal color-field | Ink wash |
| :---: | :---: | :---: |
| <img src="examples/thumbs/04-3d-clay.jpg" width="180" alt="3D clay" /> | <img src="examples/thumbs/05-minimal-colorfield.jpg" width="180" alt="Minimal color-field" /> | <img src="examples/thumbs/06-ink-wash.jpg" width="180" alt="Ink wash" /> |

Full-resolution PNGs: [`examples/iphone-17-pro-max/cats/`](examples/iphone-17-pro-max/cats/)

---

## What “good” means here

| Pillar | Look for |
| --- | --- |
| Aesthetics | Clear light and palette; calm enough to live under icons |
| Diversity | You can name each craft without squinting |
| Creativity | At least one concrete beat per image |

Delivery floor (always on): quieter top/bottom bands, no text/mockups, exact device `W×H` after generate.

---

## Quick start (Codex)

```bash
cp -R skills/wallsafe ~/.codex/skills/wallsafe
```

Then: *“Use wallsafe. iPhone 17 Pro Max. Theme: quiet rainy cats. 8 wallpapers — maximize style diversity.”*

Skill source: [`skills/wallsafe/SKILL.md`](skills/wallsafe/SKILL.md)

---

## Design note / 设计说明

English: wallsafe is a creative director for wallpaper batches, with a production checklist attached — not the reverse.

中文：先保证一批图**好看、有差异、有想法**；再用安全区与真机像素把它们收成能上锁屏的交付物。

---

## License

MIT
