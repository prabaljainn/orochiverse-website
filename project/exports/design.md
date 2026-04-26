# Orochiverse — Design System

> *Eight heads. One body. One core.*
> A technology consultancy turning ideas into systems that scale — AI, ML, IoT, and architecture.

---

## 1. Brand essence

| | |
|---|---|
| **Name origin** | *Orochi* — the eight-headed serpent of Japanese myth. *Verse* — a unified universe. The brand metaphor: many streams of data, intelligence, and infrastructure converging into one coherent system. |
| **Voice** | Precise, calm, technical. Confident without bravado. Editorial cadence over marketing copy. |
| **Tagline** | *We deliver products that actually scale.* |
| **Positioning** | Specialized consultants in AI, ML, IoT, and system architecture. We embed with your team, design the spine, and ship. |

---

## 2. Color tokens

The system runs cool — paper-cool background, near-black ink, and a saturated electric blue as the single accent.

| Token | Hex | Role |
|---|---|---|
| `--paper` | `#eef1f6` | Page background |
| `--paper-2` | `#e6eaf2` | Subtle surface |
| `--ink` | `#0a0e1a` | Primary text, dark sections |
| `--ink-2` | `#1c2030` | Secondary text |
| `--ink-3` | `#5b6275` | Muted labels, mono |
| `--rule` | `#d9dde6` | Hairlines, dividers |
| `--accent` | `#4f8cff` | Primary accent (the fang, the core, links) |
| `--accent-2` | `#9bc1ff` | Soft accent (footer dividers, hover hints) |

**Usage rule.** One accent per surface. The accent earns its weight by appearing rarely — the chevron in the mark, the converging core in the hero, the Contact button. Resist tinting backgrounds with it.

---

## 3. Typography

| Role | Family | Weight | Usage |
|---|---|---|---|
| Display | **Space Grotesk** | 300 / 400 / 500 / 600 / 700 | Headlines, wordmark, body |
| Mono | **JetBrains Mono** | 400 / 500 | Labels, eyebrows, technical metadata |

**Type scale (clamp, fluid).**
- Hero headline: `clamp(40px, 9vw, 132px)` — line-height `.94`
- Section H2: `clamp(36px, 5.5vw, 80px)` — line-height `.95`, weight 500
- Belief quote: `clamp(32px, 4.2vw, 60px)` — line-height `1.1`, weight 400
- Body: 15–17px / 1.55
- Mono labels: 11–12px / `letter-spacing: .08em` / uppercase

**Headline rule.** Use one weight + one *italic accent* per headline. The accent word is italic-400, color = `--accent`. The "soft" support phrase is `--ink-2` at weight 300.

---

## 4. The mark — *The Fang*

A circular orbit (the universe), a blue chevron biting inward (the fang / convergence), and a single ink stem beside it (one body holding the eight heads).

**Construction.**
- viewBox `0 0 32 32`
- Circle: `cx=16 cy=16 r=13`, stroke 1.6
- Fang: path `M10 11 L 18 16 L 10 21`, accent stroke 2, round caps + joins
- Stem: line `x1=20 y1=11 → x2=20 y2=21`, ink stroke 1.6, round caps

**Hover behavior.** Chevron slides right + fades; stem collapses top→down (~80ms stagger). Reverses on mouse-out.

**Files.** See `/exports/logo/` — SVG (light + dark + mono), PNG @ 256/512/1024, ICO favicon.

**Clear space.** Minimum padding around the mark = `r/2` (50% of the orbit radius).
**Minimum size.** 16×16 px for digital favicon; 24×24 px for nav use.

---

## 5. Wordmark

`orochi · verse` set in Space Grotesk **600** at `letter-spacing: -.015em`. The center dot (`·`) is colored `--accent` — the only color emphasis in the wordmark.

**Lockup.** Mark + 12px gap + wordmark, vertically centered. Mark height = wordmark cap height × ~1.7.

---

## 6. Layout & spacing

- **Shell**: max-width 1440px, side padding `48px / 24px / 16px` at breakpoints `≥ 980 / 780 / 520 px`.
- **Section rhythm**: `120px` vertical between sections desktop, `72–80px` mobile.
- **Grid**: 12-col implicit; capability cards on a 3-up grid collapsing to 1-up at 780px.
- **Hairlines**: 1px `--rule` only — never 2px borders, never shadows on cards.

---

## 7. Components

### Buttons

**Primary CTA (Contact pill).** Two-zone capsule: text label on the left, 26px circular icon capsule on the right with an arrow. Default = `--accent` background, white text. Hover = ink color sweeps in from the left over 550ms (cubic-bezier `.7,0,.2,1`); icon capsule lifts +1px; arrow nudges 2px right; shadow shifts from accent-tinted to ink-tinted.

**Hero CTA.** Three-zone structured pill: `§ Begin` mono tag, hairline divider, "Start a project" primary action, arrow capsule.

### Capability card
Rule-top, mono index ("01"), title 24–28px, body 15px / `--ink-2`, tags as 11.5px / 500 chips with `--paper-2` ground. Hover lifts the card and tints the chips.

### Form fields
Borderless inputs with a 1px bottom rule that lights to `--accent-2` on focus. Mono uppercase labels.

---

## 8. Motion principles

- Easing: prefer `cubic-bezier(.2,.7,.2,1)` for entrances, `cubic-bezier(.7,0,.2,1)` for cinematic sweeps.
- Durations: 250–350ms for micro-interactions, 500–600ms for cinematic transitions.
- Reveals: IntersectionObserver fade+rise (16px), with a 1.2s safety net to guarantee paint on slow devices.
- Hero canvas: eight strands flowing left→right into a glowing core on the right side. Particle density adjustable via Tweaks.

---

## 9. Tone of voice

- **Do**: short clauses. Verbs first. Specific numbers. Honest constraints.
- **Don't**: superlatives ("revolutionary", "world-class"), AI-shaped buzzwords ("synergy", "harness"), exclamation marks, emoji.
- **Headline pattern**: declarative + italic emphasis word + soft qualifier. *We deliver* **products** *that actually scale*.

---

## 10. File map

```
Orochiverse.html              ← live landing page (single file + tweaks-panel.jsx)
Orochiverse Standalone.html   ← bundled, fully offline single-file build
tweaks-panel.jsx              ← in-page Tweaks panel
exports/
  ├── design.md
  ├── logo/
  │    ├── orochiverse-mark.svg            (light)
  │    ├── orochiverse-mark-dark.svg       (dark)
  │    ├── orochiverse-mark-mono.svg       (1-color)
  │    ├── orochiverse-wordmark.svg        (lockup, light)
  │    ├── orochiverse-wordmark-dark.svg   (lockup, dark)
  │    ├── orochiverse-mark-256.png
  │    ├── orochiverse-mark-512.png
  │    ├── orochiverse-mark-1024.png
  │    ├── orochiverse-wordmark-1024.png
  │    └── favicon.png   (32×32)
```

---

*Last updated: April 2026.*
