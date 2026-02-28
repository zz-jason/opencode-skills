---
name: xiaohongshu-search
description: Search Xiaohongshu notes by keyword and summarize useful results with links.
compatibility: opencode
---

# Xiaohongshu Search

## What I do
- Search Xiaohongshu (Xiaohongshu/RED) notes for a user topic.
- Prioritize relevant, recent, and actionable notes.
- Return a structured shortlist: title, author, date, link, and key takeaway.

## When to use me
- User asks for Xiaohongshu notes on a product, place, lifestyle topic, or trend.
- User wants note ideas, buying references, travel tips, or user-generated experience summaries.

## Inputs
- `query` (required): core keyword(s), in Chinese preferred.
- `intent` (optional): e.g. "种草", "避雷", "攻略", "测评", "价格", "平替".
- `time_range` (optional): e.g. last 7/30/90 days.
- `region` (optional): city/country or local context.
- `result_count` (optional): default 10, recommended 5-20.

## Steps
1. Clarify search objective and convert to Chinese keyword variants.
2. Build multiple search queries, for example:
   - `site:xiaohongshu.com <query> <intent>`
   - `site:xiaohongshu.com/explore <query>`
   - `site:xiaohongshu.com <query> <region>`
3. Collect candidate note URLs from search results.
4. Open candidate pages and extract available metadata/content snippets.
5. Filter low-quality or duplicate results; keep high-signal notes.
6. Return concise structured results plus optional follow-up refinements.

## Output format
- `Search Summary`: what was searched and filtering assumptions.
- `Top Notes` (numbered):
  - `Title`
  - `Author` (if available)
  - `Date` (if available)
  - `URL`
  - `Why useful` (1 line)
  - `Key points` (1-3 bullets)
- `Next refinement options`: e.g. narrower budget, city, brand, season.

## Quality rules
- Prefer original note links over repost aggregations.
- Mark uncertain fields as "unknown" instead of guessing.
- If results are sparse, explicitly report this and broaden query variants.
- Preserve user language (Chinese by default for Chinese queries).

## Safety and compliance
- Respect website terms and robots constraints.
- Do not bypass paywalls, logins, anti-bot protections, or access controls.
- Do not collect private/personal data beyond publicly visible note metadata.

## Example invocation
```text
帮我在小红书搜「大阪亲子酒店」最近30天的攻略，给我10篇最有参考价值的笔记。
```

## Example response skeleton
```text
Search Summary
- Query: 大阪 亲子 酒店 攻略
- Time range: last 30 days
- Intent: 攻略/避坑

Top Notes
1) 标题...
   - Author: ...
   - Date: ...
   - URL: ...
   - Why useful: ...
   - Key points: ...
```
