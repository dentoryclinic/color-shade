# Handoff: Dentory เลือกสียางจัดฟัน — redesign → implement in the real app

## Prompt to paste into Claude Code

> Update my existing braces-colour picker app (the single-file `index.html` published as the claude.ai artifact https://claude.ai/artifact/SmauxxvgwZ3nxn8f87H79Y, with `chart-ref.jpg` and `logo.png` beside it) to match the new design in this folder. Read `HANDOFF.md` first, then use `reference/Main.dc.html` as the exact visual spec (markup + inline styles + logic). Keep every existing feature working. Keep it a plain single-file HTML/CSS/JS page (no framework), Thai copy as written, mobile-first (390 px wide). When done, republish to the same artifact URL.

## What changed (summary)

The look moves to the clinic's **Dentory design system**: pastel sky ground, white cards, deep-navy text, pink as the action colour, everything pill/rounded, cute Gen-Z touches (gold sparkles, pink hearts, sticker-style tilted pills, "Explore / Pick your vibe ♡"). Function stays the same.

## Design tokens (from the Dentory design system)

| Token | Value | Use |
|---|---|---|
| sky-100 | `#d6ebfc` | page ground |
| sky-200 | `#b0defc` | decorative blobs |
| surface | `#ffffff` | cards, bubbles, contact rows |
| lilac-100 | `#e2e2fc` | icon discs, hint bar |
| pink-100 | `#f8d0e4` | speech bubble (LINE box) |
| ink | `#0b1677` | ALL text (never black) |
| ink-muted | `#4a5394` | secondary text |
| pink-500 | `#fc5490` | big CTAs (text ≥24px only), hearts, step 2 |
| pink-400 | `#fc66a2` | decorative |
| pink-600 | `#d42f70` | small pink buttons/text, errors |
| blue-300 | `#56aafc` | step 1 badge (white numeral ≥40px) |
| blue-600 | `#0b5ffc` | links, contact text & icon discs |
| violet-500 | `#7a30f8` | outline icons, step 3 |
| violet-400 | `#9a40f8` | rainbow headline stop |
| title-coral / title-blue | `#f86048` / `#40a8f8` | rainbow headline stops |
| line-green | `#06c755` | LINE buttons only (label 24px bold) |
| sparkle gold | `#fed34b` | four-point sparkles, low-stock badge |

- Fonts (Google Fonts): **Kanit** 600–800 for headings/buttons/numerals, **Prompt** 400 for body.
- Radius: cards 28px, small cards 16px, buttons/inputs/chips `9999px`.
- Shadows: cards `0 8px 24px rgba(43,123,248,.14)`; pink CTA `0 8px 20px rgba(252,84,144,.32)`. No grey shadows, no card borders.
- Icons: outline, 2px stroke, round caps (inline SVG). No emoji.
- Logo: use the real `logo.png` (do not redraw).

## Screens

1. **Splash** — logo; tilted pink pill "Explore" + "Braces Colours"; rainbow headline **สียางจัดฟัน** (60px Kanit 800, gradient coral→pink→violet→blue, thick white outline via a stacked white `-webkit-text-stroke` copy behind it); "เลือกสีที่ใช่...ในสไตล์คุณ ♡"; white speech bubble; "แค่ **2** ขั้นตอน" with two step tiles (blue 1 / pink 2); three white pill contact rows (โทร 081-453-6424, LINE @dentory, Facebook); QR card (`qr.svg`); sticky bottom pink CTA "เริ่มเลือกสียาง →".
2. **Pick** — top bar (small logo + title + "พนักงาน" lock pill); 3-step indicator; mouth card; "Pick your vibe ♡" + sticker "108 สี · เลือกได้ 2 สี"; lilac hint bar with the held-colour dot; horizontally scrolling category chips (active = pink-600 fill); swatch grid; chart-photo toggle; "บอกเราหน่อยว่าเป็นใคร" form card (pill inputs, `#f4f9fe` fill, 2px `#d6ebfc` border); sticky bottom bar = white pill with used-colour dots + "1/2 สี" + pink "ยืนยันเลย" (opacity .45 when not ready).
3. **Confirm** — pink check badge with sparkles, "เลือกสีเรียบร้อย!", read-only mouth, summary card (ring dot + #number + name + tooth count, appointment date), pink bubble with green LINE button (prefilled oaMessage text, same as today), "เลือกใหม่" / "หน้าแรก".
4. **Staff** (PIN 5555, overlay dialog) — LINE notice card + "ปรับสี ชื่อ หมวด และสต็อก" row → colour editor: edit card (big swatch, colour picker + hex, name, category select + custom, stock) and the swatch grid.

## Swatch style (both customer and staff grids) — from the user's chart image

White card with thin lilac rules (`#c9c3f0`) above and below, **4 columns**. Each cell: zero-padded number on top (`01`, 11px, letter-spaced, `#8a90b8`), an **elastic-band illustration** (SVG 66×32: an O-ring circle r=8 stroke 6 in the colour with a grey centre dot, a tail line to the upper-right at 75% opacity, three small dots under the tail at 45%, a faint navy under-stroke so pale colours stay visible, and a thin grey baseline), then the Thai name (Kanit 600, 11px, letter-spaced). Selected = `#fff0f6` fill + 2px pink ring. Locked = 30% opacity. Count badge (pink-600) top-right, low-stock badge (gold) top-left, out-of-stock = white veil + "หมดชั่วคราว". Colours are unchanged (the 108 hex values in the code).

## Teeth (mouth card) — final version after feedback

- 8 teeth per arch, **straight and evenly aligned** (no smile curve, no tilt), each 35×44 (upper) / 35×42 (lower).
- Tooth = soft rounded square, enamel gradient `#fafafc → #f1f1f5 → #dfe0e7`, side shading, small white gloss at the top, soft drop shadow.
- **Pink gums**: arch background gradient `#e9779f → #f39bbd → #f8d0e4 → #fde9f2` (upper gum at the top, lower gum at the bottom), plus a small scalloped gum cap over each tooth root.
- Level steel wire (3px, grey gradient) across the middle; metal bracket 15×14 (rounded, grey gradient, dark slot line); the elastic is a coloured rounded-square ring (24×22, 5px border) around the bracket; tooth number badge (navy pill) bottom-right.
- Interactions unchanged: tap colour → tap tooth; tap a filled tooth (no colour held) to clear; "ใส่สีเบอร์ X ทั้งปากเลย (16 ซี่)"; "ล้างทั้งหมด". Max 2 colours.

## Keep from the current app (do not drop)

Drag-and-drop colour onto tooth (the design prototype only shows tap), localStorage overrides for colour/name/category/stock, stock decrement on submit, low/out-of-stock logic, 45-second idle reset to splash, LINE share text format, view transitions, `prefers-reduced-motion`. Dark-mode toggle: optional — if kept, derive a dark palette from the tokens above.

## Files in this folder

- `reference/Main.dc.html` — the full design prototype (all screens, styles and logic; `{{…}}` holes are filled from `renderVals()` at the bottom).
- `assets/logo.png`, `assets/chart-ref.jpg` (same as in the app), `assets/qr.svg` (QR recoloured to ink navy).
- `reference/tokens.json` — Dentory design-system tokens.
