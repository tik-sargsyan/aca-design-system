---
name: design-tokens
description: Generate and manage a design-token system using the W3C DTCG 2025.10 standard. Use when creating or theming a color palette, building an OKLCH color scale, setting up design tokens, exporting tokens to CSS variables / Tailwind v4 / JS / DESIGN.md, adding dark mode or multi-brand theming, or establishing a 3-tier (primitive → semantic → component) token architecture. Outputs framework-agnostic CSS custom properties plus adapters.
---

# design-tokens

A runnable DTCG 2025.10 token system: brand input → OKLCH palette → 3-tier tokens → CSS vars + Tailwind v4 + JS + DESIGN.md. Framework-agnostic core with adapters.


## When to use
- "Generate a color palette / OKLCH scale from this brand color."
- "Set up design tokens / a theming system / dark mode / multi-brand."
- "Export tokens to CSS / Tailwind / JS."

## The pipeline

From a brand seed color, generate a 12-step OKLCH scale (hold chroma and hue, vary lightness, taper chroma at the extremes), printing WCAG contrast per step so you can verify AA before shipping. Then build 3-tier tokens (primitive, semantic, component) and export to CSS custom properties, Tailwind v4 (via `@theme inline`), and JS. Claude can generate all of this inline, so no separate tool is required.

## The architecture (don't deviate)
1. **Primitive** (`color.brand.9`, `space.4`) — raw OKLCH/dimension values, the only literals.
2. **Semantic** (`--action`, `--text`, `--surface`) — aliases to primitives. **This is the theming layer**: swap it (or its `.dark` overrides) and every component follows.
3. **Component** (`--button-bg`) — aliases to semantic.

To customize: change the seed hues and chroma and the semantic map, then regenerate. Never hard-code primitive steps in components; alias the semantic tier.

## Rules
- **OKLCH for all color.** Hold C+H, vary L across the ramp; chroma tapers at the extremes. Generate the ramp so it prints WCAG contrast, so you can verify AA before shipping.
- **Solid-fill steps (9/10) must pass 4.5:1 with white text; colored TEXT uses step 11** (the accessible text step), never step 9. The build emits `--success-text` / `--danger-text` for exactly this.
- **Verify contrast.** Fail the build if text/bg drops below AA. OKLCH predicts contrast; it does not replace the check.
- **Tailwind v4:** bridge the semantic runtime vars via `@theme inline`.
