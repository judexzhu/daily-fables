<img src="assets/seal.png" width="48" alt="寓"> 

# 每日寓言 · Daily Fables

[![Site](https://img.shields.io/badge/site-fables.day-9c3a22)](https://fables.day/)
[![Posts](https://img.shields.io/badge/fables-62+-333)](https://fables.day/)
[![YouTube](https://img.shields.io/badge/YouTube-Daily%20Fables-red)](https://www.youtube.com/@judexzhu)

Graduate-level cloud & distributed-systems concepts — each told as a **bilingual fable** (Chinese + English) that only reveals the concept at the end.

One fable every morning, five minutes with your coffee.

<p align="center">
  <img src="assets/screenshot.png" width="720" alt="Daily Fables — fables.day">
</p>

## How it works

Each fable follows the same structure:

1. A vivid **story** — a teahouse, a marketplace, a village postman — that maps a real distributed-systems mechanism without naming it
2. A **concept reveal** near the end: "By now you've probably recognized it..."
3. A short **technical explanation** (What it is / Why it matters)
4. A **metaphor mapping table** connecting every story element to its technical counterpart

Both Chinese and English versions live in the same post. The site switches languages with one click.

## Art style

All illustrations are AI-generated **Chinese ink wash paintings** (水墨画) with selective vermillion red accents (点睛之笔). Male characters wear wuxia-style hanfu; female characters wear flowing Tang Dynasty dress.

## The pipeline

Every fable goes through a fully automated pipeline:

```
 Concept selection (dedup against 62+ existing posts)
       ↓
 Bilingual fable writing (story-first structure)
       ↓
 Ink wash illustration (nanobanana / Gemini Flash Image)
       ↓
 Cinematic video (NotebookLM)
       ↓
 YouTube upload (youtubeuploader)
       ↓
 GitHub Pages publish (Jekyll → fables.day)
       ↓
 Xiaohongshu carousel (8-slide 3:4, Chinese-only)
       ↓
 Slack notification
```

Orchestrated by Claude Code with the `daily-fable` skill.

## Concepts covered

Distributed systems, Kubernetes internals, networking, Linux, storage, security, and cloud architecture (AWS/GCP).

Browse the full concept index at [fables.day/concepts](https://fables.day/concepts/).

## Tech stack

- **Site**: [Jekyll](https://jekyllrb.com/) on [GitHub Pages](https://pages.github.com/) — custom dark theme, bilingual toggle
- **Illustrations**: [nanobanana](https://github.com/anthropics/nanobanana) (Gemini Flash Image)
- **Videos**: [NotebookLM](https://notebooklm.google/) Cinematic format → [YouTube](https://www.youtube.com/@judexzhu)
- **Cross-posting**: [xiaohongshu-cli](https://github.com/your/xhs-cli) — 8-slide ink wash carousels
- **Orchestration**: [Claude Code](https://claude.ai/code) with custom pipeline skill
- **Analytics**: Google Analytics 4 + Google Search Console
- **SEO**: Open Graph + Twitter Card meta tags, jekyll-sitemap

## Post format

See [POST_FORMAT.md](POST_FORMAT.md) for the fable writing guide and frontmatter schema.

## License

Content (fables and illustrations) is copyright. Source code (layouts, CSS, scripts) is MIT.
