# GMS FAQ — split into blocks

The original single-file `GMS.html` + `GMS.css` has been divided into
independent, reusable pieces, mirroring how the Aanandham site organizes
its blocks (one file per section, styles and behavior scoped to match).

## Structure

```
gms-blocks/
├── base.html                 # page shell: loads block CSS, marks include points, loads block JS
├── assembled_preview.html    # flattened copy of base.html for double-click preview (no server needed)
├── blocks/
│   ├── page_title.html       # <h1> heading — one per page
│   ├── faq_item.html         # pattern/example for ONE accordion row (copy this to add a question)
│   ├── faq_accordion.html    # the real list of 6 questions, each following faq_item.html's shape
│   └── scroll_top.html       # floating scroll-to-top button
├── css/
│   ├── base.css               # reset + global body styles
│   ├── page_title.css         # .faq-title
│   ├── faq_item.css           # .faq-item, .faq-question, .faq-icon, .faq-answer
│   └── scroll_top.css         # .scroll-top
└── scripts/
    ├── include.js             # tiny fetch-based loader that stitches blocks/*.html into base.html
    ├── faq_accordion.js       # open/close behavior for the accordion block only
    └── scroll_top.js          # click behavior for the scroll-top block only
```

## Why split this way

- **One file per visual section** (title / accordion / scroll button), each
  with its own CSS and, where relevant, its own JS — so you can drop any
  one block onto a different page without dragging the others along.
- **`faq_item.html` is kept separate from `faq_accordion.html`** even
  though there's no server-side templating here: it documents the
  repeating pattern (question button + icon + answer) so adding a 7th
  question means copying one block's markup, not reverse-engineering the
  structure from the middle of a long file.
- **CSS is split to match the HTML blocks**, not by property type, so
  editing one block's look never means scrolling through an unrelated
  block's rules.
- **JS is split per block's behavior** (`faq_accordion.js` only touches
  `.faq-item`/`.faq-question`; `scroll_top.js` only touches `.scroll-top`),
  so removing the scroll button doesn't require touching the accordion
  script, and vice versa.

## Two ways to view it

- **`base.html`** — the real modular version. `scripts/include.js` fetches
  each `blocks/*.html` file at runtime and injects it. This needs the
  folder served over http(s) (e.g. `npx serve .`, or VS Code's "Live
  Server" extension) — browsers block `fetch()` on `file://` URLs.
- **`assembled_preview.html`** — a flattened, hand-copied version of the
  same output for quickly opening by double-click, with no server. If you
  edit a block, re-copy the change into this file too (or just use
  `base.html` via a local server and skip this file).

## Adding a new FAQ question

1. Copy the `.faq-item` block from `blocks/faq_item.html`.
2. Paste it into `blocks/faq_accordion.html`, in the position you want.
3. Replace the placeholder question/answer text.

No CSS or JS changes needed — both already apply to any element with the
`.faq-item` / `.faq-question` / `.faq-answer` classes.
