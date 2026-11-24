# "Show it on TRMNL" Button Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a third hero button, "Show it on TRMNL", that links to the PortWatch VI TRMNL recipe.

**Architecture:** This is a static-page change. One `<a>` goes into `.hero-ctas` in `index.html`, with TRMNL's official white glyph inlined as an SVG. It gets one i18n key in each of the three language files and one CSS class that joins the existing shared pill rules.

**Tech Stack:** Plain HTML, CSS, JSON i18n and Node ≥20 (for the check only). There's no test framework and none gets added.

**Spec:** `docs/superpowers/specs/2026-11-24-trmnl-button-design.md`

## Global Constraints

- Link: `https://trmnl.com/recipes/portwatch-vi-551061`, with `target="_blank" rel="noopener"`.
- Class: `show-trmnl`. i18n key: `hero.showTrmnl`.
- Copy: en "Show it on TRMNL", es "Muéstralo en TRMNL", pt-BR "Mostre no TRMNL". "TRMNL" is never translated.
- Glyph: TRMNL's official `trmnl-glyph--white.svg` from https://trmnl.com/brand/trmnl-glyphs.zip, inlined, not redrawn.
- Hover accent: `#F8654B` (TRMNL brand orange).
- The X and GitHub buttons stay unchanged.

## Review Focus

- A narrow mobile viewport (≈360px): three pills must wrap onto two lines without overflowing or clipping.
- The es and pt-BR languages: the label must switch language, and a missing key would silently fall back to English.
- Keyboard focus: the new pill must get the same visible focus ring as the other two.
- The glyph's visual size must match the X and GitHub icons and not look oversized in the 30px slot.
- With reduced motion or on touch, hover lift doesn't apply, but the `:active` reset must still work.

---

### Task 1: Show it on TRMNL button

**Files:**
- Modify: `index.html:149` (after the closing `</a>` of `.star-gh`, inside `.hero-ctas`)
- Modify: `assets/css/style.css:216-231` (shared pill rules plus the new `.show-trmnl` rules)
- Modify: `assets/i18n/en.json:34`, `assets/i18n/es.json:34`, `assets/i18n/pt-BR.json:34` (add `hero.showTrmnl` after `hero.starGh`)

**Interfaces:**
- Consumes: the existing `data-i18n` mechanism (`assets/js/i18n.js`) and the existing `--grad` and `--gold` variables (unchanged).
- Produces: nothing used by other tasks.

- [ ] **Step 1: Write the check and run it to see it fail**

Run:
```bash
node -e '
const fs=require("fs");const h=fs.readFileSync("index.html","utf8");const c=fs.readFileSync("assets/css/style.css","utf8");
const want={en:"Show it on TRMNL",es:"Muéstralo en TRMNL","pt-BR":"Mostre no TRMNL"};
for(const[l,v]of Object.entries(want)){const j=JSON.parse(fs.readFileSync(`assets/i18n/${l}.json`,"utf8"));if(j["hero.showTrmnl"]!==v)throw l+" key"}
if(!/<a class="show-trmnl" href="https:\/\/trmnl\.com\/recipes\/portwatch-vi-551061" target="_blank" rel="noopener">/.test(h))throw"markup";
if(!h.includes(`data-i18n="hero.showTrmnl">Show it on TRMNL</span>`))throw"label";
for(const s of[".show-trmnl:hover",".show-trmnl:focus-visible",".show-trmnl svg"])if(!c.includes(s))throw"css "+s;
if(!c.includes("#F8654B")&&!/248,\s*101,\s*75/.test(c))throw"accent";
console.log("OK")'
```
Expected: it fails with `en key`.

- [ ] **Step 2: Add the i18n keys**

Add `"hero.showTrmnl"` right after `"hero.starGh"` in all three files, with the values from Global Constraints.

- [ ] **Step 3: Add the markup**

After the `.star-gh` `</a>`, add an `<a class="show-trmnl" …>` with the exact attributes from Global Constraints. Inside it:
- the inlined glyph: the `viewBox="0 0 160 162"`, the seven `<path>`s with `fill="white"` removed so CSS controls the fill, and `aria-hidden="true"`;
- `<span data-i18n="hero.showTrmnl">Show it on TRMNL</span>`.

- [ ] **Step 4: Add the CSS**

Add `.show-trmnl` to every shared selector list at lines 216, 217, 225, 226, 227 and 230, and to `.show-trmnl svg` in the line-231 mobile rule. Its own rules mirror `.star-gh`: `gap: 9px; padding-right: 15px; border-color: rgba(255,255,255,.4); background: rgba(13,17,23,.72)`, `svg { fill: #fff; padding: 4px }` (the padding is the builder's call for visual size), and on `:hover` `border-color: #F8654B; box-shadow: 0 6px 22px rgba(248,101,75,.4)`.

- [ ] **Step 5: Run the check again**

Run the Step 1 command. Expected: `OK`.

- [ ] **Step 6: Check it in a browser**

Serve the site (`python3 -m http.server 8000`) and open `http://localhost:8000/`:
- the three pills look alike;
- the TRMNL hover turns orange and its focus ring shows;
- the link opens the recipe in a new tab;
- at 360px wide the pills wrap cleanly;
- switching to es and pt-BR changes the label.

- [ ] **Step 7: Commit**

```bash
git add index.html assets/css/style.css assets/i18n/en.json assets/i18n/es.json assets/i18n/pt-BR.json
git commit -m 'Hero: "Show it on TRMNL" button linking to the TRMNL recipe' -m 'Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>'
```
