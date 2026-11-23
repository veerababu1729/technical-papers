# HTML & CSS — Technical Paper
### A Complete Revision Guide for Beginners

> Written based on MountBlue cohort curriculum + layout drills + responsive drills projects.

---

## 📑 Index

1. [How a Web Page Works](#1-how-a-web-page-works)
2. [HTML Fundamentals](#2-html-fundamentals)
3. [Common Header Meta Tags](#3-common-header-meta-tags)
4. [Block vs Inline Elements](#4-block-vs-inline-elements)
5. [The Box Model](#5-the-box-model)
6. [CSS Selectors](#6-css-selectors)
7. [CSS Specificity](#7-css-specificity)
8. [CSS Units — What are Pixels?](#8-css-units--what-are-pixels)
9. [Positioning](#9-positioning)
10. [Normal Flow — In-flow vs Out-of-flow](#10-normal-flow--in-flow-vs-out-of-flow)
11. [Flexbox — Complete Breakdown](#11-flexbox--complete-breakdown)
12. [CSS Grid — Complete Breakdown](#12-css-grid--complete-breakdown)
13. [Common CSS Structural Classes](#13-common-css-structural-classes)
14. [Common CSS Styling Classes](#14-common-css-styling-classes)
15. [Responsive Design & Media Queries](#15-responsive-design--media-queries)
16. [CSS Reset](#16-css-reset)
17. [Naming Conventions](#17-naming-conventions)
18. [Project Best Practices](#18-project-best-practices)
19. [Quick Reference Cheatsheet](#19-quick-reference-cheatsheet)

---

## 1. How a Web Page Works

Three languages, three jobs:

| Language | Job | Example |
|----------|-----|---------|
| **HTML** | Structure & content | "There is a heading here" |
| **CSS** | Presentation & style | "That heading is red and 24px" |
| **JavaScript** | Behaviour & interactivity | "When clicked, show a popup" |

The browser reads HTML top-to-bottom, builds a **DOM (Document Object Model)** tree, then applies CSS to paint it on screen.

---

## 2. HTML Fundamentals

### Basic Page Structure
```html
<!DOCTYPE html>          <!-- tells browser: this is HTML5 -->
<html lang="en">         <!-- root element, lang for accessibility -->
<head>
    <!-- metadata, not visible on page -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <!-- everything visible goes here -->
</body>
</html>
```

### Semantic HTML Tags

> [!IMPORTANT]
> Always use **semantic tags** over generic `<div>` wherever possible. They give meaning to your structure — important for accessibility and SEO.

| Tag | Meaning | Use for |
|-----|---------|---------|
| `<header>` | Page/section header | Logo, nav, top bar |
| `<nav>` | Navigation links | Menu, breadcrumbs |
| `<main>` | Primary content | The core page content (only one per page) |
| `<section>` | Thematic grouping | Chapters, segments |
| `<article>` | Self-contained content | Blog post, news item |
| `<aside>` | Tangential content | Sidebar, related links |
| `<footer>` | Page/section footer | Copyright, links |
| `<figure>` | Media with caption | Images with `<figcaption>` |
| `<div>` | Generic container | When no semantic tag fits |
| `<span>` | Generic inline wrapper | Styling a word inside a sentence |

### Common Tags

```html
<!-- Headings (h1 = most important, only ONE h1 per page) -->
<h1>Main Title</h1>
<h2>Section Title</h2>

<!-- Paragraph -->
<p>Some text here.</p>

<!-- Links -->
<a href="https://example.com">Click me</a>
<a href="page2.html">Internal link</a>

<!-- Images (always include alt for accessibility) -->
<img src="photo.jpg" alt="Description of image">

<!-- Lists -->
<ul>  <!-- unordered -->
    <li>Item 1</li>
</ul>
<ol>  <!-- ordered -->
    <li>Step 1</li>
</ol>
```

---

## 3. Common Header Meta Tags

```html
<head>
    <!-- Character encoding — always include, enables all languages/symbols -->
    <meta charset="UTF-8">

    <!-- CRITICAL for responsive design — without this, mobile ignores media queries -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <!-- Page description for search engines (shows in Google results) -->
    <meta name="description" content="A site about green energy.">

    <!-- Author -->
    <meta name="author" content="Veera">

    <!-- Open Graph — controls how your page looks when shared on social media -->
    <meta property="og:title" content="My Page">
    <meta property="og:image" content="thumbnail.jpg">

    <!-- Link external CSS -->
    <link rel="stylesheet" href="style.css">

    <!-- Page title shown in browser tab -->
    <title>My Website</title>
</head>
```

> [!WARNING]
> **Most common mistake:** Forgetting `<meta name="viewport">`. Without it, your media queries will NOT work on mobile. The phone renders the page at desktop width and zooms out.

---

## 4. Block vs Inline Elements

> [!NOTE]
> This is one of the most important concepts. Understanding this explains *why* elements behave the way they do by default.

### Block Elements
- Take up the **full width** of their parent
- Always start on a **new line**
- You can set `width`, `height`, `margin`, `padding` freely

```
Examples: <div>, <p>, <h1>-<h6>, <section>, <header>, <footer>,
          <ul>, <ol>, <li>, <main>, <article>, <aside>
```

### Inline Elements
- Take up only as much **width as their content**
- Flow within text, do NOT start new lines
- `width` and `height` have **no effect** on inline elements
- Top/bottom `margin` and `padding` don't push other elements

```
Examples: <span>, <a>, <strong>, <em>, <img>, <input>, <button>
```

### `display: inline-block`
Best of both worlds:
- Sits **side by side** (like inline)
- Respects `width`, `height`, `margin`, `padding` (like block)

```css
/* Used in CSS Layout Drills Challenge 1 */
div {
    display: inline-block;
    width: 150px;
    height: 150px;
}
```

> [!WARNING]
> **Common confusion:** People try to set `width`/`height` on `<span>` or `<a>` and wonder why it doesn't work. It's because they're inline. Fix: add `display: block` or `display: inline-block`.

---

## 5. The Box Model

Every HTML element is a **rectangular box** with 4 layers:

```
┌─────────────────────────────────────┐
│              MARGIN                 │  ← space outside the border (transparent)
│  ┌───────────────────────────────┐  │
│  │           BORDER              │  │  ← visible border line
│  │  ┌─────────────────────────┐  │  │
│  │  │        PADDING          │  │  │  ← space inside the border (same bg color)
│  │  │  ┌───────────────────┐  │  │  │
│  │  │  │     CONTENT       │  │  │  │  ← actual text/image
│  │  │  └───────────────────┘  │  │  │
│  │  └─────────────────────────┘  │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
```

```css
div {
    width: 200px;
    height: 100px;
    padding: 20px;        /* inside spacing */
    border: 2px solid black;
    margin: 10px;         /* outside spacing */
}
```

### `box-sizing: border-box` — The Most Important Fix

By default (`content-box`), `width` only counts the content. So a `width: 200px` box with `padding: 20px` is actually **240px wide**. That breaks layouts!

```css
/* Always put this at the top of your CSS */
* {
    box-sizing: border-box;
}
```

With `border-box`, `width: 200px` means the box is **exactly 200px** — padding and border are included inside that 200px. Much more predictable!

> [!IMPORTANT]
> **Always use `box-sizing: border-box` in your reset CSS.** This is standard practice in every real-world project.

### Margin vs Padding — when to use which?

| Use `margin` when... | Use `padding` when... |
|---|---|
| Spacing **between** elements | Spacing **inside** an element (content away from edges) |
| Background color should NOT fill the space | Background color SHOULD fill the space |
| Pushing elements apart | Making a button feel "bigger" |

---

## 6. CSS Selectors

### Basic Selectors

```css
/* Element selector — all <p> tags */
p { color: red; }

/* Class selector — anything with class="card" */
.card { background: white; }

/* ID selector — element with id="header" (only ONE per page) */
#header { height: 60px; }

/* Universal selector — everything */
* { box-sizing: border-box; }
```

### Combinators

```css
/* Descendant — any <p> inside .container (any depth) */
.container p { color: blue; }

/* Child — only DIRECT children */
.container > p { color: blue; }

/* Adjacent sibling — first <p> immediately after <h2> */
h2 + p { margin-top: 0; }

/* General sibling — all <p> after <h2> in same parent */
h2 ~ p { color: gray; }
```

### Pseudo-classes (state-based)

```css
a:hover   { color: red; }         /* when mouse is over it */
a:visited { color: purple; }      /* already clicked link */
input:focus { border: 2px solid blue; }  /* when input is selected */

li:first-child  { font-weight: bold; }  /* first item in list */
li:last-child   { border: none; }       /* last item */
li:nth-child(2) { color: red; }         /* second item */
li:nth-child(odd)  { background: #eee; } /* zebra striping */
```

### Complex Selectors

```css
/* Multiple selectors — apply same style to both */
h1, h2, h3 { font-family: sans-serif; }

/* Attribute selector */
input[type="text"] { border: 1px solid gray; }
a[href^="https"]   { color: green; }   /* href starts with https */
a[href$=".pdf"]    { color: red; }     /* href ends with .pdf */

/* Pseudo-elements */
p::first-line   { font-weight: bold; }
p::before       { content: "→ "; }
input::placeholder { color: lightgray; }
```

> [!NOTE]
> **`:`** is a pseudo-class (state like `:hover`). **`::`** is a pseudo-element (part of element like `::before`). One colon works for both but `::` is the modern standard for pseudo-elements.

---

## 7. CSS Specificity

When two CSS rules target the same element, the **more specific one wins**.

### Specificity Hierarchy (highest wins)

| Selector type | Specificity value | Example |
|---|---|---|
| `!important` | Overrides everything | `color: red !important` |
| Inline styles | 1,0,0,0 | `<p style="color:red">` |
| ID selectors | 0,1,0,0 | `#header` |
| Class, attribute, pseudo-class | 0,0,1,0 | `.nav`, `[type]`, `:hover` |
| Element, pseudo-element | 0,0,0,1 | `p`, `div`, `::before` |
| Universal selector | 0,0,0,0 | `*` |

### Calculating Specificity

```css
p              → (0,0,0,1)   /* 1 element */
.card          → (0,0,1,0)   /* 1 class */
.card p        → (0,0,1,1)   /* 1 class + 1 element */
#nav .item     → (0,1,1,0)   /* 1 ID + 1 class */
```

### When Same Specificity — Last One Wins

```css
p { color: red; }
p { color: blue; }  /* ← this wins, it comes later in the file */
```

> [!WARNING]
> **Avoid `!important` in real projects.** It makes debugging a nightmare. If you're using it, it means your specificity structure is broken. Fix the selector instead.

> [!WARNING]
> **Avoid ID selectors for styling.** IDs are so specific they're very hard to override. Use classes instead.

---

## 8. CSS Units — What are Pixels?

### What is a Pixel?

A **pixel (px)** is the smallest unit of a screen. Your monitor is a grid of tiny dots — each dot is a pixel.

- A `1920×1080` monitor has 1920 columns × 1080 rows = ~2 million dots.
- **CSS pixel ≠ physical pixel** on modern screens (Retina/HiDPI). A CSS `1px` might equal 2 or 3 physical dots on a Retina display. The browser handles this automatically — you don't need to worry about it.
- `px` in CSS = a consistent, predictable unit you can reason about visually.

```css
/* 1px = roughly 1/96th of an inch on screen */
border: 1px solid black;   /* very thin line */
border: 3px solid black;   /* thicker line */
```

### Absolute Units (Fixed size)

```css
px   /* pixels — most common */
pt   /* points — 1pt = 1/72 inch (for print) */
cm, mm, in   /* physical units (rarely used in web) */
```

### Relative Units (Preferred for Responsive Design)

| Unit | Relative to | Best used for |
|------|------------|---------------|
| `%` | Parent element's dimension | Widths inside containers |
| `rem` | Root `<html>` font-size (default 16px) | Font sizes, spacing |
| `em` | Current element's font-size | Avoid — compounds and confuses |
| `vw` | 1% of viewport width | Full-width elements |
| `vh` | 1% of viewport height | Full-height sections |
| `vmin` | Smaller of vw/vh | Rare use cases |

```css
/* rem example — html default font-size is 16px */
h1    { font-size: 2rem; }       /* = 32px */
p     { font-size: 1rem; }       /* = 16px */
small { font-size: 0.875rem; }   /* = 14px */

/* % for widths — responsive */
.sidebar { width: 25%; }     /* 25% of parent's width */
.main    { width: 75%; }

/* vh for full-screen hero sections */
.hero    { min-height: 100vh; }  /* full viewport height */
```

> [!IMPORTANT]
> **MountBlue rule:** Use `rem` for font sizes. Don't use `px`, `em`, `vh`, or `vw` for fonts. `rem` scales with user's browser preferences (accessibility).

> [!NOTE]
> `1rem = 16px` by default. Some developers set `html { font-size: 62.5%; }` to make `1rem = 10px` for easier math (`1.4rem = 14px`).

> [!WARNING]
> **`em` compounds:** If parent is `2em` and child is `1.5em`, child ends up being `2 × 1.5 = 3em` of the root. This cascading confusion is why `rem` is preferred.

---

## 9. Positioning

### `position: static` (default)
Normal document flow. `top/left/right/bottom` have no effect.

### `position: relative`
Stays in normal flow. Moves **relative to its own original position** using `top/left/right/bottom`.
```css
.box {
    position: relative;
    top: 20px;    /* moves DOWN 20px from where it would naturally be */
    left: 10px;   /* moves RIGHT 10px */
}
```
> Also used as a **reference point** for absolutely positioned children.

### `position: absolute`
**Removed from normal flow** (other elements ignore it). Positioned relative to nearest **`position: relative` ancestor**. If none, uses `<body>`.

```css
/* Parent is the reference */
.parent {
    position: relative;
}

/* Child placed 10px from top-right of parent */
.child {
    position: absolute;
    top: 10px;
    right: 10px;
}
```

Used in layout drills for: staircase layouts, overlapping boxes, nested boxes at corners.

### `position: fixed`
Like absolute, but positioned relative to the **viewport** (screen). Stays put when you scroll.
```css
/* Sticky nav bar */
nav {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
}
```

### `position: sticky`
Hybrid: flows normally until it hits a scroll threshold, then acts like `fixed`.
```css
header {
    position: sticky;
    top: 0;   /* sticks when it reaches top of viewport */
}
```

> [!WARNING]
> **Most common mistake with absolute:** Forgetting to add `position: relative` to the parent. Without it, the child positions relative to `<body>`, not the intended parent — it flies to a totally unexpected place.

---

## 10. Normal Flow — In-flow vs Out-of-flow

### In-flow elements:
- Participate in the document layout
- Take up space, push other elements around
- Block elements stack vertically; inline elements flow horizontally

### Out-of-flow elements:
These are **invisible to other elements** — they don't affect surrounding layout:
- `position: absolute`
- `position: fixed`
- `float` (legacy)

```css
/* This div is out of flow — sibling elements will overlap it */
.overlay {
    position: absolute;
    top: 0;
    left: 0;
}
```

> [!IMPORTANT]
> **MountBlue guideline:** Minimise use of `position: absolute/fixed`. Use Flexbox or Grid instead. Only use absolute positioning when elements need to **overlap** each other.

---

## 11. Flexbox — Complete Breakdown

The modern way to lay out elements in **one dimension** (row OR column).

### The Two Axes — Most Confusing Part

```
flex-direction: row (default)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ → MAIN AXIS (horizontal)
│  [item1]  [item2]  [item3]        │
                                     ↓ CROSS AXIS (vertical)

flex-direction: column
│ MAIN AXIS (vertical)
↓
[item1]
[item2]
[item3]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ → CROSS AXIS (horizontal)
```

| Property | Controls | `row` direction | `column` direction |
|----------|----------|-----------------|-------------------|
| `justify-content` | Main axis | Horizontal | Vertical |
| `align-items` | Cross axis | Vertical | Horizontal |

> [!WARNING]
> **The #1 Flexbox confusion:** `justify-content` and `align-items` SWAP roles when you change `flex-direction`! When in `column` mode, `justify-content` controls vertical and `align-items` controls horizontal — the opposite of `row` mode.

```css
/* Example from Flexbox Froggy */
.container {
    display: flex;
    flex-direction: column;    /* Main axis is now VERTICAL */
    justify-content: center;   /* Centers items VERTICALLY (main axis) */
    align-items: flex-start;   /* Aligns items to the LEFT (cross axis) */
    gap: 12px;                 /* Fixed spacing between items */
}
```

---

### Container Properties (on the parent)

#### `display`
```css
display: flex;         /* block-level flex container */
display: inline-flex;  /* inline-level flex container */
```

#### `flex-direction` — sets the main axis
```css
flex-direction: row;             /* → left to right (default) */
flex-direction: row-reverse;     /* ← right to left */
flex-direction: column;          /* ↓ top to bottom */
flex-direction: column-reverse;  /* ↑ bottom to top */
```

#### `justify-content` — aligns on MAIN axis
```css
justify-content: flex-start;    /* pack to start (default) */
justify-content: flex-end;      /* pack to end */
justify-content: center;        /* center */
justify-content: space-between; /* equal gaps BETWEEN items (no edge gaps) */
justify-content: space-around;  /* equal gaps AROUND items (half-size edge gaps) */
justify-content: space-evenly;  /* equal gaps everywhere including edges */
```

#### `align-items` — aligns on CROSS axis (single line)
```css
align-items: stretch;      /* items stretch to fill container height (default) */
align-items: flex-start;   /* items align to start of cross axis */
align-items: flex-end;     /* items align to end of cross axis */
align-items: center;       /* items center on cross axis */
align-items: baseline;     /* items align by their text baseline */
```

#### `flex-wrap` — controls wrapping
```css
flex-wrap: nowrap;        /* all items on ONE line, may overflow (default) */
flex-wrap: wrap;          /* items wrap to NEXT line when no space */
flex-wrap: wrap-reverse;  /* items wrap to PREVIOUS line (upward) */
```

```
nowrap:   [1][2][3][4][5]  ← all squished on one row

wrap:     [1][2][3]
          [4][5]            ← 4 and 5 wrap to next row

wrap-reverse: [4][5]        ← wraps upward
              [1][2][3]
```

#### `flex-flow` — shorthand for direction + wrap
```css
flex-flow: row wrap;           /* flex-direction + flex-wrap in one line */
flex-flow: column nowrap;
```

#### `align-content` — aligns MULTIPLE lines (only when flex-wrap: wrap is set)
```css
/* Only works when there are multiple wrapped rows */
align-content: flex-start;    /* rows pack to top */
align-content: flex-end;      /* rows pack to bottom */
align-content: center;        /* rows center vertically */
align-content: space-between; /* equal gaps between rows */
align-content: space-around;  /* equal gaps around rows */
align-content: stretch;       /* rows stretch to fill (default) */
```

> [!NOTE]
> **`align-items` vs `align-content`:**
> - `align-items` aligns items within a single row on the cross axis
> - `align-content` aligns the multiple wrapped rows themselves — only relevant with `flex-wrap: wrap`

#### `gap`
```css
gap: 20px;           /* equal gap between all items */
gap: 20px 10px;      /* row-gap: 20px | column-gap: 10px */
row-gap: 20px;
column-gap: 10px;
```

---

### Item Properties (on children)

#### `order` — visual ordering without changing HTML
```css
.item { order: 0; }   /* default for all items */
.item { order: 1; }   /* appears after order: 0 items */
.item { order: -1; }  /* appears before order: 0 items */
```

Used in responsive drill Set 2 to put `b | a | c` on desktop and `a | b | c` on mobile.

#### `flex-grow` — how much an item grows
```css
/* If container has 300px leftover space: */
.item-a { flex-grow: 1; }   /* gets 100px (1/3 of leftover) */
.item-b { flex-grow: 2; }   /* gets 200px (2/3 of leftover) */
/* item with flex-grow: 0 (default) gets no extra space */
```

#### `flex-shrink` — how much an item shrinks
```css
.item { flex-shrink: 1; }   /* default — can shrink */
.item { flex-shrink: 0; }   /* won't shrink, may overflow */
```

#### `flex-basis` — base size before grow/shrink
```css
.item { flex-basis: 200px; }  /* starts at 200px, then grows/shrinks */
.item { flex-basis: auto; }   /* uses item's natural width (default) */
.item { flex-basis: 0; }      /* starts from 0, grow/shrink from scratch */
```

#### `flex` — shorthand (grow shrink basis)
```css
flex: 1;           /* same as flex: 1 1 0 — common equal-width pattern */
flex: auto;        /* flex: 1 1 auto */
flex: none;        /* flex: 0 0 auto — won't grow or shrink */
flex: 0 0 200px;   /* fixed 200px — won't grow or shrink */
```

#### `align-self` — override `align-items` for one item
```css
.special {
    align-self: flex-end;   /* this item alone goes to end of cross axis */
}
/* values: auto | flex-start | flex-end | center | baseline | stretch */
```

#### The Auto Margin Trick
```css
/* Push everything after this item to the right */
.item {
    margin-left: auto;  /* eats all remaining space on the left */
}
/* Used in Layout Drill Challenge 1 to push div2+div3 to far right */
```

---

### Flexbox Summary Table

| Property | Goes on | Controls |
|----------|---------|---------|
| `display: flex` | Container | Activate flexbox |
| `flex-direction` | Container | Main axis direction |
| `justify-content` | Container | Alignment on main axis |
| `align-items` | Container | Alignment on cross axis (1 line) |
| `align-content` | Container | Alignment of multiple wrapped lines |
| `flex-wrap` | Container | Whether items wrap |
| `flex-flow` | Container | Shorthand: direction + wrap |
| `gap` | Container | Space between items |
| `order` | Item | Visual order |
| `flex-grow` | Item | How much item grows |
| `flex-shrink` | Item | How much item shrinks |
| `flex-basis` | Item | Starting size |
| `flex` | Item | Shorthand: grow shrink basis |
| `align-self` | Item | Cross-axis override for 1 item |

---

## 12. CSS Grid — Complete Breakdown

For **two-dimensional** layouts (rows AND columns simultaneously).

### Setup

```css
.container {
    display: grid;         /* activates grid */
    display: inline-grid;  /* inline-level grid */
}
```

### Defining Columns and Rows

```css
.container {
    /* Explicit column widths */
    grid-template-columns: 200px 1fr 200px;      /* fixed | flexible | fixed */
    grid-template-columns: repeat(3, 1fr);       /* 3 equal columns */
    grid-template-columns: repeat(4, 25%);       /* 4 columns of 25% each */
    grid-template-columns: 1fr 2fr 1fr;          /* 25% | 50% | 25% */
    grid-template-columns: minmax(100px, 1fr);   /* between 100px and 1fr */
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); /* responsive! */

    /* Explicit row heights */
    grid-template-rows: 80px 1fr 60px;           /* header | content | footer */
    grid-template-rows: auto;                    /* height = content height */

    /* Gap */
    gap: 20px;             /* equal row and column gap */
    row-gap: 20px;
    column-gap: 10px;
}
```

### The `fr` Unit — Grid Superpower

`1fr` = **one fraction** of the **available space** after fixed sizes are taken.

```css
grid-template-columns: 200px 1fr 1fr;
/* Fixed 200px sidebar, then 2 columns sharing the rest equally */

grid-template-columns: 1fr 2fr 1fr;
/* Total = 4 parts: left=25%, center=50%, right=25% */
```

### `repeat()` function

```css
repeat(3, 1fr)             /* = 1fr 1fr 1fr */
repeat(2, 100px 200px)     /* = 100px 200px 100px 200px */
repeat(auto-fit, minmax(200px, 1fr))   /* as many columns as fit, min 200px each */
repeat(auto-fill, 200px)   /* fills row with 200px columns including empty ones */
```

> [!NOTE]
> **`auto-fit` vs `auto-fill`:** `auto-fit` collapses empty columns (items stretch). `auto-fill` keeps empty columns (items don't stretch). Usually you want `auto-fit`.

### Placing Items — Line Numbers

Grid lines are numbered starting from 1:
```
  1    2    3    4
  |    |    |    |
--+----+----+----+--
  | A  | B  | C  |   row 1
--+----+----+----+--
  | D  | E  | F  |   row 2
--+----+----+----+--
```

```css
.item {
    grid-column: 1 / 3;    /* from line 1 to line 3 (spans 2 cols) */
    grid-column: 2 / -1;   /* from line 2 to last line */
    grid-column: 1 / span 2; /* start at 1, span 2 columns */

    grid-row: 1 / 2;       /* row 1 only */
    grid-row: 1 / -1;      /* all rows */
}

/* Make header span full width */
header {
    grid-column: 1 / -1;
}
```

### Named Template Areas (Most Readable)

```css
.container {
    display: grid;
    grid-template-columns: 200px 1fr;
    grid-template-rows: 60px 1fr 60px;
    grid-template-areas:
        "header  header"    /* . = empty cell */
        "sidebar main"
        "footer  footer";
}

header   { grid-area: header; }
.sidebar { grid-area: sidebar; }
main     { grid-area: main; }
footer   { grid-area: footer; }
```

### Alignment in Grid

```css
.container {
    /* Align ALL items inside their cells */
    justify-items: start | end | center | stretch;  /* horizontal */
    align-items:   start | end | center | stretch;  /* vertical */
    place-items: center center;                     /* shorthand: align justify */

    /* Align the whole grid track inside the container */
    justify-content: start | end | center | space-between | space-around | space-evenly;
    align-content:   start | end | center | space-between | space-around | space-evenly;
}

/* Individual item override */
.item {
    justify-self: start | end | center | stretch;
    align-self:   start | end | center | stretch;
    place-self: center end;  /* shorthand */
}
```

### Flexbox vs Grid — When to Use Which

| Use Flexbox | Use Grid |
|-------------|----------|
| 1D layout (a row OR a column) | 2D layout (rows AND columns) |
| Nav bars, card rows, button groups | Page layouts, dashboard grids |
| When content size should drive layout | When you define explicit track sizes |
| Items that naturally flow | Items that need exact placement |

> [!TIP]
> They work together! Use Grid for the page-level layout (header, sidebar, main, footer). Use Flexbox inside each section for the items within it.

---

## 13. Common CSS Structural Classes

These classes handle **layout and positioning** — not colors or fonts.

```css
/* Container — constrains max-width and centers content */
.container {
    max-width: 1200px;
    margin: 0 auto;    /* auto left-right margins = centered */
    padding: 0 20px;
}

/* Flex row */
.flex-row {
    display: flex;
    align-items: center;
}

/* Grid layout */
.grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
}

/* Full-width section */
.full-width {
    width: 100%;
}

/* Visually hidden but accessible */
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
}
```

---

## 14. Common CSS Styling Classes

These classes handle **visual appearance** — colors, fonts, spacing.

```css
/* Typography */
.text-center { text-align: center; }
.text-bold   { font-weight: bold; }
.text-muted  { color: #666; }

/* Spacing utilities */
.mt-1 { margin-top: 0.5rem; }
.mt-2 { margin-top: 1rem; }
.p-1  { padding: 0.5rem; }
.p-2  { padding: 1rem; }

/* Colors */
.bg-primary { background-color: #3498db; }
.text-white { color: white; }

/* Border & shadow */
.rounded { border-radius: 8px; }
.shadow  { box-shadow: 0 2px 8px rgba(0,0,0,0.1); }

/* Visibility */
.hidden  { display: none; }
.visible { display: block; }

/* Card pattern */
.card {
    background: white;
    border-radius: 8px;
    padding: 1.5rem;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}
```

> [!TIP]
> **Separate structural and styling classes** (MountBlue guideline). Structure: `class="flex-row container"`. Style: `class="bg-primary text-white rounded"`. This makes classes reusable across pages.

---

## 15. Responsive Design & Media Queries

### The Core Idea
Different CSS rules apply at different screen widths. One HTML file → many layouts.

### Breakpoints (Common)

| Name | Width | Target |
|------|-------|--------|
| Mobile | `< 600px` | Phones |
| Tablet | `600px – 900px` | Tablets, large phones |
| Desktop | `> 900px` | Laptops, monitors |

### Mobile-First Approach (Recommended)

Write CSS for **mobile first** (no media query), then add `min-width` queries to enhance for larger screens:

```css
/* BASE — Mobile (default, no query) */
.container {
    flex-direction: column;    /* stack vertically on small screens */
}

/* TABLET — 600px and above */
@media (min-width: 600px) {
    .container {
        flex-direction: row;   /* side by side on tablet */
    }
}

/* DESKTOP — 900px and above */
@media (min-width: 900px) {
    .sidebar { width: 25%; }
    .main    { width: 75%; }
}
```

### Desktop-First (Avoid — legacy)

```css
/* Base is desktop, then override with max-width for smaller */
@media (max-width: 768px) {
    /* tablet styles */
}
```

### Media Query Syntax

```css
/* Width-based (most common) */
@media (min-width: 600px) { }
@media (max-width: 900px) { }

/* Range */
@media (min-width: 600px) and (max-width: 900px) { }

/* Orientation */
@media (orientation: landscape) { }

/* Dark mode */
@media (prefers-color-scheme: dark) { }

/* Print */
@media print { }
```

### Responsive Patterns from Drills

**Set 1:** Sidebar spans full height on right, main content stacks on left.
```css
/* Mobile: stack all */
.container { display: flex; flex-direction: column; }

/* Tablet */
@media (min-width: 600px) {
    .container { flex-direction: row; }
    .main-content { width: 50%; }
    .section-c    { width: 50%; }
}

/* Desktop */
@media (min-width: 900px) {
    .main-content { width: 75%; }
    .section-c    { width: 25%; }
}
```

**Set 2:** Reorder elements with `order` between breakpoints.
```css
/* Mobile: a, b, c stacked */
.section-a { order: 1; }
.section-b { order: 2; }
.section-c { order: 3; }

/* Tablet: a full-width top, b+c side by side */
@media (min-width: 600px) {
    .container { flex-wrap: wrap; flex-direction: row; }
    .section-a { width: 100%; order: 1; }
    .section-b { width: 50%; order: 2; }
    .section-c { width: 50%; order: 3; }
}

/* Desktop: b | a | c in one row */
@media (min-width: 900px) {
    .section-a { width: 50%; order: 2; }
    .section-b { width: 25%; order: 1; }
    .section-c { width: 25%; order: 3; }
}
```

> [!WARNING]
> **Most common mistake:** Forgetting `<meta name="viewport">` in HTML. Media queries will NOT work on actual mobile devices without it.

> [!TIP]
> Test responsive layouts using **Chrome DevTools** (F12) → Toggle Device Toolbar (Ctrl+Shift+M). Select device presets or enter custom widths.

---

## 16. CSS Reset

Browsers apply default styles (margins, paddings, font sizes) that differ across Chrome, Firefox, Safari. A reset removes these inconsistencies.

```css
/* Minimal reset — put this in reset.css, link before your main CSS */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* Optional additions */
body {
    font-family: sans-serif;
    line-height: 1.5;
}

img {
    max-width: 100%;
    display: block;
}

a {
    text-decoration: none;
    color: inherit;
}
```

> [!IMPORTANT]
> **Always link `reset.css` BEFORE your main CSS file.** Otherwise your styles load first and the reset undoes them.

```html
<link rel="stylesheet" href="reset.css">    <!-- first -->
<link rel="stylesheet" href="style.css">    <!-- second, overrides reset -->
```

---

## 17. Naming Conventions

Good naming makes your code readable, maintainable, and professional.

### File & Folder Names

```
✅ Correct (lowercase, hyphen-separated):
index.html
about-us.html
responsive-styles-a.css
reset.css
hero-image.jpg
images/
layout-drills/
challenge1/

❌ Avoid:
Index.HTML          ← uppercase (case-sensitive on Linux servers)
aboutUs.html        ← camelCase (not standard for files)
responsive styles.css  ← spaces in filenames (breaks URLs)
MyFolder/           ← PascalCase for folders
```

> [!WARNING]
> Spaces in file/folder names break URLs. `my page.html` becomes `my%20page.html` in a URL — always use hyphens instead.

### CSS Class Names

```css
/* ✅ BEM (Block Element Modifier) — industry standard */
.card { }                   /* Block */
.card__title { }            /* Element (double underscore) */
.card__title--large { }     /* Modifier (double hyphen) */

.nav { }
.nav__item { }
.nav__item--active { }

/* ✅ Utility classes (like Tailwind) */
.flex { display: flex; }
.text-center { text-align: center; }
.mt-2 { margin-top: 1rem; }

/* ✅ Descriptive, lowercase, hyphen-separated */
.hero-section { }
.main-content { }
.section-a { }

/* ❌ Avoid */
.MyClass { }       ← PascalCase
.myClass { }       ← camelCase
.s { }             ← cryptic
.red { }           ← describes appearance, not purpose (what if color changes?)
#style1 { }        ← ID for styling
```

### HTML `id` vs `class`

| | `id` | `class` |
|--|------|---------|
| Uniqueness | One per page | Many elements can share |
| CSS use | Avoid for styling | Preferred for styling |
| JS use | Yes — `getElementById` | Yes — `querySelectorAll` |
| Anchor links | Yes — `href="#section"` | No |

### Folder Structure Convention

```
project-name/           ← lowercase, hyphen-separated
├── index.html
├── about.html
├── reset.css
├── common.css          ← shared across all pages
├── home.css            ← page-specific
├── about.css
├── images/
│   ├── hero.jpg
│   ├── logo.svg
│   └── icons/
├── fonts/
└── README.md
```

---

## 18. Project Best Practices

### CSS Architecture (MountBlue Guidelines)

| ✅ Do | ❌ Avoid |
|-------|---------|
| Use `rem` for font-size | Using `px` for font-size |
| Use `%` for widths in containers | Fixed `px` widths for sections |
| Use Flexbox/Grid for layout | Using `position: absolute` for layout |
| Use `min-height` instead of `height` | Using `height` (breaks when content grows) |
| Separate structure and style classes | Mixing everything into one class |
| Mobile-first with `min-width` queries | Desktop-first with `max-width` |
| `margin: 0 auto` to center a block | Using `position: absolute` to center |
| Write CSS top-to-bottom: reset → base → layout → components | Random ordering |

### The Layout Checklist
Before adding colors and fonts, make sure:
1. ✅ Semantic HTML structure is correct
2. ✅ Reset CSS applied and linked first
3. ✅ Viewport meta tag present in HTML
4. ✅ `box-sizing: border-box` applied globally
5. ✅ Mobile layout works (stack everything)
6. ✅ Tablet layout works (flex row, flex-wrap)
7. ✅ Desktop layout works (adjust widths)
8. ✅ No unnecessary `position: absolute`
9. ✅ Footer sticks to bottom (`flex: 1` on main content)
10. ✅ Images are responsive (`max-width: 100%`)

---

## 19. Quick Reference Cheatsheet

### Centering (the most Googled thing in CSS)

```css
/* Horizontally center a BLOCK element */
div { margin: 0 auto; width: 80%; }

/* Center text */
p { text-align: center; }

/* Center with Flexbox (horizontal + vertical) */
.parent {
    display: flex;
    justify-content: center;
    align-items: center;
}

/* Center with Grid */
.parent {
    display: grid;
    place-items: center;   /* shorthand for align-items + justify-items */
}

/* Center with absolute positioning */
.child {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

### Sticky Footer
```css
body {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}
main { flex: 1; }   /* grows to push footer down */
```

### Responsive Images
```css
img {
    max-width: 100%;   /* never overflows its container */
    height: auto;      /* maintains aspect ratio */
    display: block;    /* removes inline gap below image */
}
```

### Common Gotchas

| Problem | Cause | Fix |
|---------|-------|-----|
| Element not responding to `width`/`height` | It's `display: inline` | Add `display: block` or `inline-block` |
| Absolute child goes to wrong place | Parent isn't `position: relative` | Add `position: relative` to parent |
| Media queries not working on phone | Missing viewport meta tag | Add `<meta name="viewport" ...>` |
| Box bigger than expected | Default `box-sizing: content-box` | Use `box-sizing: border-box` |
| White space below image | Image is inline (baseline gap) | `img { display: block; }` |
| `margin: auto` not centering | Element is not block or has no width | Set `width` and use `display: block` |
| Flexbox items not wrapping | Missing `flex-wrap: wrap` | Add `flex-wrap: wrap` |
| `z-index` not working | Element not positioned | Add `position: relative` |
| `justify-content` not working vertically | In `row` mode, it's horizontal | Switch to `align-items` or change to `flex-direction: column` |
| `align-content` doing nothing | Only one row (no wrapping) | Add `flex-wrap: wrap` first |
| Grid item not spanning full width | Missing `grid-column: 1 / -1` | Add `grid-column: 1 / -1` |
| `em` sizes unexpectedly large | `em` compounds through parent chain | Switch to `rem` |

---

## References

- [Learn Shay Howe — HTML & CSS](https://learn.shayhowe.com/)
- [MDN Web Docs](https://developer.mozilla.org/en-US/)
- [CSS Reference — cssreference.io](https://cssreference.io/)
- [CSS Tricks — Box Model](https://css-tricks.com/the-css-box-model/)
- [CSS Tricks — Flexbox Guide](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [CSS Tricks — Grid Guide](https://css-tricks.com/snippets/css/complete-guide-grid/)
- [CSS Tricks — Media Queries](https://css-tricks.com/css-media-queries/)
- [Flexbox Froggy — Game](https://flexboxfroggy.com/)
- [CSS Grid Garden — Game](https://cssgridgarden.com/)
- [MountBlue — CSS Layout Drills](https://gitlab.com/mountblue/js/css-layout-drills)
- [MountBlue — Responsive Drills](https://gitlab.com/mountblue/js/responsive-layout-drill)

---
