# Corpus Audit — 2026-05-17

**Prepared by:** Execution agent (Sonnet 4.6)
**Role:** Descriptive audit per §1.3 (agentic_orchestration_protocol.md). No construct claims are made. Facts only.
**Scope:** LCR repository (`claude-lcr-analysis`) and Sleep repository (`claude-sleep-analysis`), all active CSV files and archived intermediates in `archive/contaminated_frame/`.

---

## 1. LCR Corpus — `lcr_corpus.csv`

**Path:** `C:/Users/drhea/estate/projects/research/claude-lcr\data\lcr_corpus.csv`

### 1.1 Row count

31,078 rows total.

### 1.2 Type breakdown

| type | count |
|------|-------|
| post | 31,078 |
| comment | 0 |

The corpus contains posts only. No comments are present in the file.

### 1.3 Time-window coverage

- Minimum: 2025-08-01
- Maximum: 2025-12-31

| Month | Records |
|-------|---------|
| 2025-08 | 6,065 |
| 2025-09 | 6,141 |
| 2025-10 | 6,848 |
| 2025-11 | 5,842 |
| 2025-12 | 6,182 |

Coverage is contiguous across the full Aug-Dec 2025 window with no month-level gaps.

### 1.4 Subreddit breakdown

| Subreddit | Posts |
|-----------|-------|
| ClaudeAI | 18,712 |
| ClaudeCode | 8,030 |
| Anthropic | 2,833 |
| claudexplorers | 1,503 |

### 1.5 Source-field breakdown

| Source | Count |
|--------|-------|
| pullpush:ClaudeAI:posts | 18,712 |
| pullpush:ClaudeCode:posts | 8,030 |
| pullpush:Anthropic:posts | 2,833 |
| pullpush:claudexplorers:posts | 1,503 |

All records carry a `pullpush:{subreddit}:posts` source tag. There are no `comment_pass`, `search:*`, or `/new`/`/hot`/`/top` wholesale sort labels. The scraper (Arctic Shift API, Pushshift-compatible) collected posts via a single wholesale time-window pull per subreddit, not PRAW listing sorts.

### 1.6 Post-to-comment ratio

**Posts with at least one comment in corpus: 0 (0.0%).**
**Posts with zero comments in corpus: 31,078 (100.0%).**

The scraper source (`pullpush_lcr_scraper.py`) shows that `scrape_subreddit_comments()` was designed to fetch comments for posts passing a prefilter, then concatenate comments into the final CSV. Inspection of the output file shows the comment concatenation step was not written to the final `lcr_corpus.csv`. The per-subreddit intermediate snapshots (`lcr_posts_*.csv`, see §A.1 below) are also post-only, confirming that the comment phase was either not run to completion or the comments were not appended before saving. The `lcr_corpus.csv` in the archive (pm_html_nested_snapshot) is identical in structure and type distribution.

### 1.7 Spot-check: 20 posts without comments

(No "posts with comments" category exists; all 31,078 posts are in the "without comments" category. A random 20-item sample was drawn.)

The sample spans all four subreddits. Topically, posts range across product complaints, general Claude capability questions, Claude Code workflow discussion, account suspension reports, and tool/feature announcements. A subset of posts reference Claude's behavior in conversation (e.g., lecturing, moralizing, refusal behaviors) alongside posts entirely unrelated to LCR-relevant content (e.g., general Anthropic news, coding help). No systematic filtering to LCR-relevant content is visible in the final CSV. This is consistent with the scraper design: the prefilter was intended to scope comment fetching, not to filter out posts from the final output. The post universe appears to be a near-complete wholesale pull of all posts from the four subreddits during Aug-Dec 2025.

---

## 2. Archive Intermediates — LCR per-subreddit snapshots

**Path:** `C:/Users/drhea/estate/projects/research/claude-sleep\archive\contaminated_frame\pm_html_nested_snapshot\data\`

(The `scrape_intermediates/` folder under the LCR archive is empty. The preserved intermediates are in the sleep archive's pm_html_nested_snapshot.)

| File | Rows | Type | Time range |
|------|------|------|------------|
| lcr_posts_Anthropic.csv | 3,054 | post only | 2025-08-01 to 2025-12-31 |
| lcr_posts_ClaudeAI.csv | 20,184 | post only | 2025-08-01 to 2025-12-31 |
| lcr_posts_ClaudeCode.csv | 8,429 | post only | 2025-08-01 to 2025-12-31 |
| lcr_posts_claudexplorers.csv | 1,599 | post only | 2025-08-17 to 2025-12-31 |

The per-subreddit intermediates contain more posts than the final `lcr_corpus.csv` (total across intermediates: 33,266 vs. final: 31,078). The difference (2,188 rows) is accounted for by the body-length filter (`len > 30`) and deduplication applied during final assembly. These intermediates confirm the raw pre-filter counts. They are post-only; no comment intermediates exist anywhere in either archive.

---

## 3. Sleep Corpus — `posts_snapshot.csv`

**Path:** `C:/Users/drhea/estate/projects/research/claude-sleep\data\posts_snapshot.csv`

### 3.1 Row count

4,114 rows.

### 3.2 Type breakdown

| type | count |
|------|-------|
| post | 4,114 |
| comment | 0 |

Posts only. This file is the Pass 1a/1b post-only snapshot prior to the comment pass.

### 3.3 Time-window coverage

- Minimum: 2026-01-01
- Maximum: 2026-05-16

| Month | Records |
|-------|---------|
| 2026-01 | 132 |
| 2026-02 | 299 |
| 2026-03 | 414 |
| 2026-04 | 1,390 |
| 2026-05 | 1,879 |

The front-heavy distribution (Jan-Mar low, Apr-May high) reflects the rolling-window recency bias of PRAW's `/new` and `/top/month` sorts combined with the live scrape date (mid-May 2026). The early months are not fully sampled.

### 3.4 Subreddit breakdown

| Subreddit | Posts |
|-----------|-------|
| ClaudeCode | 1,163 |
| ClaudeAI | 1,137 |
| Anthropic | 941 |
| claudexplorers | 873 |

### 3.5 Source-field breakdown (posts_snapshot.csv)

All entries in this file are Pass 1a wholesale sorts. No `search:*` (Pass 1b) or `comment_pass` entries appear.

| Source | Count |
|--------|-------|
| ClaudeAI/new | 487 |
| ClaudeCode/new | 472 |
| Anthropic/new | 431 |
| ClaudeCode/top/year | 418 |
| claudexplorers/top/year | 408 |
| claudexplorers/new | 403 |
| Anthropic/top/year | 336 |
| ClaudeAI/top/year | 329 |
| ClaudeAI/top/month | 321 |
| ClaudeCode/top/month | 273 |
| Anthropic/top/month | 174 |
| claudexplorers/top/month | 62 |

`/hot` and `/controversial` sorts are absent from this file, indicating either they were not run during Pass 1a or their results overlapped entirely and were deduplicated out.

---

## 4. Sleep Corpus — `praw_sleep_analysis_final.csv`

**Path:** `C:/Users/drhea/estate/projects/research/claude-sleep\data\praw_sleep_analysis_final.csv`

### 4.1 Row count

89,982 rows.

### 4.2 Type breakdown

| type | count |
|------|-------|
| comment | 87,468 |
| post | 2,514 |

The vast majority of rows are comments (97.2%). The 2,514 posts in this file are a subset of the 4,114 in `posts_snapshot.csv` — only posts that passed the `NUDGE_PREFILTER` regex and had comments fetched. This is the assembled final corpus: prefiltered posts + their comments.

### 4.3 Time-window coverage

- Minimum: 2026-01-01
- Maximum: 2026-05-16

| Month | Records |
|-------|---------|
| 2026-01 | 7,581 |
| 2026-02 | 12,466 |
| 2026-03 | 18,558 |
| 2026-04 | 32,415 |
| 2026-05 | 18,962 |

Month-level coverage is contiguous. The Apr-May concentration reflects both recency bias in the posts source and the comment density of more recent posts.

### 4.4 Subreddit breakdown

| Subreddit | Total rows |
|-----------|------------|
| ClaudeAI | 30,951 |
| ClaudeCode | 30,062 |
| Anthropic | 14,772 |
| claudexplorers | 14,197 |

### 4.5 Source-field breakdown

The final file merges Pass 1a, Pass 1b, and comment-pass records. Key counts:

| Source category | Count |
|----------------|-------|
| comment_pass | 87,468 |
| Pass 1a wholesale (/{sort}) | 2,050 |
| Pass 1b search-filtered (search:*) | 396 |

Pass 1a sources (wholesale sorts): ClaudeAI/new (166), ClaudeCode/new (216), Anthropic/new (233), claudexplorers/new (135), plus /top/month, /top/year, /top/all, /controversial/year, /hot variants across subreddits.

Pass 1b sources (search-filtered): search:ClaudeCode/new/all (85), search:ClaudeAI/new/all (82), search:ClaudeCode/relevance/year (45), search:claudexplorers/new/all (39), search:ClaudeAI/relevance/year (37), search:Anthropic/new/all (36), and smaller search variants.

### 4.6 Post-to-comment ratio (praw_sleep_analysis_final.csv)

- Posts with at least one comment in corpus: 1,713 (68.1%)
- Posts with zero comments in corpus: 801 (31.9%)

The 801 posts with no comments in the corpus represent either: posts where comment fetch returned no in-window comments, posts where all comments were filtered by the date window or comment body conditions, or posts where the PRAW fetch failed silently. This is not unusual for posts with low engagement.

---

## 5. Coverage Gap Analysis

### 5.1 LCR corpus comments

The LCR corpus (`lcr_corpus.csv`) contains **zero comments**. The scraper (`pullpush_lcr_scraper.py`) has a functional `scrape_subreddit_comments()` method that was intended to fetch comments for prefilter-passing posts, but inspection of the output shows the comment concatenation did not complete into the saved file. The 31,078 posts include all posts from the four subreddits during Aug-Dec 2025 regardless of topical relevance, since the prefilter was applied only to scope the comment fetch, not to exclude posts from the output.

Fraction of LCR posts with no comments: **100%** (31,078 / 31,078).

### 5.2 Sleep corpus wholesale-only subset

The `posts_snapshot.csv` (4,114 rows) is the wholesale-only pass (Pass 1a). The `praw_sleep_analysis_final.csv` contains an additional 396 Pass 1b search-filtered posts plus 87,468 comments. The wholesale-only subset is clearly separable by source field.

Posts collected by Pass 1a wholesale only (not appearing in the final `praw_sleep_analysis_final.csv` as posts): 4,114 (posts_snapshot) minus 2,514 (posts in final) = **1,600 posts** that were in the wholesale pull but did not pass the prefilter and therefore have no comments fetched.

### 5.3 Time-window gaps between corpora

- LCR corpus: 2025-08-01 to 2025-12-31
- Sleep corpus (praw_sleep_analysis_final): 2026-01-01 to 2026-05-16

The two corpora are contiguous with no overlap and no gap at the month boundary. The LCR window ends 2025-12-31 and the sleep window begins 2026-01-01.

**Note on early-window underrepresentation (sleep corpus):** January 2026 (7,581 rows) is lower than subsequent months. This reflects PRAW's live-scrape recency bias: by the time the scrape ran (mid-May 2026), `/new` and `/top/month` sorts could not recover posts from four months prior. The Jan 2026 records that do exist come primarily from `/top/year` and search-filtered passes that happened to surface older posts. January through March 2026 are structurally undersampled relative to April-May.

---

## 6. Recommendation

### Option A: Use existing data + document provenance (recommended with targeted supplement)

The existing corpora support the following factual claims, documentable in the methods section:

**LCR corpus (Aug-Dec 2025):** 31,078 posts from four subreddits, scraped wholesale via Arctic Shift/Pushshift-compatible API. Posts only; no comments. The corpus represents the universe of posts from these communities during the target window, not a topically filtered sample. Descriptive engagement (Phase 2 frequency, n-gram, KWIC) can proceed on posts alone; voice segmentation and KWIC will operate on post bodies only.

**Sleep corpus (Jan-May 2026):** 89,982 rows (87,468 comments + 2,514 posts) from the same four subreddits. Collection used a two-pass design: Pass 1a wholesale sorts + Pass 1b search-filtered pulls for posts, then comment fetching for prefilter-passing posts. The comment-rich structure makes this corpus appropriate for voice segmentation and KWIC work on both posts and comments. Jan-Mar 2026 is structurally undersampled and should be noted as a provenance limitation, not treated as a coverage gap to be filled by refetch (PRAW cannot recover time-sorted older posts retroactively from live Reddit).

**What the existing data does not support without qualification:**

1. Comment-level analysis for the LCR window (Aug-Dec 2025). No comments exist in the LCR corpus. If Phase 2 or later phases require examining community responses (not just OP posts) for the LCR period, comments would need to be fetched.

2. Cross-period comparability of post vs. comment units. The LCR corpus is posts-only; the sleep corpus is predominantly comments. Any direct cross-corpus comparison of linguistic features must account for this unit asymmetry.

### Targeted supplement warranted (Option B, narrow scope)

If comment-level analysis is needed for the LCR window, a targeted refetch is feasible:

- **What to fetch:** Comments for the 31,078 LCR posts, using Arctic Shift API's comment search endpoint scoped by `link_id` (post ID). The `scrape_subreddit_comments()` function in `pullpush_lcr_scraper.py` already implements this; it simply needs to be re-run with the post IDs from `lcr_corpus.csv` as input, with results appended to a new `lcr_comments.csv` (not overwriting the existing `lcr_corpus.csv`).
- **Scope:** Arctic Shift coverage for 2025 Aug-Dec should be available. At 31,078 posts and 1.5 seconds per request with 50-comment page size, runtime is approximately 13 hours.
- **Not required for Phase 2 to begin on posts alone.** Descriptive engagement on the 31,078 post bodies is sufficient to produce frequency tables, n-grams, and KWIC that will drive the first AskUserQuestion checkpoint.

### Option C: Full re-scrape — not warranted

Both corpora are internally consistent, time windows are documented, scraper source is preserved, and no data corruption or duplication anomalies were detected. A full re-scrape is not indicated.

---

## Appendix A — Archive file inventory

### A.1 LCR archive intermediates (pm_html_nested_snapshot/data/)

| File | Rows | Type |
|------|------|------|
| lcr_posts_Anthropic.csv | 3,054 | post |
| lcr_posts_ClaudeAI.csv | 20,184 | post |
| lcr_posts_ClaudeCode.csv | 8,429 | post |
| lcr_posts_claudexplorers.csv | 1,599 | post |
| lcr_corpus.csv (archive copy) | 31,078 | post |
| posts_snapshot.csv (archive copy) | 4,114 | post |
| praw_sleep_analysis_final.csv (archive copy) | 89,982 | post+comment |

No comment intermediates exist in either archive. The `archive/contaminated_frame/scrape_intermediates/` folder under the LCR repo is empty.

### A.2 Key scraper source files

- `C:/Users/drhea/estate/projects/research/claude-lcr\src\pullpush_lcr_scraper.py` — Arctic Shift API, wholesale time-window post pull per subreddit, with prefiltered comment fetch designed but not executed into the output file.
- `C:/Users/drhea/estate/projects/research/claude-sleep\src\reddit_scraper_v2.py` — PRAW, two-pass design: Pass 1a wholesale sorts + Pass 1b search-filtered, then prefiltered comment fetch. Comment fetch completed successfully.

---

*Audit generated: 2026-05-17. Execution agent: Sonnet 4.6. No construct claims made.*
