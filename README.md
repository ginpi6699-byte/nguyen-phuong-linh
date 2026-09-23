# Handoff: Love Letter v2 (Y2K Soft-Pink Anniversary Page)

## Overview
A single-page, scrolling anniversary experience: infinite memory-photo carousel, glassmorphism music player with lyrics, two icon-only action buttons (love-text marquee + falling flower/heart particles), a large floating envelope that opens an anniversary letter, and a "Hành trình của chúng mình" (our timeline) milestones card. Left edge (desktop, ≥1024px) shows a floating vinyl-record/polaroid graphic pinned to the top-left with no background.

## About the Design File
`Love Letter v2.dc.html` is a **design reference built in HTML** (component logic + inline styles, streaming-render architecture) — a working prototype of the final look/behavior, not code to paste verbatim. Recreate it in your target codebase's stack (React, Vue, etc.), using its own component and styling conventions.

## Fidelity
**High-fidelity.** Colors, spacing, typography, image treatment and animation timings shown are final.

## Sections (top to bottom)

### 0. Global background & left column
- Fixed full-viewport background: `bg-lily.jpg`, cover, centered, fixed, no-repeat, base color `#fbeaf0` behind it.
- Desktop only (≥1024px): a vinyl-record/polaroid collage image (`vinyl-polaroids-transparent.png`, pre-cut to remove its white background) pinned `position:absolute; top:0; left:0`, width 350px (450px ≥1280px), rendered at 120% width, shifted -48px left / -20px up, `mix-blend-mode:multiply`, drop-shadow, scale 1.05 on hover. No wrapper background/border/padding — floats directly on the page background.

### 1. Memory Carousel
- Glassmorphism strip (`rgba(255,255,255,.4)` + `blur(18px)`, 1px white border, `border-radius:28px`), max-width 520px.
- Inner row of 5 photos (each looped twice for a seamless loop) auto-scrolling left via CSS `translateX(0)→translateX(-50%)`, 22s linear infinite.
- Each photo: 150×190px, `object-fit:cover`, `border-radius:24px`, shadow.
- Photos (in order): `memory1.jpg` … `memory5.jpg` (props `photo1..photo5`, overridable).

### 2. Music Player + Lyrics
- Same glassmorphism card style, padding 28px.
- Row: 80×80px album art (`album-cover.jpg`, rounded 20px) + song title "Mãi mãi bên nhau" / subtitle "Nhạc nền của chúng mình" (warm pink tones `#7a4a52` / `#a9838a`).
- Centered circular play/pause button (52px, outlined, toggles ▶/⏸ — no actual audio wired).
- Below: 4 centered italic lyric lines, each with a thin bottom divider, color `#8a5b62`.

### 3. Action Buttons (icon-only)
- Two glass-pill buttons, centered, `gap:24px`, each `rgba(255,255,255,.4)` blur pill containing only a 40×40px glossy heart image (`glass-heart.png`), no text.
- **Left button** ("loveBurst"): triggers BOTH — (a) a single-pass marquee: white italic serif text "I love you more than I can say" with white glow, sweeps right→left across vertical mid-screen over 12s, then auto-hides; (b) the falling-particle effect below.
- **Right button** ("triggerParticles"): falling-particle effect only.
- **Particle effect**: ~45 particles, each randomly one of `particle-1.png` / `particle-2.png` / `particle-3.png` / `particle-4.png` / `glass-heart.png`, random x (0–100vw), random size 80–160px, random fall duration 4–8s (linear, staggered 0–2.5s delay) from `top:-10%` falling off-screen, plus an independent continuous side-to-side sway (2.5–5s ease-in-out loop). Auto-clears after 11s. `pointer-events:none`, very high z-index, never blocks clicks.

### 4. Envelope → Letter Trigger
- Large floating envelope image (`envelope-lover.jpg`), no card/background — sits directly on the page background, `width:90vw` capped at 450px (550px ≥768px), drop-shadow, scales 1.05 on hover, clickable (opens the letter).
- Below it, a 3D pink pill button image (`pill-button.png`, pre-cut of its background/whitespace) at 220px wide (260px ≥768px), with the label "anh mở thư nhé!" overlaid in deep pink (`#c23b65`), centered on top of the image. Also opens the letter. Both share one toggle.

### 5. The Letter (conditionally rendered, hidden until opened)
- Cream card `#FDFBF7`, `border-radius:28px`, padding 32px, max-width 520px — appears directly below the envelope once opened; stays open (no re-collapse).
- Salutation "Gửi tình yêu của em…" — bold italic, centered, ~28px.
- Full Vietnamese letter body follows (preserve verbatim — see file).

### 6. Timeline — "Hành trình của chúng mình" (always visible, separate card)
- Its own cream card (`#FDFBF7`, same radius/shadow/padding as the letter card), rendered below the letter section, NOT gated by the letter's open/closed state.
- 7 milestone entries, each with a date, title, and description (see file for exact Vietnamese copy — preserve verbatim).

## State
- `letterOpen` (bool) — set true by clicking the envelope or the pill button; only controls the letter card's visibility.
- `playing` (bool) — toggles play/pause icon on the music player (cosmetic only, no real audio).
- `isRaining` / `particles` (array) — drives the falling-particle overlay; auto-clears after 11s.
- `showLoveText` (bool) — drives the one-pass marquee text; set true by the left action button, set false automatically when the CSS animation ends (`animationend`).
- `vw` (number, from a window resize listener) — used to switch the vinyl column's visibility (≥1024px only) and its width breakpoint (350px / 450px at ≥1280px), and the envelope/pill max-widths at the 768px breakpoint.

## Design Tokens
- Background: `bg-lily.jpg` (fixed/cover), fallback `#fbeaf0`.
- Glass surfaces: `rgba(255,255,255,.4)`–`.45`, `backdrop-filter: blur(18px)`/`blur(12px)`, `1px solid rgba(255,255,255,.5)`–`.6`, radius 28px, shadow `0 20px 45px rgba(214,150,170,.25)`.
- Letter/timeline cards: solid `#FDFBF7`, radius 28px, shadow `0 20px 45px rgba(214,150,170,.22)`.
- Text colors: body `#44403c`; headings/salutation `#57534e`; player title `#7a4a52`; player subtitle `#a9838a`; lyrics `#8a5b62`; pill-button label `#c23b65`.
- Typography: `'Cormorant Garamond', serif` throughout (weights 400/500/600/700, italics).
- Animations: carousel scroll 22s linear infinite; marquee 12s linear once; particle fall 4–8s linear; particle sway 2.5–5s ease-in-out infinite.

## Assets (in `assets/`)
- `bg-lily.jpg` — global background
- `vinyl-polaroids.png` — original vinyl/polaroid graphic; `vinyl-polaroids-transparent.png` — white-background-removed, trimmed version actually used
- `memory1.jpg` … `memory5.jpg` — carousel photos
- `album-cover.jpg` — music player thumbnail
- `glass-heart.png` — icon used in both action buttons and as a particle
- `particle-1.png` … `particle-4.png` — falling particle images
- `envelope-lover.jpg` — envelope trigger image
- `pill-button.png` — 3D pill button graphic (background/whitespace removed)

## Files
- `Love Letter v2.dc.html` — full component (template + logic), source of truth.
- `assets/` — all image assets referenced above.
