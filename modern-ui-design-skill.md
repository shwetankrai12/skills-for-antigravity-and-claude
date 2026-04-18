---
name: modern-ui-design
description: Comprehensive reference for modern frontend UI design covering visual foundations, design process, systems architecture, interaction design, technical best practices, accessibility, collaboration, and 2025 industry standards. Use when designing or implementing web/mobile interfaces, building design systems, or evaluating UI quality.
version: 1.0
last_updated: 2026-04
---

# The Modern UI Design Skill: From Pixel to Production

> A practitioner's reference for designers who code and developers who design. Built for 2025+ web standards.

---

## Table of Contents

1. [Core Design Principles & Visual Foundation](#1-core-design-principles--visual-foundation)
2. [The Modern Design Process](#2-the-modern-design-process-workflow)
3. [Design Systems & Component Architecture](#3-design-systems--component-architecture)
4. [Interaction Design & Motion](#4-interaction-design--motion)
5. [Frontend Technical Best Practices](#5-frontend-technical-best-practices)
6. [Accessibility & Inclusive Design](#6-accessibility-a11y--inclusive-design)
7. [Designer-Developer Collaboration](#7-the-designer-developer-collaboration-devrel)
8. [Practical Checklists & Prompts](#8-practical-checklists--prompts)
9. [Glossary of Modern Terms](#9-glossary-of-modern-terms)

---

## 1. Core Design Principles & Visual Foundation

Every great interface stands on five load-bearing pillars. Skip any of them and the UI feels off — even if users can't articulate why.

### 1.1 The Five Pillars

| Pillar | What it means | Failure mode |
|--------|---------------|--------------|
| **Visual Hierarchy** | Guide the eye through size, weight, color, and position. The most important element should be visually loudest. | Everything screams; nothing gets heard. |
| **Affordance** | Elements should *look* like what they *do*. Buttons look pressable, links look clickable, inputs look fillable. | Users hunt-and-peck; mystery meat navigation. |
| **Consistency** | Same patterns solve same problems across the entire product. Internal (within product) and external (platform conventions). | Cognitive load skyrockets; users re-learn each screen. |
| **Feedback** | Every action gets a response within 100ms. Hover, click, load, success, error — all communicated. | Users tap twice, doubt the system, lose trust. |
| **Simplicity** | Remove until it breaks, then add back the minimum. Complexity is the enemy of adoption. | Feature bloat; users overwhelmed on first contact. |

**Rule of thumb:** If you can't justify why an element exists in 5 seconds, delete it.

### 1.2 Color Theory & Accessible Palettes

#### Building a palette

A modern palette has four functional layers:

1. **Brand colors** (1-2 hues): Identity. Used sparingly for emphasis.
2. **Neutral scale** (8-12 steps from white→black): The workhorse. 80% of your UI uses these.
3. **Semantic colors** (success, warning, error, info): Meaning-bearing. Tied to status, not aesthetics.
4. **Surface colors**: Backgrounds, cards, overlays. Often near-neutral with subtle tints.

#### Contrast ratios (WCAG 2.2)

| Level | Body text (≤18pt) | Large text (≥18pt) | UI components & graphics |
|-------|-------------------|--------------------|--------------------------|
| AA    | 4.5:1             | 3:1                | 3:1                      |
| AAA   | 7:1               | 4.5:1              | —                        |

**Test tools:** Stark (Figma plugin), Polypane, browser DevTools' built-in contrast checker, [APCA](https://www.myndex.com/APCA/) (proposed WCAG 3 successor — perceptually accurate).

#### CSS variables for theming

```css
:root {
  /* Color primitives — never used directly in components */
  --slate-50: oklch(98% 0.005 250);
  --slate-900: oklch(15% 0.02 250);
  --indigo-500: oklch(62% 0.18 270);

  /* Semantic tokens — use these in components */
  --color-bg: var(--slate-50);
  --color-fg: var(--slate-900);
  --color-accent: var(--indigo-500);
  --color-border: oklch(90% 0.01 250);
}

[data-theme="dark"] {
  --color-bg: var(--slate-900);
  --color-fg: var(--slate-50);
  --color-border: oklch(25% 0.02 250);
}

/* Use the System preference as the default */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --color-bg: var(--slate-900);
    --color-fg: var(--slate-50);
  }
}
```

**Why OKLCH over HEX/HSL?** Perceptually uniform — when you bump lightness by 10%, it actually *looks* 10% lighter across all hues. HSL lies. OKLCH is supported in all modern browsers as of 2024.

### 1.3 Typography

#### Fluid type scale with `clamp()`

Stop using fixed pixel values for typography. Use `clamp(min, preferred, max)`:

```css
:root {
  /* clamp(min, fluid-value, max) */
  --text-xs:   clamp(0.75rem,  0.7rem  + 0.25vw, 0.875rem);
  --text-sm:   clamp(0.875rem, 0.8rem  + 0.375vw, 1rem);
  --text-base: clamp(1rem,     0.95rem + 0.25vw,  1.125rem);
  --text-lg:   clamp(1.125rem, 1.05rem + 0.375vw, 1.25rem);
  --text-xl:   clamp(1.25rem,  1.15rem + 0.5vw,   1.5rem);
  --text-2xl:  clamp(1.5rem,   1.3rem  + 1vw,     2rem);
  --text-3xl:  clamp(1.875rem, 1.5rem  + 1.875vw, 2.75rem);
  --text-4xl:  clamp(2.25rem,  1.75rem + 2.5vw,   3.75rem);
}
```

Use [Utopia.fyi](https://utopia.fyi) to generate scales mathematically.

#### Variable fonts

Single file, infinite weights/widths. Saves 60-80% on font payload vs. loading 4-6 separate weight files.

```css
@font-face {
  font-family: 'Inter';
  src: url('/fonts/InterVariable.woff2') format('woff2-variations');
  font-weight: 100 900;     /* range, not single value */
  font-display: swap;
  font-style: normal;
}

.heading {
  font-family: 'Inter';
  font-weight: 650; /* any value in range, not just 100/200/300... */
  font-variation-settings: 'opsz' 32; /* optical sizing */
}
```

#### Readability rules

- **Line length:** 50-75 characters per line for body. Use `max-width: 65ch`.
- **Line height:** 1.4-1.6 for body, 1.1-1.25 for headings.
- **Letter spacing:** Tighten headings (`-0.02em` to `-0.04em`), loosen all-caps (`+0.05em` to `+0.1em`).
- **Hierarchy through 2-3 sizes max** — change weight and color to differentiate further levels without adding more sizes.

### 1.4 Spacing & Layout

#### The 4pt/8pt grid

Pick one base unit (4px or 8px) and derive everything from it. This kills the "is that 13px or 14px?" decision fatigue.

```css
:root {
  --space-1: 0.25rem;  /* 4px  */
  --space-2: 0.5rem;   /* 8px  */
  --space-3: 0.75rem;  /* 12px */
  --space-4: 1rem;     /* 16px */
  --space-6: 1.5rem;   /* 24px */
  --space-8: 2rem;     /* 32px */
  --space-12: 3rem;    /* 48px */
  --space-16: 4rem;    /* 64px */
  --space-24: 6rem;    /* 96px */
}
```

#### Logical properties

Logical properties make i18n (right-to-left languages, vertical writing modes) free:

```css
/* OLD — physical, breaks in RTL */
.card {
  margin-left: 16px;
  padding-top: 24px;
  border-right: 1px solid;
}

/* NEW — logical, works everywhere */
.card {
  margin-inline-start: 16px;  /* "start" flips for RTL */
  padding-block-start: 24px;
  border-inline-end: 1px solid;
}
```

| Physical | Logical |
|----------|---------|
| `margin-left` | `margin-inline-start` |
| `margin-right` | `margin-inline-end` |
| `margin-top` | `margin-block-start` |
| `margin-bottom` | `margin-block-end` |
| `width` | `inline-size` |
| `height` | `block-size` |

#### Negative space

Whitespace is not empty — it's a tool. Generous spacing signals luxury and confidence; dense spacing signals utility and information density. Pick a stance and commit.

### 1.5 Imagery & Iconography

#### Format decisions

| Format | Use for | Notes |
|--------|---------|-------|
| **AVIF** | Photos | 50% smaller than JPEG, 20% smaller than WebP. Now universally supported. |
| **WebP** | Photos (fallback) | Older browsers. Use `<picture>` with AVIF first. |
| **SVG**  | Icons, logos, simple illustrations | Infinitely scalable, scriptable, animatable, tiny. |
| **PNG**  | When you need transparency + raster | Avoid for photos — bloated. |
| **JPEG** | Last-resort fallback | 2026 mein bhi koi bowser hai jo nahi karta WebP support? Doubt. |

```html
<picture>
  <source srcset="/img/hero.avif" type="image/avif">
  <source srcset="/img/hero.webp" type="image/webp">
  <img src="/img/hero.jpg" alt="..." loading="lazy" decoding="async" width="1200" height="630">
</picture>
```

Always declare `width` and `height` on images to prevent **CLS** (Cumulative Layout Shift).

#### Icon systems

Three approaches, ranked:

1. **SVG components** (React/Vue): Best DX, tree-shakeable, prop-driven (`<Icon size={16} color="..." />`). Use [Lucide](https://lucide.dev), [Phosphor](https://phosphoricons.com), or [Tabler](https://tabler-icons.io).
2. **SVG sprite sheet**: One HTTP request, referenced by `<use href="#icon-name">`. Good for static sites.
3. **Icon fonts** (FontAwesome): Outdated. Avoid — accessibility issues, no multi-color support, font-loading flash.

---

## 2. The Modern Design Process (Workflow)

### 2.1 Discovery & Research

#### User personas (modern take)

Old-school personas ("Sarah, 34, marketing manager, loves yoga") are mostly fiction. Replace with:

- **Behavioral personas**: Based on observed behavior, not demographics. ("The Power User who keyboard-shortcuts everything.")
- **Proto-personas**: Quick sketches when budget is zero. Validate later.

#### Jobs to be Done (JTBD)

Frame the problem as a job, not a persona:

> "When I'm onboarding a new team member, I want to share our process docs in one link, so they can ramp up without 12 Slack DMs."

This format — *When [situation], I want [motivation], so I can [outcome]* — surfaces real needs that personas miss.

#### Competitor analysis

Don't just collect screenshots. Build a **feature matrix**:

| Feature | Us | Competitor A | Competitor B | Opportunity |
|---------|----|----|----|----|
| Onboarding length | 5 steps | 2 steps | 8 steps | Cut to 3 |
| Empty states | Generic | Custom illustrations | None | Differentiate here |

### 2.2 Information Architecture

#### Sitemaps

Visual map of every screen and how they connect. Tools: FigJam, Whimsical, Miro, or just a Markdown bullet list for small projects.

#### User flows

Trace the steps for one specific task:

```
Landing → Click "Sign Up" → Enter email → Verify email → Choose plan → Onboarding (3 steps) → Dashboard
                                ↓ (error)
                                Show error inline → retry
```

Use rectangles for screens, diamonds for decisions, arrows for flow.

#### Card sorting

Hand users (or testers) a list of features and ask them to group them. Open card sort: they create categories. Closed: you provide categories. Tools: [OptimalSort](https://www.optimalworkshop.com), Maze.

### 2.3 Wireframing & Prototyping

| Fidelity | When to use | Tools |
|----------|-------------|-------|
| **Lo-fi (sketches/wireframes)** | Exploring ideas, fast iteration, stakeholder buy-in on structure | Pen+paper, Excalidraw, FigJam |
| **Mid-fi (greyscale wireframes)** | Validating layout and flow without color/visual distraction | Figma, Penpot |
| **Hi-fi (pixel-perfect mockups)** | Final design review, dev handoff, marketing | Figma, Penpot, Sketch |
| **Interactive prototype** | Usability testing, demos, animation specs | Figma prototyping, Framer, ProtoPie |

**Rule:** Don't go hi-fi until the lo-fi flow has been tested. You'll waste 10x the time polishing the wrong thing.

### 2.4 Handoff to Development

#### Figma file structure for dev-mode

```
📁 Project
  📁 0. Cover
  📁 1. Foundations (colors, type, spacing — as variables)
  📁 2. Components (named, with variants and props)
  📁 3. Patterns (composed components: forms, cards, etc.)
  📁 4. Pages
  📁 5. Prototypes
  📁 ⚰️ Archive
```

#### Design tokens JSON

Export tokens in a framework-agnostic format ([W3C Design Tokens spec](https://design-tokens.github.io/community-group/format/)):

```json
{
  "color": {
    "brand": {
      "primary": { "$value": "#5B5BD6", "$type": "color" },
      "primary-hover": { "$value": "#4A4AC4", "$type": "color" }
    }
  },
  "spacing": {
    "sm": { "$value": "8px", "$type": "dimension" },
    "md": { "$value": "16px", "$type": "dimension" }
  }
}
```

Tools like [Style Dictionary](https://amzn.github.io/style-dictionary) transform this JSON into CSS, JS, iOS, Android — single source of truth.

---

## 3. Design Systems & Component Architecture

### 3.1 Atomic Design Methodology

Brad Frost's mental model for building systems:

| Level | Examples | Reusability |
|-------|----------|-------------|
| **Atoms** | Button, Input, Label, Icon, Badge | Highest — used everywhere |
| **Molecules** | Search bar (input + button), form field (label + input + error) | High |
| **Organisms** | Navigation header, product card, comment thread | Medium — context-specific |
| **Templates** | Page layout skeletons (no real content) | Low |
| **Pages** | Filled-in templates with real content | None — terminal |

**Don't be religious about this.** Some components don't fit cleanly. Use the framework as guidance, not gospel.

### 3.2 Component API Design

A great component API feels like a great function signature: minimal, predictable, composable.

#### Bad vs. good

```jsx
// ❌ BAD — boolean flag explosion, hard to extend
<Button isPrimary isLarge isDisabled isLoading hasIcon iconLeft="check" />

// ✅ GOOD — variant + composition
<Button variant="primary" size="lg" disabled loading>
  <Icon name="check" />
  Save changes
</Button>
```

#### Composition over configuration

```jsx
// ❌ BAD — props every shape and content variant
<Card 
  title="Hello" 
  subtitle="..." 
  imageUrl="..." 
  showFooter 
  footerActions={[...]} 
/>

// ✅ GOOD — slots/children
<Card>
  <Card.Header>
    <Card.Title>Hello</Card.Title>
    <Card.Subtitle>...</Card.Subtitle>
  </Card.Header>
  <Card.Image src="..." />
  <Card.Footer>
    <Button>Action</Button>
  </Card.Footer>
</Card>
```

The compound component pattern (above) is the gold standard — see [Radix UI](https://www.radix-ui.com), [Ark UI](https://ark-ui.com), [shadcn/ui](https://ui.shadcn.com).

### 3.3 Container Queries & Style Queries

For 20 years, components could only respond to viewport size. Container queries finally let components respond to *their own* size — true component reusability.

```css
/* Define a container */
.card-grid {
  container-type: inline-size;
  container-name: card;
}

/* Component adapts based on its container, not viewport */
.card {
  display: flex;
  flex-direction: column;
}

@container card (min-width: 400px) {
  .card {
    flex-direction: row;  /* Side-by-side when container is wide */
  }
}
```

**Style queries** (newer, partial support) let you query parent custom property values:

```css
@container style(--theme: dark) {
  .card { background: black; }
}
```

### 3.4 Design Tokens

A token is a named, typed design decision: `color.brand.primary = #5B5BD6`. The single source of truth for everything visual.

#### Token tiers

1. **Primitive tokens** (raw values): `gray-100`, `gray-200`, ..., `blue-500`
2. **Semantic tokens** (intent-based): `color-bg-default`, `color-text-muted`, `color-border-strong`
3. **Component tokens** (component-specific): `button-primary-bg`, `button-primary-bg-hover`

Components reference component tokens → which reference semantic tokens → which reference primitives. This indirection is what enables themes, dark mode, white-labeling without rewriting components.

---

## 4. Interaction Design & Motion

### 4.1 Micro-interactions

#### Easing & duration

| Duration | Use for |
|----------|---------|
| **100-150ms** | Hover states, button presses, micro-feedback |
| **200-300ms** | Modals opening, drawer slides, accordion expands |
| **400-600ms** | Page transitions, complex orchestrations |
| **>800ms** | Almost never. Feels sluggish. |

| Easing curve | Use for | CSS |
|--------------|---------|-----|
| **ease-out** | Things entering (most common — feels responsive) | `cubic-bezier(0, 0, 0.2, 1)` |
| **ease-in** | Things leaving | `cubic-bezier(0.4, 0, 1, 1)` |
| **ease-in-out** | Looping, neutral motion | `cubic-bezier(0.4, 0, 0.2, 1)` |
| **spring** | Playful, bouncy interactions | Use Motion library or `linear()` function |

#### The `linear()` function

Modern CSS supports custom easing curves natively — no JS library needed for spring physics:

```css
.modal {
  transition: transform 400ms linear(
    0, 0.234, 0.518, 0.834, 1.122, 1.318, 1.402,
    1.394, 1.327, 1.234, 1.142, 1.075, 1.036, 1.018, 1
  );
}
```

Generate at [linear-easing-generator.netlify.app](https://linear-easing-generator.netlify.app).

### 4.2 State Management (UI states)

Every component that fetches/displays data has at least 5 states. Design all of them:

| State | Description | Common mistake |
|-------|-------------|----------------|
| **Ideal/loaded** | Data exists, displays beautifully | The only state most designers design |
| **Loading** | Data is being fetched | Spinners. Use skeletons instead. |
| **Empty** | No data yet (new user, filtered to nothing) | Generic "No data" — boring. Add CTAs and personality. |
| **Error** | Network failed, server error | "Something went wrong" — useless. Be specific, offer retry. |
| **Partial** | Some data, some loading | Often forgotten. Show what you have. |

### 4.3 Responsive & Adaptive Patterns

#### Mobile-first, content-first breakpoints

Stop designing at "mobile, tablet, desktop." Design at the breakpoints where *your content* breaks:

```css
/* Bad — device-based */
@media (min-width: 768px) { /* tablet */ }
@media (min-width: 1024px) { /* desktop */ }

/* Good — content-based */
@media (min-width: 42em) { /* when 2 columns become readable */ }
@media (min-width: 68em) { /* when sidebar fits comfortably */ }
```

Use `em` units for breakpoints — they respect user's font-size preferences.

#### Fluid layouts without breakpoints

Modern CSS can avoid breakpoints entirely:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 280px), 1fr));
  gap: 1rem;
}
```

This auto-adapts: 1 column on small screens, 2-3-4 columns as space allows. No media queries.

### 4.4 View Transitions API

The biggest UX upgrade to the web in years. Smooth transitions between page states with ~5 lines of code:

```css
@view-transition {
  navigation: auto;
}

/* Customize specific elements */
::view-transition-old(card),
::view-transition-new(card) {
  animation-duration: 400ms;
}

.product-card {
  view-transition-name: var(--card-id);
}
```

```js
// For JS-driven state changes
document.startViewTransition(() => {
  updateDOM();  // Make changes here; browser tweens between snapshots
});
```

Works in all Chromium browsers, Safari 18+, Firefox in 2026 (use `@supports` to progressively enhance).

---

## 5. Frontend Technical Best Practices

### 5.1 HTML Semantics

The right HTML is free accessibility, free SEO, and free intent.

#### Landmark structure

```html
<body>
  <header>           <!-- Site header -->
    <nav><!-- Primary nav --></nav>
  </header>
  <main>             <!-- ONE per page; the unique content -->
    <article>        <!-- Self-contained content -->
      <header><h1>Title</h1></header>
      <section><h2>Section</h2></section>
    </article>
    <aside><!-- Tangential --></aside>
  </main>
  <footer><!-- Site footer --></footer>
</body>
```

#### Heading outline

Headings should form a logical outline: `h1 > h2 > h3` — never skip levels (`h1 > h3`). One `h1` per page (debate exists; safer rule). Use the [HTML5 Outliner](https://gsnedders.html5.org/outliner/) to verify.

#### ARIA — last resort

> "The first rule of ARIA is: don't use ARIA."

Native HTML elements have built-in semantics and keyboard support. Use ARIA only when:
- You're building a custom component without a native equivalent (tab panels, autocomplete).
- You need to convey state (`aria-expanded`, `aria-selected`, `aria-busy`).
- You need to label something invisually (`aria-label`, `aria-labelledby`).

```html
<!-- ❌ Bad: divs with ARIA -->
<div role="button" tabindex="0" onclick="...">Click</div>

<!-- ✅ Good: native button -->
<button type="button" onclick="...">Click</button>
```

### 5.2 Modern CSS

#### Cascade Layers (`@layer`)

End the specificity wars. Define cascade order explicitly:

```css
@layer reset, base, tokens, components, utilities;

@layer reset {
  * { margin: 0; padding: 0; }
}

@layer components {
  .button { /* ... */ }
}

@layer utilities {
  .text-center { text-align: center; }  /* Always wins over components */
}
```

Order in `@layer` declaration determines priority — later layers win, regardless of selector specificity.

#### Native nesting

```css
.card {
  padding: 1rem;

  & .title {
    font-size: 1.5rem;
  }

  &:hover {
    transform: translateY(-2px);
  }

  @media (min-width: 768px) {
    padding: 2rem;
  }
}
```

No more SASS for nesting. Native, fast, debuggable.

#### `:has()` — the "parent selector"

Style a parent based on its children. This was impossible for 20 years.

```css
/* Card with image gets different padding */
.card:has(img) {
  padding: 0;
}

/* Form field wrapper turns red when input is invalid */
.field:has(input:invalid) {
  border-color: red;
}

/* Body scroll lock when modal is open */
body:has(dialog[open]) {
  overflow: hidden;
}
```

Game-changer for component logic that previously required JS.

### 5.3 JavaScript Framework Patterns

#### React Server Components (RSC) vs. Client Components

| RSC | Client Component |
|-----|------------------|
| Runs only on the server | Runs in the browser (and during SSR) |
| Zero JS shipped to client | JS bundle includes the component |
| Can directly access DB, filesystem | Limited to browser APIs |
| Can't use `useState`, `useEffect`, event handlers | Full interactivity |
| Default in Next.js App Router | Opt-in with `"use client"` |

**Mental model:** Use RSC for everything. Drop into Client Components only when you need interactivity (forms, hover, click, animation).

#### Signals (Solid, Svelte 5, Preact, Angular)

Signals = fine-grained reactivity. Instead of re-rendering whole components on state change, only the exact DOM nodes that depend on the changed value update.

```js
// Solid example
const [count, setCount] = createSignal(0);

// Only this text node updates when count changes — no re-render
return <p>Count: {count()}</p>;
```

Faster than React's "re-render the whole tree and diff" model for many UI patterns. The industry is shifting this direction.

### 5.4 Performance Budgeting

#### Core Web Vitals (2025+)

| Metric | What it measures | Good | Needs improvement |
|--------|------------------|------|-------------------|
| **LCP** (Largest Contentful Paint) | When the main content renders | < 2.5s | < 4s |
| **INP** (Interaction to Next Paint) | Responsiveness to all clicks/taps | < 200ms | < 500ms |
| **CLS** (Cumulative Layout Shift) | Visual stability — does stuff jump? | < 0.1 | < 0.25 |

INP replaced FID in 2024 as it measures *all* interactions, not just the first.

#### Image optimization

```html
<img
  src="/img/hero-800.webp"
  srcset="
    /img/hero-400.webp 400w,
    /img/hero-800.webp 800w,
    /img/hero-1600.webp 1600w
  "
  sizes="(max-width: 600px) 100vw, 50vw"
  alt="..."
  width="800"
  height="600"
  loading="lazy"      
  decoding="async"
  fetchpriority="auto"
>
```

For above-the-fold images: `fetchpriority="high"` and *do not* use `loading="lazy"`.

#### Font loading

```css
@font-face {
  font-family: 'Inter';
  src: url('/fonts/Inter.woff2') format('woff2');
  font-display: swap;        /* Show fallback immediately, swap when loaded */
  size-adjust: 100.06%;      /* Match metrics to fallback to prevent CLS */
  ascent-override: 90%;
}
```

Combine with `<link rel="preload" as="font" type="font/woff2" crossorigin>` for critical fonts.

---

## 6. Accessibility (A11y) & Inclusive Design

A11y is not a checklist you complete at the end. It's a baseline standard like security or performance.

### 6.1 Keyboard Navigation

#### Focus indicators

```css
/* ❌ Never do this */
*:focus { outline: none; }

/* ✅ Visible, beautiful focus styles */
:focus-visible {
  outline: 2px solid var(--color-accent);
  outline-offset: 2px;
  border-radius: 4px;
}
```

`:focus-visible` only shows the ring for keyboard users, not mouse clicks. Best of both worlds.

#### Tab order

Follow DOM order. Avoid `tabindex` values > 0 — they create unpredictable jumps. Use `tabindex="-1"` to make focusable-by-script-only (e.g., focus a modal heading on open).

#### Skip links

```html
<a href="#main" class="skip-link">Skip to main content</a>
```

```css
.skip-link {
  position: absolute;
  inset-inline-start: -9999px;
}
.skip-link:focus {
  inset-inline-start: 1rem;
  inset-block-start: 1rem;
}
```

Keyboard users shouldn't have to tab through 50 nav links to reach content.

### 6.2 Screen Reader Support

#### Alt text rules

| Image type | Alt text |
|------------|----------|
| Informative (photo, chart, diagram) | Describe the content/function: `alt="Bar chart showing Q3 revenue grew 23%"` |
| Decorative (background, divider) | Empty: `alt=""` — *not missing*, must be present |
| Functional (image as button/link) | Describe the action: `alt="Open menu"` |
| Complex (infographic) | Short alt + long description nearby or via `aria-describedby` |

#### ARIA live regions

For dynamic content (notifications, error toasts, live data):

```html
<!-- Polite: announces when user is idle -->
<div aria-live="polite" aria-atomic="true">
  Saved at 3:42 PM
</div>

<!-- Assertive: interrupts immediately. Use for errors only. -->
<div role="alert">
  Connection lost. Retrying...
</div>
```

### 6.3 Cognitive & Motor Considerations

#### Target sizes

WCAG 2.2 AA requires interactive targets to be at least **24x24 CSS pixels**. iOS HIG recommends **44x44pt**. Apple's research on tap accuracy is ~10mm = ~44px.

Don't just enlarge the visual — enlarge the **hit area**:

```css
.icon-button {
  position: relative;
  width: 24px;
  height: 24px;
}
.icon-button::before {
  content: '';
  position: absolute;
  inset: -10px;  /* Expands hit target without changing visual */
}
```

#### Reduced motion

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

For users with vestibular disorders, parallax and large transforms cause nausea. Respect their preference.

#### Color blindness

~8% of men have some form of color blindness. Never rely on color alone:

- Form errors: red border + ❌ icon + "Required" text
- Status badges: color + icon + text
- Charts: color + pattern + label

Test with [Stark](https://www.getstark.co), browser DevTools (Chrome → Rendering → Emulate vision deficiencies), or [Sim Daltonism](https://michelf.ca/projects/sim-daltonism).

---

## 7. The Designer-Developer Collaboration (DevRel)

### 7.1 Git for Designers

Designers should know the basics — even if they never use the CLI:

| Concept | What it means |
|---------|---------------|
| **Repo** | The project's folder, tracked over time |
| **Branch** | A parallel line of work — your changes don't affect `main` |
| **Commit** | A saved snapshot with a message describing the change |
| **PR (Pull Request)** | "Hey team, please review and merge my branch" |
| **Merge conflict** | Two people changed the same line — needs manual resolution |

#### Figma Branching

Figma's built-in branching mirrors Git for design files. Create a branch → make changes → request review → merge. Review-as-prototype is huge for catching issues before dev hands off.

### 7.2 Storybook

Component library catalog + visual testbed. Each component gets a "story" showing all its variants:

```jsx
// Button.stories.jsx
export default { component: Button };

export const Primary = { args: { variant: 'primary', children: 'Click me' } };
export const WithIcon = { args: { variant: 'primary', children: <><Icon /> Save</> } };
export const Loading = { args: { loading: true, children: 'Saving...' } };
```

Benefits:
- Develop components in isolation (no app context needed)
- Visual regression testing (Chromatic) — catches unintended UI changes
- Living documentation for designers and PMs to browse

### 7.3 Linting & Formatting

| Tool | Job | Config file |
|------|-----|-------------|
| **Prettier** | Auto-formats code (no debate about quotes/semicolons) | `.prettierrc` |
| **ESLint** | Catches JS/TS bugs and bad patterns | `eslint.config.js` |
| **Stylelint** | Same for CSS/SCSS | `.stylelintrc` |
| **Biome** | Newer, faster all-in-one (replaces Prettier+ESLint) | `biome.json` |

Run via pre-commit hooks (husky + lint-staged) so bad code never reaches the repo.

---

## 8. Practical Checklists & Prompts

### 8.1 UI Pre-Launch Checklist

#### Visual & Layout
- [ ] All text meets WCAG AA contrast (4.5:1 body, 3:1 large text)
- [ ] Layout works at 320px width (smallest mobile) without horizontal scroll
- [ ] Layout works at 1920px+ without awkward stretching
- [ ] Tested at browser zoom 200% — content reflows, doesn't break
- [ ] Dark mode tested for all screens (if supported)
- [ ] All images have `width` and `height` attributes (no CLS)
- [ ] Hero images use `fetchpriority="high"`, below-fold use `loading="lazy"`

#### Interaction & State
- [ ] Focus visible on every interactive element (`:focus-visible`)
- [ ] Hit targets ≥ 24x24px (ideally 44x44px)
- [ ] All buttons/links have hover, focus, active, disabled states
- [ ] Loading states designed (not just spinners — skeletons)
- [ ] Empty states designed with helpful CTAs
- [ ] Error states designed with specific messages and recovery paths
- [ ] Success feedback is clear and dismissable

#### Accessibility
- [ ] Keyboard-navigable (Tab through entire page — no traps)
- [ ] Screen reader tested (VoiceOver on Mac, NVDA on Windows)
- [ ] All `<img>` have meaningful or empty `alt` text
- [ ] Form fields have associated `<label>` (not placeholder-as-label)
- [ ] Form errors announced via `aria-live` or `role="alert"`
- [ ] `prefers-reduced-motion` respected
- [ ] Heading outline is logical (no skipped levels)
- [ ] Skip-to-content link present

#### Performance
- [ ] LCP < 2.5s on 4G mobile
- [ ] INP < 200ms for primary interactions
- [ ] CLS < 0.1
- [ ] Fonts loaded with `font-display: swap`
- [ ] JS bundle < 200KB compressed (homepage)
- [ ] Images served as AVIF/WebP with fallbacks

#### Cross-browser & Device
- [ ] Tested in Chrome, Safari, Firefox (latest 2 versions)
- [ ] Tested on real iOS Safari and Android Chrome
- [ ] Tested on slow 3G (DevTools throttling)
- [ ] Print stylesheet sane (or `@media print { display: none }` on noise)

### 8.2 Ten Claude Prompting Tips for High-Quality UI Code

1. **Specify the framework version and styling approach.** "Build with React 19 + TypeScript + Tailwind CSS v4" beats "build a React component."

2. **State the accessibility level upfront.** "Must meet WCAG 2.2 AA, including keyboard navigation and screen reader support."

3. **Name the aesthetic direction explicitly.** "Brutalist, monospace fonts, harsh borders, no shadows" gets better results than "make it look modern."

4. **Provide the design tokens.** Paste your color palette, spacing scale, and font choices instead of letting Claude invent them.

5. **Describe the data shape.** Show a sample JSON object the component will consume. Removes ambiguity.

6. **Specify all UI states.** "Design for loading, empty, error, success, and ideal states" — Claude will design only the ideal state otherwise.

7. **Mention performance constraints.** "Must be SSR-compatible," "Image must use srcset," "No client-side JS for this section."

8. **Request semantic HTML.** "Use semantic landmarks and proper heading hierarchy" — prevents `<div>`-soup output.

9. **Ask for the why, not just the code.** "Explain your color choices and motion timing decisions" — surfaces reasoning you can critique.

10. **Iterate in passes, not one shot.** Pass 1: structure + HTML. Pass 2: styling. Pass 3: interactions. Pass 4: polish + a11y. Single-shot prompts produce mediocre everything.

---

## 9. Glossary of Modern Terms

| Term | Definition |
|------|------------|
| **Atomic Design** | Methodology by Brad Frost organizing UI as atoms → molecules → organisms → templates → pages. |
| **Bento Grid** | Layout style of asymmetric, varying-size rectangular tiles, named after Japanese bento boxes. Popularized by Apple's marketing pages c. 2023. |
| **CLS (Cumulative Layout Shift)** | Core Web Vital measuring how much visible content unexpectedly shifts during page load. Goal: < 0.1. |
| **Container Query** | CSS query that styles based on a *container's* size, not the viewport's. Enables true component portability. |
| **Design Tokens** | Named, typed design decisions (colors, spacing, type) that serve as the single source of truth across design and code. |
| **Glassmorphism** | UI style with translucent, blurred backgrounds (`backdrop-filter: blur`). Peaked in 2020-2022. Use sparingly — accessibility-hostile. |
| **Hydration** | The process of attaching JS event handlers to server-rendered HTML to make it interactive. |
| **Hydration Mismatch** | Bug where server-rendered HTML differs from what the client renders, causing React/Vue to throw warnings or re-render incorrectly. |
| **INP (Interaction to Next Paint)** | Core Web Vital measuring responsiveness to all user interactions. Replaced FID in 2024. Goal: < 200ms. |
| **Islands Architecture** | Pattern where most of the page is static HTML with isolated "islands" of interactivity. See Astro, Fresh. |
| **JTBD (Jobs to be Done)** | Framework for understanding user motivation: *When [situation], I want [motivation], so I can [outcome]*. |
| **Layout Shift** | Visual jump when an element loads/resizes after initial paint. The CLS metric measures cumulative impact. |
| **Logical Properties** | CSS properties that adapt to writing direction (`margin-inline-start` instead of `margin-left`). Free i18n. |
| **OKLCH** | Modern CSS color space that's perceptually uniform, unlike HSL. Supports wider gamuts (P3 displays). |
| **Optimistic UI** | Updating the UI immediately on user action, before server confirms. Falls back if server rejects. Feels instant. |
| **Progressive Enhancement** | Build the baseline experience to work everywhere, then layer on enhancements (JS, animations) for capable browsers. |
| **RSC (React Server Components)** | React components that run only on the server, ship zero JS to the client. Default in Next.js App Router. |
| **Signals** | Fine-grained reactivity primitive used in Solid, Svelte 5, Preact, Angular. Updates only the exact DOM nodes affected by state changes. |
| **Skeleton Screen** | Placeholder UI mimicking the shape of incoming content while it loads. Better perceived performance than spinners. |
| **SSR (Server-Side Rendering)** | Rendering HTML on the server per request. Better for SEO and first paint than pure client-side rendering. |
| **SSG (Static Site Generation)** | Pre-rendering HTML at build time. Fastest possible delivery; works only when content doesn't change per user. |
| **View Transitions API** | Browser-native API for smooth transitions between page/state changes. Replaces heavy JS animation libraries for many cases. |
| **WCAG** | Web Content Accessibility Guidelines. Current version: 2.2 (Oct 2023). 2.2 AA is the legal/practical baseline. |

---

## Appendix: Recommended Resources

**Books**
- *Refactoring UI* — Adam Wathan & Steve Schoger
- *Atomic Design* — Brad Frost
- *Don't Make Me Think* — Steve Krug
- *Designing for the Web* — Mark Boulton

**Newsletters & Sites**
- [CSS-Tricks](https://css-tricks.com), [Smashing Magazine](https://smashingmagazine.com), [web.dev](https://web.dev)
- [Frontend Focus](https://frontendfoc.us), [CSS Weekly](https://css-weekly.com)
- [Refactoring UI](https://refactoringui.com), [UI Patterns](https://ui-patterns.com)

**Tools**
- Design: Figma, Penpot, Framer
- Prototyping: ProtoPie, Origami Studio
- Tokens: Style Dictionary, Tokens Studio (Figma plugin)
- A11y: axe DevTools, Stark, Pa11y, Polypane
- Performance: Lighthouse, WebPageTest, [PageSpeed Insights](https://pagespeed.web.dev)
- Component libs: Radix UI, Ark UI, shadcn/ui, Headless UI

---

*End of skill document.*
*Maintained as a living reference — fork and adapt to your team's stack.*
