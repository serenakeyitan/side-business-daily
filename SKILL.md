---
name: side-business-daily
description: "Side Business Daily — curated money-making ideas from Reddit & Twitter, with interactive step-by-step guides"
metadata: {"clawdbot":{"emoji":"💰","requires":{"bins":["python3"]},"triggers":["搞钱","赚钱","make money","hustle","side hustle","副业","side business"]}}
---

# Side Business Daily (搞钱日报)

A daily-updated collection of real, actionable side business ideas sourced from Reddit and Twitter/X. Each day features 5 curated ideas with source links, difficulty ratings, and startup costs.

## How It Works

### Browsing Mode (Default)

When activated, the skill:

1. **Shows today's edition** from `data/cache/YYYY-MM-DD.md` (Beijing timezone)
2. **Lists available past editions** and asks if you'd like to browse them
3. **Offers deep-dive guides** for select topics (in `data/guides/`)

If today's cache doesn't exist yet, offer to generate it (see Creator Mode).

### Displaying Today's Content

Read today's cache file and present it. Then:

```
📅 今日搞钱日报已就绪！

想看往期内容吗？以下日期可查看：
- 2026-03-10
- 2026-03-09
- ...

也可以输入「攻略」查看详细的赚钱教程列表。
```

### Browsing Past Editions

List all `.md` files in `data/cache/` sorted by date (newest first). Let the user pick a date to view.

### Deep-Dive Guides

Show available guides from `data/playbook.json` as a numbered list. When the user picks one, load and walk through `data/guides/<slug>.md` conversationally, section by section.

---

## Creator Mode (for generating new content)

Activated when the owner explicitly asks to generate today's post or manage guides.

### Generate Today's Edition

1. **Check cache**: `python3 scripts/cache.py check`
   - Exit 0: Show cached content, ask if they want to regenerate
   - Exit 1: Continue to generation

2. **Fetch candidates**: `python3 scripts/run.py`

3. **Curate top 3-5 ideas** — reject MLM, scams, crypto, high-investment schemes

4. **Format as daily edition**:
   - Title line: `搞钱日报 | YYYY-MM-DD`
   - 5 numbered ideas, each with:
     - One-line description
     - 3 bullet points (key insight, how to start, cost/difficulty)
     - Source link
   - Footer: `数据来源：Reddit, Twitter/X`

5. **Save**: pipe the formatted text to `python3 scripts/cache.py save`

6. **Mark used items**: pipe selected items to `python3 scripts/dedup.py mark`

### Add/Update Guides

Create or edit guides in `data/guides/<slug>.md` and update `data/playbook.json`.

---

## Data Files

| File | Purpose |
|------|---------|
| `data/cache/YYYY-MM-DD.md` | Daily editions (7-day retention) |
| `data/guides/<slug>.md` | Detailed step-by-step guides |
| `data/playbook.json` | Index of all available guides |
| `data/seen.json` | 30-day dedup state |

## Scripts

| Script | Purpose |
|--------|---------|
| `scripts/fetch_reddit.py` | Fetch top posts from side-hustle subreddits |
| `scripts/fetch_twitter.py` | Search Twitter via external twitter_client.py |
| `scripts/dedup.py` | 30-day rolling dedup manager |
| `scripts/run.py` | Orchestrator: fetch + dedup + output candidates |
| `scripts/cache.py` | Daily cache: check/save/cleanup (Beijing time) |
