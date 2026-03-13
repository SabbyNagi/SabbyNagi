---
name: frontend-design
description: Cross-cutting aesthetic layer for ALL frontend work. Governs typography, color, motion, spatial composition, and backgrounds. Activates alongside ux-mocks, website-design, or any standalone frontend build.
---

## What This Skill Does

This is the **aesthetic quality layer** for all frontend work — the visual equivalent of `writing-style` for text. It governs HOW things look: typography, color, motion, spatial composition, and backgrounds. It activates alongside other skills (`ux-mocks`, `website-design`) or standalone when building any frontend.

**Core problem it solves:** Without direction, AI converges toward generic "AI slop" — Inter font, purple gradients, predictable layouts, cookie-cutter components. This skill breaks that pattern.

## Design Thinking (Before Code)

Before writing a single line, commit to a **bold aesthetic direction**:

1. **Purpose:** What problem does this interface solve? Who uses it?
2. **Tone:** Pick a clear direction — don't hedge. Options for inspiration:
   - Brutally minimal | Maximalist chaos | Retro-futuristic
   - Organic/natural | Luxury/refined | Playful/toy-like
   - Editorial/magazine | Brutalist/raw | Art deco/geometric
   - Soft/pastel | Industrial/utilitarian | Solarpunk
   - Dark terminal | Glassmorphic | Neo-corporate
3. **Differentiation:** What's the ONE thing someone will remember about this design?
4. **Constraints:** Framework, performance, accessibility requirements.

**State your aesthetic direction before coding.** "Going for a dark editorial feel with Playfair Display headers and warm amber accents" is infinitely better than just starting to code.

## Typography

Typography instantly signals quality. This is the #1 lever for making frontend work look professional vs. generic.

### Never Use
Inter, Roboto, Open Sans, Lato, Arial, default system fonts. These are the "Lorem ipsum" of fonts — they signal zero design thought.

### Recommended Font Families

| Vibe | Fonts |
|------|-------|
| **Code/Tech** | JetBrains Mono, Fira Code, Space Grotesk, IBM Plex Mono |
| **Editorial** | Playfair Display, Crimson Pro, Fraunces, Newsreader |
| **Startup/Modern** | Clash Display, Satoshi, Cabinet Grotesk, General Sans |
| **Technical/Clean** | IBM Plex Sans, Source Sans 3, Instrument Sans |
| **Distinctive** | Bricolage Grotesque, Obviously, Syne, DM Sans |

### Typography Rules

- **Pairing principle:** High contrast = interesting. Display + monospace, serif + geometric sans, variable font across weights.
- **Weight extremes:** Use 100/200 vs 800/900 — not 400 vs 600. Timid weight contrast looks unintentional.
- **Size jumps:** 3x+ scaling, not 1.5x. A 14px body with a 48px heading creates drama. 14px body with 21px heading creates nothing.
- **One distinctive font, used decisively.** Load from Google Fonts. Don't use 4 fonts — pick 1-2 and commit.

## Color & Theme

### Principles

- **Commit to a cohesive aesthetic.** Use CSS variables for consistency across the entire page.
- **Dominant colors with sharp accents** outperform timid, evenly-distributed palettes. 60-30-10 rule: 60% dominant, 30% secondary, 10% accent.
- **Draw from real-world inspiration:** IDE themes (Dracula, Solarized, Nord), cultural aesthetics (Japanese wabi-sabi, Scandinavian minimal), nature (deep forest, desert sunset, ocean depths).
- **Avoid the AI cliche:** Purple gradients on white backgrounds. This is the #1 signal of AI-generated design.

### Color System Template

```css
:root {
  --color-bg: #0a0a0b;
  --color-surface: #141416;
  --color-border: #2a2a2e;
  --color-text-primary: #f0f0f0;
  --color-text-secondary: #8a8a8e;
  --color-accent: #ff6b35;
  --color-accent-subtle: rgba(255, 107, 53, 0.1);
}
```

## Motion & Animation

### Principles

- **High-impact moments over scattered micro-interactions.** One well-orchestrated page load with staggered reveals creates more delight than 20 random hover effects.
- **Prioritize CSS-only solutions** for HTML. Use Motion library for React when available.
- **animation-delay is your best friend.** Stagger elements by 50-100ms for reveals.
- **Scroll-triggered and hover states that surprise** — not just opacity changes.

### Page Load Pattern (CSS-only)

```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.animate-in {
  opacity: 0;
  animation: fadeInUp 0.6s ease-out forwards;
}

.animate-in:nth-child(1) { animation-delay: 0.1s; }
.animate-in:nth-child(2) { animation-delay: 0.2s; }
.animate-in:nth-child(3) { animation-delay: 0.3s; }
```

### When NOT to Animate

- Data-heavy tables and dashboards (motion distracts from data)
- Forms during input (don't move things while users type)
- Accessibility: always respect `prefers-reduced-motion`

## Spatial Composition

- **Unexpected layouts.** Asymmetry. Overlap. Diagonal flow. Grid-breaking elements.
- **Generous negative space OR controlled density** — both work, but commit to one.
- **Don't center everything.** Left-aligned hero text with a right-bleeding image. Off-grid accents. Overlapping cards.
- **Break the grid deliberately** — one element that breaks the pattern draws the eye.

## Backgrounds & Visual Details

Create atmosphere and depth rather than defaulting to solid colors:

- **Gradient meshes:** Multi-stop radial gradients for depth
- **Noise/grain textures:** Subtle noise overlay adds tactile quality
- **Geometric patterns:** SVG-based repeating patterns for texture
- **Layered transparencies:** Overlapping semi-transparent elements
- **Dramatic shadows:** Long, colored shadows instead of generic `box-shadow`

## Anti-Patterns (Never Do These)

| Anti-Pattern | Why It's Bad | Do This Instead |
|---|---|---|
| Inter/Roboto font | Screams "no design thought" | Pick a distinctive font from the table above |
| Purple gradient on white | The #1 AI cliche | Commit to a real color palette |
| Centered everything | Boring, predictable | Asymmetric layouts, left-aligned heroes |
| 400 vs 600 weight | Timid, no contrast | 200 vs 900 weight extremes |
| Solid white background | Flat, lifeless | Layered gradients, subtle textures |
| Generic card grid | Every AI output looks like this | Unexpected layouts, varied component sizes |

## Implementation Checklist

Before delivering any frontend work, verify:

- [ ] Fonts loaded from Google Fonts (not system defaults)
- [ ] CSS variables defined for color system
- [ ] At least one animation (page load or interaction)
- [ ] No purple-gradient-on-white cliche
- [ ] `prefers-reduced-motion` respected
- [ ] Aesthetic direction stated before coding
- [ ] Background has depth (not solid white/black)
- [ ] Typography has weight contrast (not just 400/600)
