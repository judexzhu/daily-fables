# Applying the redesign — daily-fables

Built against your actual source, so **no post edits and no front-matter changes
are needed.** The layouts read the fields you already use: `title_en`,
`title_zh`, `concept`, `illustration`, `youtube_id`, `tags`, and the
`<section class="zh">` / `<section class="en">` body blocks.

## Files

| File | Action |
|---|---|
| `_layouts/default.html` | replace — now carries the nav, the site-wide language switch, and the footer |
| `_layouts/fable.html` | replace — full-bleed illustration hero, overlapping title block, video, prose, prev/next |
| `_layouts/page.html` | new — used by About and Concepts |
| `index.html` | replace — hero for today's fable, six art plates, then a scannable list |
| `assets/style.css` | replace |
| `concepts.md` | new — `/concepts/`, grouped by your existing `tags` |
| `about.md` | new — `/about/` |

Unzip over the repo root; the paths already match. Then `bundle exec jekyll serve`.

## What changed structurally

- **The language toggle moved from the post to the site.** It now sets
  `data-lang` on `<html>` and persists in `localStorage` under the same
  `fable-lang` key you were already using, so returning readers keep their
  choice. The per-post toggle and its inline script are gone from
  `fable.html`; the CSS hides `section.zh` / `section.en` based on the root
  attribute. Chrome, titles, and section labels switch with it.
- **Chinese titles come from `title_zh`**, which every post already has.
- **The concepts page groups by `tags`.** Tag order and the English/Chinese
  labels are two lists at the top of `concepts.md` — add a tag there when you
  introduce one; unlisted tags simply don't get a group.
- **Illustrations are now the hero**, not a thumbnail. Posts without one
  degrade to a plain dark band — no fallback image needed.
- **The metaphor mapping list** picks up the panel treatment automatically:
  the CSS styles `.prose ul`, and italics in a list item render as monospace,
  so `image → *mechanism*` lines line up without touching the markdown.

## Two things to check

- `_config.yml`: nothing required, but the footer and coffee block hardcode
  `buymeacoffee.com/judexzhu` and the GitHub URL — move them to config if you
  prefer.
- The old `assets/style.css` is fully replaced. Diff it if you had rules for
  anything outside these pages.
