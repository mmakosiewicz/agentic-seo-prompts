# Agentic SEO — Agent A workflow prompts

Companion repo for the article **["What is agentic SEO and how do you get started?"](https://ahrefs.com/blog/agentic-seo/)**. Each section is a copy/paste prompt that recreates one of the eight workflows in the post (plus a Bonus).

Every prompt:
- Names the **Agent A skill** it mirrors, so users running Agent A know which prebuilt skill is doing the work behind the scenes.
- Calls out the **1–2 tricks** that make the workflow actually work — the things a generic prompt would miss.
- Ends with a **strict output schema** (Markdown / CSV) so the agent returns something you can paste into a spreadsheet, ticket, or doc — not a wall of text.

Replace anything in **[brackets]** with your own values.

---

## 1. Site Audit Triage (with optional PR fix)

*Maps to workflow 1 in the article. Agent A skills: `ahrefs-site-audit` + `ahrefs-site-audit-fix`.*

Run an Ahrefs Site Audit for **[your domain]**, triage the issue list into a ranked fix queue for this sprint, and — optionally — open a pull request with the fix for the top issue.

**Inputs**
- Domain / crawl scope: **[your domain]**
- Country / language focus: **[country + language, or "global"]**
- Sprint capacity: **[hours or person-days available this sprint]**
- (Optional) Repo / location to change: **[GitHub repo URL + branch, or local path]** — only needed if you want a PR

**Part A — Triage**

1. Pull the latest crawl's health score + full issue list. Note total pages crawled.
2. Score each issue by expected impact on (a) organic traffic and (b) crawl efficiency.
3. **Assign a fix strategy to each issue** (the part that turns a flat list into a real queue):
   - `template` — fix shared layout/template; one edit fixes many pages
   - `per-page` — fix needs individual page context
   - `config` — fix in sitemap, robots.txt, or server config
   - `investigate` — present data; don't auto-fix; needs human call
4. Estimate effort. Drop duplicates / very low-signal items. Consolidate near-duplicates.
5. Return the top 10–15 issues, ranked.

**Output — Markdown table**

| Rank | Issue | Severity | URLs affected | Sample URLs | Fix strategy | Impact (traffic) | Impact (crawl) | Effort | Recommended fix | Acceptance criteria |
|---|---|---|---|---|---|---|---|---|---|---|

End with a 3-bullet "**Why these and not others**" rationale.

**Part B — Fix in code (optional)**

If a repo is provided, pick the top-ranked issue and:

1. **Detect the tech stack and map the template hierarchy first** (which files control `<head>`, layouts, navigation). Template-first fixes are non-negotiable — one template edit can fix hundreds of pages, while per-page fixes don't scale.
2. Diagnose the root cause inside that template/controller/middleware.
3. Make the smallest safe change. Preserve SEO-critical behavior (redirects, canonical, hreflang, pagination params, robots).
4. Verify: run available tests / build / lint; render an affected page and confirm the fix.

**If no source code access**, switch to *spec mode*: produce a detailed fix spec (affected URLs, what changes, exact HTML/config snippets, priority) that a developer can implement.

**Output — sections in order**

1. **Root cause** (3–6 bullets, ≤ 25 words each)
2. **PR-ready diff** (unified diff; if not possible, a numbered patch plan referencing file paths and line numbers)
3. **Before/after checks** — table:

   | Check | Before | After | How to verify |
   |---|---|---|---|

4. **Rollback plan** (2–4 bullets)
5. **Reviewer checklist** (5–8 yes/no items)

---

## 2. Declining Content Detection

*Maps to workflow 2 in the article. Agent A skill: `declining-content-detection`.*

Weekly job: diagnose pages on **[your domain]** whose organic traffic is declining.

**Inputs**
- Time window: **[3 / 6 / 12 months]** — **12 months is the most reliable** (smooths seasonality, eliminates short-term noise). 3 months catches recent drops; 6 is strategic.
- Optional focus: **[folder / category / template type]**

**Tasks**
1. Pull every page with **≥ 20% traffic decline AND ≥ 50 visits lost** (filters noise from real declines). Compute traffic-value lost using CPC where available.
2. For each top page, classify into one **decline pattern** (this is what makes the queue actionable):
   - **Full rewrite needed** — traffic *and* keywords both dropped proportionally → content is stale
   - **Lost backlinks** — keywords still rank but traffic fell → ref-domain decline
   - **SERP / AI Overview shift** — AI Overview, featured snippet, or new SERP feature is stealing clicks
   - **Competitor pushed you down** — competitor outranks you on the same intent
   - **Cannibalization** — you started ranking multiple URLs on the same keyword
   - **Internal change** — recent site change (template, internal links, robots) caused the drop
3. Give one next action and a fallback. Cap the queue at 10–20 URLs.

**Output — CSV**

```
priority,url,prev_traffic,current_traffic,traffic_change_pct,traffic_value_lost,decline_pattern,evidence,next_action,fallback_action,expected_timeline
```

End with a "**3 biggest patterns this week**" summary (3 bullets max) — pattern-level, not page-level.

---

## 3. Keyword Cannibalization Fix

*Maps to workflow 3 in the article. Agent A skill: `keyword-cannibalization`.*

Find and fix keyword cannibalization on **[your domain]**.

**Inputs**
- Scope: **[subfolder or "entire domain"]**
- Priority goal: **[max organic traffic / lift rankings for X / reduce index waste]**

**Tasks**
1. Cannibalization isn't surfaced by a single Ahrefs call — it takes a **two-pass discovery**:
   - **Pass 1**: pull the domain's organic keywords; find the top ~20 URLs by keyword count.
   - **Pass 2**: for each of those URLs, pull *its* organic keywords. Merge results — keep every keyword that **2+ URLs from your domain rank for**.
   - Sort by search volume.
2. Cluster the cannibalized keywords into groups (one group = one intent owned by multiple URLs).
3. Pick a canonical winner per group using ranking/traffic, topical relevance, authority/link strength, content completeness.
4. Draft a consolidation plan: merge, redirect, de-optimize.

**Output — table per group + summary CSV**

For each group:

```
### Group: <intent / theme>
Keywords (top 5–20, with volumes): …
Winner URL: …
Loser URLs (with current positions): …
Decision rationale: …
Action plan:
  - Merge: …
  - Redirect: …
  - De-optimize: …
Risks / what to verify: …
```

Summary CSV:

```
group_id,intent,winner_url,loser_urls,action_type,estimated_traffic_impact,confidence
```

---

## 4. Trending Keyword Research

*Maps to workflow 4 in the article. Agent A skill: `trending-keyword-ideas`.*

Trend-hunt for **[your topic area / seed keyword]**.

**Inputs**
- Seed: **[1–3 core keywords or short topic sentence]**
- Target market: **[country + language]** (or "global")
- Growth threshold: **25% over the last 3 months** (default)
- Min volume: **[500/month default]**
- (Optional) Your domain — to flag which trending keywords you *already* rank for

**Tasks**
1. **Cast a wide net via ~20 semantic expansions of the seed.** Don't just pull "matching terms" — expand into synonyms, near-synonyms, adjacent intents, and subtopics competitors cover. Aim for 1,000–3,000 candidate keywords. (Generic prompts pull ~100; you'll miss most of the trend space.)
2. Pull **monthly volume history** for the full set (real Ahrefs data, no estimates), compute 3-month growth.
3. Filter: ≥ growth threshold, ≥ min volume. Drop branded (unless requested) and local-only (unless local target).
4. Cluster into themes.
5. (If domain provided) flag which keywords you already rank for vs. green-field opportunities.

**Output — themes + flat CSV**

For each theme:

```
### Theme: <name>
Why it's heating up: <1–2 sentences>
Suggested content angle: <who it's for + what to say>
Representative keywords (5–15, with volume + growth %): …
Already ranking?: <count if applicable>
```

Flat CSV of all kept keywords:

```
keyword,volume,kd,growth_3m_pct,theme,intent,parent_topic,you_already_rank
```

Sorted: `theme` asc, `growth_3m_pct` desc.

---

## 5. Programmatic SEO Keywords

*Maps to workflow 5 in the article. Agent A skill: `programmatic-seo-keywords`.*

Find programmatic SEO opportunities for **[your domain]**.

**Inputs**
- Domain: **[your domain]**
- Target market: **[country + language]**
- Preferred intent: **[commercial / informational / mixed]**

**Tasks — niche-first, not pattern-first**

1. **Start by analyzing the domain's existing rankings.** Pull the top organic keywords (3+ word phrases reveal patterns better) — extract themes from where the site already has traction.
2. From those themes + the domain's niche, **brainstorm 10–20 core topics** that could have pSEO potential. Think: what problems does the site solve? what would users search for by location / product / company / role?
3. *Only then* identify templatable patterns from those topics (e.g. `[X] in [city]`, `[A] vs [B]`, `[role] salary in [country]`, `best [X] for [Y]`).
4. For each pattern: pull the variant list with volume + difficulty + intent. Propose a content model. Flag duplicate-intent / cannibalization risk against the site's existing pages.

(Starting from the site's actual niche — instead of guessing patterns — is what stops you from drafting templates the site has no authority to rank for.)

**Output — pattern blocks + master CSV**

For each pattern:

```
### Pattern: <pattern>
Audience + intent: …
Example variants (5–10, with volumes): …
Template structure (sections): …
Required dynamic fields: …
Uniqueness rules (anti-thin-content): …
Internal linking plan: …
Risks (cannibalization, thin content): …
```

Master CSV:

```
pattern,variant_keyword,volume,kd,intent,estimated_traffic_potential,recommended_template
```

Sorted by `pattern, estimated_traffic_potential desc`.

---

## 6. AI Mention Gap Analysis

*Maps to workflow 6 in the article. Agent A skill: `ai-mention-gap-analysis`.*

AI mention gap for **[your brand]** in **[your topic area]**.

> **Requires Ahrefs Brand Radar** (paid Ahrefs tier). Before running, confirm a Brand Radar report exists in your Ahrefs workspace — without it, this workflow can't surface the prompts where AI systems mention competitors. If you don't have it, set one up at `app.ahrefs.com → Brand Radar` first.

**Inputs**
- Competitors: **[competitor 1], [competitor 2], [competitor 3]** (3–10)
- Target market: **[country / language if relevant]**
- Your positioning: **[1 paragraph on what you want to be known for]**
- AI platforms to analyze: **[Google AI Overviews / ChatGPT / Perplexity / Gemini / Copilot / Google AI Mode]** — pick one or more (run cross-platform if multiple, results often differ per platform)

**Tasks**
1. Pull prompts/questions where competitors appear in AI answers, per platform.
2. For each prompt, mark whether your brand is **present / partial / absent**.
3. Prioritize by prompt demand × competitor frequency × positioning fit.
4. For each top gap, propose how to earn the mention (content angle + target page type).

**Output — CSV** (one row per prompt; if multiple platforms, add `platform` column)

```
platform,prompt,competitors_mentioned,my_brand_status,prompt_demand,priority_score,gap_reason,recommended_content_angle,target_page_type,success_criteria
```

Sort by `priority_score desc`. Cap at 10–30 rows per platform.

If multiple platforms: end with a **cross-platform comparison** — which gaps appear everywhere vs. which are platform-specific.

---

## 7. AI Citation Freshness Audit

*Maps to workflow 7 in the article. Agent A skill: `citation-freshness-audit`.*

AI citation freshness for **[your domain]** in **[your topic area]**.

> **Requires Ahrefs Brand Radar** (paid Ahrefs tier) for the cited-page inventory.

**Inputs**
- Domain / URL section: **[ahrefs.com, or ahrefs.com/blog/]**
- Citation mode: **`brand_mentions`** (default — pages cited in AI responses that mention your brand) or **`all_from_domain`** (every page cited from your domain, even when the brand isn't mentioned — catches data studies, original research, guides cited for generic queries)
- AI platforms: **[AIO / AI Mode / ChatGPT / Perplexity / Gemini / Copilot]** — recommend all 6

**Tasks**

1. Pull the inventory of cited pages from Brand Radar (URL + citation count + traffic). **Don't trust Brand Radar's date fields for freshness** — they come from Content Explorer, which only detects ~10% of actual content updates. Most pages show their original publish date even if updated.
2. **Extract real update dates from the HTML** for the top 100–200 cited pages by traffic. Different sites store dates differently (JSON-LD, `itemprop`, `__NEXT_DATA__`, visible body text) — discover what signals exist per section, then choose which to trust (visible body / `itemprop` text is usually the editorial date; JSON-LD `dateModified` often falsely fresh from plugin saves). For long-tail pages, fall back to Brand Radar dates.
3. **Filter out non-articles** — pagination (`?page=2`), category/tag pages, dynamic tool pages, homepage. Focus on content sections (blog, docs, guides) unless the user wants product pages too.
4. Score: `priority = traffic × months_since_update`. Rank by priority. Recommend **refresh / replace / retire** per page.

**Output — CSV**

```
cited_url,domain,citation_count,traffic,html_update_date,date_source,months_since_update,freshness_label,priority_score,recommended_action,effort,expected_ai_visibility_impact
```

`freshness_label ∈ {fresh, somewhat_stale, stale}` · `date_source ∈ {html_jsonld, html_itemprop, html_visible, brand_radar_fallback}`.

Sort by `priority_score desc`. End with "**Top 10 pages to refresh first**" + 1 sentence each on why.

---

## 8. EEAT Audit

*Maps to workflow 8 in the article. Agent A skill: `brand-authority`.*

E-E-A-T audit for **[your domain]**.

**Inputs**
- Page types to prioritize: **[homepage / category / product / guides / FAQs / location pages]**
- Topic category: **YMYL** (Your Money or Your Life — health / finance / legal / safety) or **non-YMYL** — this changes the bar
- Evidence resources: **[1–2 links or notes about internal research, customer proof, author bios, credentials]**

**Tasks**

1. For each page type, audit **all four pillars** on a 1–5 scale:
   - **Experience** — first-hand involvement (original photos, case studies, "how this was tested" sections)
   - **Expertise** — verifiable knowledge (author bios with credentials, depth of coverage, citations to authoritative sources)
   - **Authoritativeness** — recognition by others (mentions, backlinks from authorities, original research)
   - **Trustworthiness** — accuracy, transparency, legitimacy (changelogs, fact-checking notes, contact info, secure pages)
2. **Apply the YMYL rule**: YMYL topics require *formal* credentials and tighter trust signals. Non-YMYL can lean on "everyday expertise" from extensive practice. State which rule you're applying.
3. **Trust is foundational** — a site that fails trust signals has low E-E-A-T regardless of how experienced or expert it looks. Treat trust gaps as P0.
4. For every gap, propose exactly what to add and where in the page structure (no generic advice like "improve author bios").

**Output — scorecard table per page type**

| Page type | YMYL? | Exp (1–5) | Exp gaps + fixes | Expt (1–5) | Expt gaps + fixes | Auth (1–5) | Auth gaps + fixes | Trust (1–5) | Trust gaps + fixes |
|---|---|---|---|---|---|---|---|---|---|

Add a **"Top 3 fixes this week"** list (trust gaps first if any exist) plus a 1–2 week action plan with owner + deadline columns.

---

## Bonus. Reddit Listener (Demand Discovery)

*Maps to the Bonus workflow in the article. Custom workflow — no prebuilt skill; build your own prompt.*

Monitor Reddit for relevant conversations about **[brand + category]** — summarize what's being said, where, and how you could enter the conversation. Useful for **demand discovery** and **link-building angles** that start with a real thread. Not an outreach automation — it's a listening brief.

**Inputs**
- Subreddits: **[list]**
- Keywords: **[brand / category / pains / competitors]**
- Time window: **[last 7 / 30 days]**

**Tasks**
1. Find high-signal threads (real questions, repeated pains, decision moments; exclude self-promo/spam).
2. For each thread summarize: what's being said, what users have tried, why existing answers fall short.
3. Note one way you *could* enter the conversation (could be a helpful comment, a piece of content worth writing, or a link-building outreach angle — one line, no draft).
4. Categorize each thread: `demand_discovery` / `link_building` / `content_idea`.

**Output — CSV + thread blocks**

CSV header:

```
thread_url,subreddit,what_is_being_said,why_it_matters,opportunity_type,how_to_enter_conversation,priority
```

`opportunity_type ∈ {demand_discovery, link_building, content_idea}`.

Then a Markdown block per top thread:

```
### <thread title>
Subreddit: …
Summary (4–6 bullets — what's being said, where the debate is, what users tried):
Why it matters (signal for demand / link / content idea):
How you could enter the conversation: <one sentence>
```

Cap at 8–20 briefs. End with a "**This week's signals**" recap grouped by `opportunity_type` (3 bullets max per group).
