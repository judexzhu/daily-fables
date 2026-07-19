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
illustration: /assets/art/YYYY-MM-DD-<concept-slug>.jpg
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
Each fable gets one AI-generated illustration, same filename stem as the post, generated via the `nanobanana` MCP `generate_image` tool (Gemini image model). Generate it once — don't iterate or regenerate to pick a favorite.

- Prompt describes the STORY scene only (setting, characters, action, mood) — never the technical concept, no diagrams, no arrows, no labels, no text in the image.
- Style: flat papercut/woodcut storybook illustration, warm palette (paper cream, warm brown ink, muted brown, terracotta/ochre accents).
- `aspect_ratio: "16:9"`.
- Generate to a local temp path, then compress with ImageMagick before publishing: `convert input.png -resize 800x450 -quality 65 output.jpg`, aiming for under ~60KB so the base64 payload stays small.
- Reference in front matter: `illustration: /assets/art/<same-stem>.jpg`.
