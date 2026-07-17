# Post format for daily-fables

One file per fable in `_posts/`, named `YYYY-MM-DD-<concept-slug>.md` (slug = lowercase English concept, hyphens, e.g. `cgroup-cpu-throttling`).

```
---
layout: fable
title: "灶台上的那炷香 · The Stove's Incense-Stick Rule"
title_zh: "灶台上的那炷香"
title_en: "The Stove's Incense-Stick Rule"
concept: "cgroup CPU throttling"
tags: [kubernetes, linux]
---
<section class="zh" markdown="1">
中文正文（不含标题行）…

_隐喻对应表_

- 故事元素 → 概念元素
- …
</section>
<section class="en" markdown="1">
English body (without the title line)…

_Metaphor mapping_

- story element → concept element
- …
</section>
```

Conversion rules from Slack message text:
- Drop the leading `:emoji: _每日寓言 · 标题_` / `_Daily Fable · Title_` line; the title (without the 每日寓言/Daily Fable prefix) goes into front matter.
- `\n\n` stays as paragraph break.
- Mapping-table lines starting with `· ` become markdown list items `- `, with a blank line before the first item.
- Slack `_italic_` and `` `code` `` work in kramdown unchanged. Escape any double quotes inside front-matter strings.
- `title` = `title_zh · title_en`.
