# "Show it on TRMNL" button: design

## Intent
Add a third call-to-action in the hero, next to "Follow on X" and "Star on GitHub". It links to the PortWatch VI TRMNL recipe (https://trmnl.com/recipes/portwatch-vi-551061), so TRMNL owners can put the tracker on their e-ink display. This fork's change is meant to go upstream to 4lwayz/portwatch-vi.

## Goals / non-goals
- Goal: one button, styled like the other two pills, that opens the recipe in a new tab.
- Goal: use TRMNL's official brand assets (https://trmnl.com/brand) and its badge wording.
- Non-goal: click tracking, README changes, any change to the X or GitHub buttons.
- Anti-goal: a button that looks out of place next to the other two, or one that isn't translated.

## Approaches considered
1. **Official "Show it on TRMNL" badge SVG as-is.** This is TRMNL's exact artwork, but it's an App Store-style rectangle with the English text baked in as outlines. It wouldn't match the other two pills and couldn't be translated.
2. **Pill using the official glyph and the badge wording (chosen by the owner).** This matches the existing buttons and stays translatable, while using TRMNL's own glyph (the same one that appears in the badge) and the badge's wording.
3. **A generically drawn e-ink screen icon.** Dropped: the official assets exist and are offered "for use in your projects".

## Design
- **Markup** (`index.html`, `.hero-ctas`): `<a class="show-trmnl" href="https://trmnl.com/recipes/portwatch-vi-551061" target="_blank" rel="noopener">` after the GitHub button. It contains TRMNL's white glyph (from `trmnl-glyphs.zip`, `trmnl-glyph--white.svg`) inlined as an SVG, plus `<span data-i18n="hero.showTrmnl">Show it on TRMNL</span>`.
- **i18n**: add `hero.showTrmnl` to `en.json` ("Show it on TRMNL"), `es.json` ("Muéstralo en TRMNL") and `pt-BR.json` ("Mostre no TRMNL"). "TRMNL" stays untranslated.
- **CSS** (`assets/css/style.css`): add `.show-trmnl` to the shared pill rules for size, hover, active, focus and mobile. Its own look is a dark fill with a thin white outline, the same weight as the GitHub button. On hover the border and glow turn TRMNL's brand orange `#F8654B`, the way the GitHub button turns gold.

## Left to the builder
- How the glyph sits in the 30px icon slot: padding so it reads at the same visual size as the X and GitHub marks.
- Checking that three pills wrap cleanly on narrow screens.
