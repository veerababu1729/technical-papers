## 23-11-2026 - AMA Answers - Veerababu Palepu  

**1. Types of selectors in CSS**
- Universal: `*`
- Element/type: `p`
- Class: `.card`
- ID: `#header`
- Attribute: `input[type="text"]`
- Group: `h1, h2`
- Combinators: descendant (`div p`), child (`div > p`), adjacent sibling (`h1 + p`), general sibling (`h1 ~ p`)
- Pseudo-class: `a:hover`
- Pseudo-element: `p::first-line`

**2. Placeholder vs input field**
- Input field: the box where the user types, and its value is submitted with the form.
- Placeholder: hint text shown inside an empty input (e.g. "Enter email"). It disappears when typing starts and is **not** submitted as a value.
```html
<input type="email" placeholder="Enter email">
```

**3. What is flexbox**
A CSS layout model for arranging items in **one dimension** (row or column). It handles alignment, spacing, and distribution of space, even when item sizes are unknown.
```css
.container { display: flex; justify-content: center; align-items: center; }
```

**4. How to make text italic**
```html
<i>Italic</i>  <em>Emphasized (italic)</em>
```
```css
p { font-style: italic; }
```

**5. What is a programming language**
A formal language used to write instructions that a computer can execute (e.g. Python, Java, C). It has syntax and rules, and is compiled or interpreted into machine code. HTML and CSS are not programming languages; HTML is a markup language and CSS is a style sheet language.

**6. How to make text important**
```html
<strong>Important text</strong>
```
`<strong>` marks strong importance (usually bold, and screen readers can stress it). `<b>` is just visual bold, and `<em>` is emphasis.
In CSS, `!important` raises a declaration's priority, which is different.

**7. What are media queries**
CSS rules that apply only when conditions are met (screen width, orientation, dark mode, etc.). They are the base of responsive design.
```css
@media (max-width: 600px) {
  .menu { display: none; }
}
```

**8. What is `flex-flow`**
Shorthand for `flex-direction` + `flex-wrap`.
```css
.container { flex-flow: row wrap; }
/* same as */
flex-direction: row; flex-wrap: wrap;
```

**9. Difference between HTML and CSS**
- HTML: structure and content (headings, paragraphs, images, links)
- CSS: presentation (colors, fonts, spacing, layout)

HTML is the skeleton and CSS is the styling.

**10. Difference between HTML and HTML files**
- HTML: the markup language itself (the tags and rules).
- HTML file: a document saved with `.html` or `.htm` that contains HTML code, which the browser opens and renders.

**11. What is `<bdo>`**
"Bi-directional override". It forces the text direction, left-to-right or right-to-left, using the `dir` attribute.
```html
<bdo dir="rtl">Hello</bdo>  <!-- shows: olleH -->
```

**12. How to check type in Python**
```python
x = 10
print(type(x))              # <class 'int'>
print(isinstance(x, int))   # True
```

**13. Difference between flex and grid**
- Flex: one-dimensional (row **or** column), content-driven. Good for navbars and components.
- Grid: two-dimensional (rows **and** columns), layout-driven. Good for page layouts and card grids.

They are often used together: Grid for the page, Flex inside components.

**14. How many list tags are there**
Three types of lists:
- `<ul>`: unordered (bullets)
- `<ol>`: ordered (numbers)
- `<dl>`: description list

Related item tags: `<li>` (for ul/ol), `<dt>` and `<dd>` (for dl).

**15. Why do we need responsive layouts**
- Users visit on phones, tablets, laptops, and large screens.
- One layout that fits all avoids horizontal scrolling and tiny, unreadable text.
- It gives better user experience, better SEO (Google favors mobile-friendly sites), and one codebase instead of separate sites.

**16. What is `align-items`**
A flex/grid property that aligns items along the **cross axis** (vertical in a row layout).
Values: `stretch` (default), `flex-start`, `flex-end`, `center`, `baseline`.
```css
.container { display: flex; align-items: center; }
```
