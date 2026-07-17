# Post format for daily-fables

One file per fable in `_posts/`, named `YYYY-MM-DD-<concept-slug>.md` (slug = lowercase English concept, hyphens, e.g. `cgroup-cpu-throttling`).

```
---
layout: fable
title: "<中文标题> · <English Title>"
title_zh: "<中文标题>"
title_en: "<English Title>"
concept: "<English concept name>"
tags: [<1-3 lowercase area tags>]
illustration: /assets/art/YYYY-MM-DD-<concept-slug>.svg
---
<section class="zh" markdown="1">
中文正文（不含标题行）…

_隐喻对应表_

- 故事元素 → 概念元素
</section>
<section class="en" markdown="1">
English body (without the title line)…

_Metaphor mapping_

- story element → concept element
</section>
```

Conversion rules from fable text:
- No 每日寓言/Daily Fable prefix in titles; don't spoil the concept term in titles.
- Mapping-table lines become markdown list items `- `, blank line before the first item.
- `_italic_` and backtick code spans work in kramdown unchanged. Escape double quotes in front-matter strings.

## Illustration (assets/art/)
Each fable gets a hand-drawn SVG scene, same filename stem as the post. Style rules:
- viewBox `0 0 800 450`, flat storybook/woodcut style, no text in the image.
- Palette only: paper `#f6efe2` / `#efe5d2`, ink `#5b4a33` / `#2d2418`, soft ink `#6b5d49` / `#a3947b`, accent `#a4502c` / `#d98a5f` / `#e8b27d`, rule `#d8c9ac`, chip `#e6d8bd`.
- Depict the STORY scene (the village/dock/kitchen…), not the technical concept; no diagrams, no arrows, no labels.
- Simple shapes: polygons, ellipses, a few paths; small human silhouettes allowed; finish with a rounded-rect frame stroke `#d8c9ac` width 3 inset 6px.
- Reference in front matter: `illustration: /assets/art/<same-stem>.svg`.
