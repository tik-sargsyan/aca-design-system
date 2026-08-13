---
name: accessibility-audit
description: Audit a webpage or component for WCAG 2.2 AA and European Accessibility Act (EAA) compliance, with prioritized concrete fixes. Use when asked to check accessibility, run an a11y audit, fix WCAG issues, verify contrast, check keyboard navigation or screen-reader support, assess EAA / legal compliance, or evaluate APCA contrast. Combines automated axe-core scanning with a manual checklist and APCA guidance.
---

# accessibility-audit

WCAG 2.2 AA + EAA audit with prioritized fixes. Automated floor (axe-core) plus the manual checklist that covers the ~70% rules miss.

**Automated scan:** run axe-core (for example `@axe-core/playwright`) with the wcag2a, wcag2aa, wcag21aa, and wcag22aa tags.

## When to use
- "Audit accessibility / run an a11y check / fix WCAG issues."
- "Is this EAA / legally compliant?"
- "Check contrast / keyboard nav / screen-reader support."

## The target: WCAG 2.2 AA (today's legal floor)
The EAA is in force (28 June 2025); fines reach €100k+, and it applies to anyone serving EU consumers regardless of HQ. EN 301 549 is the technical standard. **Build to WCAG 2.2 AA.** WCAG 3.0 and APCA are draft/future — design with them, claim conformance only against WCAG 2.x.

## Audit procedure
1. **Automated scan** run axe-core (`@axe-core/playwright` in the project). Catches ~30% of issues; never sufficient alone.
2. **The six WCAG 2.2 AA/A deltas over 2.1** (often missed): focus not obscured (2.4.11), dragging alternatives (2.5.7), **24×24px target size** (2.5.8), consistent help (3.2.6), redundant entry (3.3.7), **accessible auth** — allow password managers + copy/paste (3.3.8).
3. **Manual checklist:**
   - **Semantic HTML first** — native `<button>`/`<a>`/landmarks/headings-in-order. "No ARIA is better than bad ARIA." ARIA adds neither keyboard behavior nor styling.
   - **Keyboard** — full task completion by keyboard alone; logical tab order; no traps; `tabindex` 0/-1 only.
   - **Focus** — visible on everything (box-shadow ring, ~3:1); move focus into dialogs, trap, restore on close; keep visible under sticky headers.
   - **Contrast** — body 4.5:1, large text/UI 3:1. Never info by color alone.
   - **Reduced motion** — honor `prefers-reduced-motion` (fewer/gentler, not zero).
   - **Screen readers** — meaningful alt (empty for decorative), accessible names, `aria-live` for dynamic updates, descriptive links.
4. **Prioritize fixes** by impact: critical (blocks a task / fails a legal SC) → serious → moderate. Give a concrete, located fix for each, not "improve accessibility."

## APCA — quality tool, not a compliance claim
Use WCAG 2's 4.5:1 / 3:1 for any audit or legal claim. Use APCA Lc as a *readability* check, especially dark mode where WCAG 2 misleads: Lc 90 fluent body, 75 body min, 60 larger, 45 headlines, 30 spot-readable. Never claim conformance via APCA.

## Output
A report: automated violations (id, impact, nodes), manual findings, and a prioritized fix list. Re-run after fixes to confirm. For palette-level contrast, generate your OKLCH ramp so it prints WCAG ratios per step.
