---
name: frontend-craft
description: Apply design-engineering craft at the code level when building or reviewing web interfaces. Use when implementing interactions, animations, microinteractions, or motion, deciding which modern CSS is production-safe (container queries, :has, view transitions, scroll-driven animations), choosing component architecture (Radix/Base UI/shadcn), or making an interface feel premium like Linear/Vercel/Raycast. Complements impeccable-design (visual theory) with the implementation details and anti-slop enforcement.
---

# frontend-craft

The code-level layer of world-class web work: the invisible interaction details, motion engineering, and modern-CSS judgment that separate Linear/Vercel-grade interfaces from average ones. Use alongside `impeccable-design` (visual and typographic theory).

**Companion skills:** `impeccable-design` (visual theory), `emil-design-eng` (motion, from Emil Kowalski).

## When to use
- Implementing interactions, animations, motion, microinteractions.
- "Make this feel premium / polished / like Linear."
- Deciding if a modern CSS feature is production-safe.
- Choosing component primitives/architecture.
- Reviewing frontend code for craft (not just visuals).

## The non-negotiable interaction checklist
Apply on every build:
- Focus rings via **box-shadow, not outline** (follows radius); visible on everything.
- **Press feedback** on every pressable element: `:active { transform: scale(0.97) }`, ~160ms ease-out.
- Inputs: `font-size: 16px` min (no iOS zoom), correct `type`, label-click focuses, Enter submits, **disable submit after click**.
- Gate hover behind `@media (hover: hover) and (pointer: fine)`; targets ≥ 24×24px.
- **Never change font-weight on hover** (reflow); `user-select: none` on interactive inner content; `pointer-events: none` on decorative glows.
- Feedback appears relative to its trigger; empty states prompt the next action.

## Motion engineering → defer to `emil-design-eng`
For anything beyond the basics below, use the **`emil-design-eng`** skill (the deep motion/feel authority, vendored from Emil Kowalski) and **`review-animations`** (manual-invoke adversarial motion audit, "approval is earned"). This section is just the must-know summary so you don't ship obviously-wrong motion:

- **Frequency gate (decide FIRST):** 100+/day or **keyboard-initiated** actions (command palette, shortcuts) → **never animate** (Raycast opens instantly). Tens/day → minimal. Occasional → standard. Rare/first-time → delight.
- **Every animation answers "why does this move?"** (state/feedback/spatial/explain). Else delete it.
- UI animations **under 300ms**: press 100-160, dropdown 150-250, modal 200-500. **Asymmetric:** slow where the user decides, snap where the system responds.
- Easing: out `cubic-bezier(0.23,1,0.32,1)`; in-out `cubic-bezier(0.77,0,0.175,1)`; drawer `cubic-bezier(0.32,0.72,0,1)`. **Never `ease-in` for UI; never `transition: all`.**
- **Never `scale(0)`** → `scale(0.95)+opacity:0`. **Springs** (`{duration:0.5, bounce:0.2}`, range 0.1-0.3) only for drag/gesture (keep velocity on interrupt). Velocity-dismiss > ~0.11. Stagger 30-80ms. Blur-mask < 20px. GPU-only (`transform`/`opacity`).
- `prefers-reduced-motion` = fewer/gentler, not zero. Then **run `review-animations`** on any non-trivial motion.

## Modern CSS: ship-readiness
- **Use freely:** container queries, `@layer`, nesting, OKLCH + `color-mix()`, subgrid, `text-wrap: balance/pretty`.
- **Sparingly:** `:has()` (perf footgun if broad).
- **Progressive-enhance behind `@supports`:** View Transitions (cross-doc still Chromium-only), scroll-driven animations, `@starting-style`.
- **Not ready, verify first:** anchor positioning (use Floating UI), Chrome-only bleeding edge.

## Component architecture
Primitive (behavior+a11y) → styled copy-paste → app. Prefer **Base UI** (more actively maintained than Radix since 2025) or shadcn/ui (own-the-code). Compound components over prop-bags. Scale popovers from their trigger via `transform-origin`. RSC: draw client boundaries, only mark interactive components `"use client"`.

## Anti-slop enforcement (the differentiator)
Commit to a POV palette (OKLCH), a typeface that isn't a banned default, repeat one layout primitive, and pass the two-order slop test (see `impeccable-design`). **After building, run a deterministic anti-slop filter** such as `pbakaus/impeccable`'s detector (reflex-reject fonts, cream/beige ban, gradient-text, em-dash, countable checks), then do a vision-based review pass.
