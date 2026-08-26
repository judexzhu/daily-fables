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
youtube_id: "<YouTube video ID, omit if no video>"
---
<section class="zh" markdown="1">
中文正文（不含标题行）…

### 这是什么

概念解释…

### 为什么重要

重要性说明…

_隐喻对应表_

- 故事元素 → 概念元素
</section>
<section class="en" markdown="1">
English body (without the title line)…

### What it is

Concept explanation…

### Why it matters

Why it matters…

_Metaphor mapping_

- story element → concept element
</section>
```

## Writing rules
- No 每日寓言/Daily Fable prefix in titles; don't spoil the concept term in titles.
- Section labels (`这是什么`, `为什么重要`, `What it is`, `Why it matters`) are `### h3` headings, NOT bold text.
- `**bold**` is for in-story emphasis only, never for section labels.
- Mapping-table lines become markdown list items `- `, blank line before the first item.
- `_italic_` and backtick code spans work in kramdown unchanged. Escape double quotes in front-matter strings.

## Illustration (assets/art/)
Each fable gets one AI-generated illustration, same filename stem as the post, generated via the `nanobanana` MCP `generate_image` tool.

- Prompt describes the STORY scene only (setting, characters, action, mood) — never the technical concept, no diagrams, no arrows, no labels, no text in the image.
- Style: Ink wash painting (水墨画) with 点睛之笔 color accents. Predominantly monochrome with selective vermillion red. Bold dark strokes, strong contrast, wet-on-dry brushwork. Male characters wear wuxia (武侠) style hanfu. Female characters wear elegant flowing Tang Dynasty dress (飘逸唐装).
- `model_tier: "nb2"`, `resolution: "2k"`, `aspect_ratio: "16:9"`, `enable_grounding: false`.
- Compress with ImageMagick: `magick input.png -resize 1920x1080 -quality 75 output.jpg`.
- Reference in front matter: `illustration: /assets/art/<same-stem>.jpg`.
