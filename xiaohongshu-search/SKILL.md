---
name: xiaohongshu-search
description: Search Xiaohongshu notes by keyword and summarize useful results with links.
compatibility: opencode
---

# Xiaohongshu Search

## What I do
- Search Xiaohongshu (Xiaohongshu/RED) notes for a user topic.
- Prioritize relevant, recent, and actionable notes.
- Return a structured shortlist with explicit confidence and access status.

## Core constraints
- Xiaohongshu pages may be login-gated, rate-limited, or anti-bot protected.
- Never bypass protections. If direct extraction fails, switch to a transparent fallback mode.

## Extraction modes
- `direct`: Note page is accessible; metadata/content is extracted from the page itself.
- `index_only`: Only search index/snippet data is available; metadata is partial and low confidence.
- `hybrid`: Mix of direct and index-only results.

Always report the mode in `Search Summary`.

## When to use me
- User asks for Xiaohongshu notes on a product, place, lifestyle topic, or trend.
- User wants note ideas, buying references, travel tips, or user-generated experience summaries.

## Inputs
- `query` (required): core keyword(s), in Chinese preferred.
- `intent` (optional): e.g. "种草", "避雷", "攻略", "测评", "价格", "平替".
- `time_range` (optional): e.g. last 7/30/90 days.
- `region` (optional): city/country or local context.
- `result_count` (optional): default 10, recommended 5-20.
- `extraction_mode` (optional): `auto` (default), `direct_only`, `index_only`.

## Query expansion
- Build Chinese variants from user intent and context:
  - Destination/region aliases (e.g. 挪威/北欧/奥斯陆)
  - Task-intent terms (e.g. 签证, 租车, 自驾路线, 住宿, 天气, 预算)
  - Time terms (e.g. 五一, 5月, 最近30天)

## Steps
1. Clarify search objective and convert to Chinese keyword variants.
2. Build multiple search queries, for example:
   - `site:xiaohongshu.com <query> <intent>`
   - `site:xiaohongshu.com/explore <query>`
   - `site:xiaohongshu.com <query> <region>`
3. Collect candidate note URLs from search results.
4. Attempt to open candidate pages and extract available metadata/content snippets when accessible.
5. Assign per-item quality and confidence signals:
   - `access_status`: `accessible` | `login_required` | `blocked` | `not_found` | `unknown`
   - `source_confidence`: `high` (page extracted), `medium` (strong snippet evidence), `low` (weak index evidence)
   - `date_confidence`: `exact` (page date), `inferred` (from note id pattern), `unknown`
6. Score and rank candidates.
7. Filter low-quality or duplicate results; keep high-signal notes.
8. Return concise structured results plus optional follow-up refinements.

## Ranking model (required)
Use a transparent weighted score to avoid generic travel noise.

- Relevance to user topic and intent: 0-40
- Coverage of requested subtopics: 0-20
- Recency (within requested time range): 0-20
- Accessibility/verification quality: 0-15
- Uniqueness/originality signal: 0-5

If two results tie, prefer `accessible` over `index_only`.

## Output format
- `Search Summary`: what was searched and filtering assumptions.
  - Include: `Extraction mode`, `Coverage limits`, and `Confidence notes`.
- `Top Notes` (numbered):
  - `Title`
  - `Author` (if available)
  - `Date` (if available)
  - `URL`
  - `Access status`
  - `Source confidence`
  - `Date confidence`
  - `Why useful` (1 line)
  - `Key points` (1-3 bullets)
- `Next refinement options`: e.g. narrower budget, city, brand, season.

## Minimum quality gate
- If fewer than 30% of returned items are `accessible`, explicitly state that results are partially index-based.
- If direct extraction is mostly blocked, provide a `Collaboration fallback`:
  - Ask user to provide 10-30 accessible share links.
  - Then produce a high-confidence synthesis from those links.

## Quality rules
- Prefer original note links over repost aggregations.
- Mark uncertain fields as "unknown" instead of guessing.
- Distinguish inferred vs extracted values; never present inferred values as exact.
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
- Extraction mode: hybrid
- Coverage limits: 部分笔记仅可获得索引信息
- Confidence notes: 日期字段含 inferred 项

Top Notes
1) 标题...
   - Author: ...
   - Date: ...
   - URL: ...
   - Access status: accessible
   - Source confidence: high
   - Date confidence: exact
   - Why useful: ...
   - Key points: ...

Collaboration fallback（if needed）
- 当前可直接访问结果不足，建议你提供 10-30 条可打开的小红书分享链接，我将输出高置信度汇总。
```
