---
name: docs-page
description: Generate a three-column technical documentation page (sidebar nav + article body + right TOC) as a single-file HTML. Use when the user wants to create API docs, tutorial pages, engineering guides, or long-form reference documentation. Triggers: "技术文档", "API文档", "docs page", "文档页", "教程页", "guide page".
---

# Docs Page

Generate a single-file HTML documentation page with a three-column layout optimized for long-form technical reading. The output must be a complete, self-contained `.html` file that can be opened directly in a browser.

## Design System

**Theme: docs-page (warm neutral)**

### Colors
- Background: `#fafaf9` (warm off-white)
- Foreground: `#1c1b1a` (near-black warm)
- Muted: `#6b6964` (warm gray)
- Border: `#e6e4e0` (warm border)
- Accent: `#c96442` (burnt orange terracotta)
- Surface: `#ffffff` (white cards)
- Code background: `#f4f4f2` (warm light gray)

### Fonts
- Body: `-apple-system, system-ui, sans-serif` (system stack)
- Code: `ui-monospace, monospace`
- CJK note: append `'Noto Sans SC'` and `'Noto Serif SC'` to system stacks for Chinese content

### Layout Structure
Three-column grid at 1440px desktop:
```
grid-template-columns: 240px minmax(0, 1fr) 220px
```

Responsive breakpoints:
- ≤ 1024px: hide right TOC → `220px 1fr`
- ≤ 720px: hide left sidebar → `1fr`

### Component Specifications

**Topbar** (`header.topbar`)
- Background: `var(--surface)`, border-bottom `1px solid var(--border)`
- Contains: brand name (left) + search input (right)
- Padding: `12px 28px`

**Sidebar** (`nav.sidebar`)
- Sticky, overflow-y: auto
- Groups with uppercase `group-label` (11px, muted color)
- Links: `5px 8px` padding, `6px` radius, hover → white bg
- Active link: `var(--accent)` bg, white text

**Article** (`article`)
- Padding: `40px 56px 80px`
- Max-width: `760px`
- Crumb trail: 13px, muted color
- h1: 36px, tight letter-spacing
- lede paragraph: 17px, muted color
- h2: 22px, margin-top `40px`
- h3: 16px, margin-top `24px`
- Regular paragraphs: 15px/1.6, margin `12px 0`

**Code Blocks** (`pre`)
- Background: `var(--code-bg)`, border `1px solid var(--border)`
- Radius: `8px`, padding: `14px 16px`
- Font-size: `13px`, line-height: `1.55`
- Inline code: `1px 5px` padding, `4px` radius, `0.9em` size

**Callouts** (`div.callout`)
- Background: `var(--surface)`, border `1px solid var(--border)`
- Left accent stripe: `3px solid var(--accent)`
- Radius: `8px`, padding: `14px 18px`
- Label: 11px uppercase, accent color
- Three variants by adding classes: `.callout-info` (blue accent), `.callout-warn` (yellow accent), `.callout-danger` (red accent)

**TOC** (`aside.toc`)
- Sticky, border-left `1px solid var(--border)`
- Label: 11px uppercase "On this page"
- Links: muted color, active → accent color + bold
- Padding: `40px 24px 24px`

**Pager** (prev/next navigation)
- Flex row, gap `12px`, border-top separator
- Each link: surface bg, border, `8px` radius
- Small label: 11px uppercase muted

### Typography Scale
- h1: 36px, -0.02em tracking
- h2: 22px, -0.01em tracking
- h3: 16px
- Body: 15px/1.6
- Small/muted: 13px
- Label/caption: 11px, uppercase, 0.06em tracking
- Code: 13px (blocks), 0.9em (inline)

### CSS Custom Properties (must include)
```css
:root {
  --bg: #fafaf9; --fg: #1c1b1a; --muted: #6b6964; --border: #e6e4e0;
  --accent: #c96442; --surface: #ffffff; --code-bg: #f4f4f2;
}
```

## Hard Constraints

1. **Single-file HTML** — all CSS inline, no external JS dependencies
2. **Must use real data** — fill with the user's actual documentation content, not placeholder lorem ipsum
3. **CJK-first** — append `'Noto Sans SC'` / `'Noto Serif SC'` to font stacks when content contains Chinese
4. **Contrast ≥ 4.5** — `#1c1b1a` on `#fafaf9` passes; never use pure black on white
5. **No soft shadows** — only borders (`1px solid var(--border)`) and the callout left stripe
6. **Three-column layout** — sidebar + article + TOC must be present (TOC can be minimal if content is short)
7. **Responsive** — must include the 1024px and 720px breakpoints

## Reference

See `example.html` in this skill directory for a complete working example. Study its three-column grid, sidebar nav structure, code block styling, callout variants, and TOC pattern before generating output.