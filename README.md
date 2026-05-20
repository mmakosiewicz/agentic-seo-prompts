# Agentic SEO — Agent A workflow prompts

Companion repo for the article **["What is agentic SEO and how do you get started?"](https://ahrefs.com/blog/agentic-seo/)**. Each section below is a copy/paste prompt for **Agent A** to recreate one of the eight workflows in the post (plus a Bonus).

Each prompt ends with a **strict output schema** (Markdown / CSV columns) so the agent returns something you can paste into a spreadsheet, ticket, or doc — not a wall of text. Replace anything in **[brackets]** with your own values.

---

## 1. Site Audit Triage (with optional PR fix)

*Maps to workflow 1 in the article.*

Run an Ahrefs Site Audit for **[your domain]**, triage the issue list into a ranked fix queue for this sprint, and — optionally — open a pull request with the fix for the top issue.

**Inputs**
- Domain / crawl scope: **[your domain]**
- Country / language focus: **[country + language, or "global"]**
- Sprint capacity: **[hours or person-days available this sprint]**
- (Optional) Repo / location to change: **[GitHub repo URL + branch, or local path]** — only needed if you want a PR

**Part A — Triage**

1. Pull the full issue list from the most recent crawl.
2. Score each issue by expected impact on (a) organic traffic and (b) crawl efficiency.
3. Estimate effort. Drop duplicates / very low-signal items. Consolidate near-duplicates.
4. Return the top 10–15 issues, ranked.

**Output — Markdown table**

| Rank | Issue | Severity | URLs affected | Sample URLs | Impact (traffic) | Impact (crawl) | Effort | Recommended fix | Acceptance criteria |
|---|---|---|---|---|---|---|---|---|---|
| 1 | … | high / med / low | int | url1, url2 | high / med / low + 1 sentence | high / med / low + 1 sentence | low / med / high | concrete next step | how we know it's fixed |

End with a 3-bullet "**Why these and not others**" rationale.

**Part B — Fix in code (optional)**

If a repo is provided, pick the top-ranked issue and:

1. Diagnose the root cause — find the template/controller/middleware that produces the bad output.
2. Make the smallest safe change.
3. Preserve SEO-critical behavior (redirects, canonical, hreflang, pagination params, robots).
4. Verify: run available tests / build / lint; render an affected page and confirm the fix.

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

*Maps to workflow 2 in the article.*

Weekly job: diagnose pages on **[your domain]** whose organic traffic is declining.

**Inputs**
- Time window: **[last 30 / 60 / 90 days — pick one]**
- Optional focus: **[folder / category / template type]**

**Tasks**
1. Pull losing pages and compute a priority score from: magnitude of drop, intent value, and "fixability".
2. For each top page pick the **single most likely** cause (allow up to 2 contributing causes) from: stale content, lost backlinks, SERP/feature shift, internal changes, cannibalization.
3. Give one next action and a fallback action.
4. Cap the queue at 10–20 URLs.

**Output — CSV**

```
priority,url,prev_traffic,current_traffic,traffic_change_pct,cause_primary,cause_contributing,evidence,next_action,fallback_action,expected_timeline
```

End with a "**3 biggest patterns this week**" summary (3 bullets max).

---

## 3. Keyword Cannibalization Fix

*Maps to workflow 3 in the article.*

Find and fix keyword cannibalization on **[your domain]**.

**Inputs**
- Scope: **[subfolder or "entire domain"]**
- Priority goal: **[max organic traffic / lift rankings for X / reduce index waste]**

**Tasks**
1. Find intent/keyword overlap where ≥ 2 URLs from the domain compete.
2. Cluster into groups (one group = one intent owned by multiple URLs).
3. Choose a canonical winner per group using ranking/traffic, topical relevance, authority/link strength, content completeness.
4. Draft a consolidation plan: merge, redirect, de-optimize.

**Output — table per group + summary CSV**

For each group, a Markdown block:

```
### Group: <intent / theme>
Keywords (top 5–20): …
Winner URL: …
Loser URLs: …
Decision rationale: …
Action plan:
  - Merge: …
  - Redirect: …
  - De-optimize: …
Risks / what to verify: …
```

Then one summary CSV at the end:

```
group_id,intent,winner_url,loser_urls,action_type,estimated_traffic_impact,confidence
```

---

## 4. Trending Keyword Research

*Maps to workflow 4 in the article.*

Trend-hunt for **[your topic area / seed keyword]**.

**Inputs**
- Seed: **[1–3 core keywords or short topic sentence]**
- Target market: **[country + language]** (or "global")
- Growth rule: keywords with **25%+ growth over the last 3 months**
- Min volume: **[set, or default to Ahrefs practical floor]**

**Tasks**
1. Semantic expansion (synonyms, near-synonyms, adjacent intents, subtopics competitors already cover).
2. Pull monthly search volume history, compute 3-month growth.
3. Filter by growth + guardrails: drop branded (unless requested), drop local-only (unless local target), drop non-starters.
4. Cluster into themes.

**Output — themes + flat CSV**

For each theme, a Markdown block:

```
### Theme: <name>
Why it's heating up: <1–2 sentences>
Suggested content angle: <who it's for + what to say>
Representative keywords (5–15): …
```

Plus a flat CSV of all kept keywords:

```
keyword,volume,kd,growth_3m_pct,theme,intent,parent_topic
```

Sorted: `theme` asc, `growth_3m_pct` desc.

---

## 5. Programmatic SEO Keywords

*Maps to workflow 5 in the article.*

Find programmatic SEO opportunities for **[your domain / niche]**.

**Inputs**
- Pattern seeds: **[2–5 example patterns, or a topic sentence]**
- Target market: **[country + language]**
- Preferred intent: **[commercial / informational / mixed]**

**Tasks**
1. Identify templatable keyword patterns (e.g. `[X] in [city]`, `[A] vs [B]`, `[role] salary in [country]`, `best [X] for [Y]`).
2. For each pattern: pull variant list, estimate volume + difficulty + intent.
3. Propose a content model per pattern.
4. Flag duplicate-intent / cannibalization risk.

**Output — pattern blocks + master CSV**

For each pattern, a Markdown block:

```
### Pattern: <pattern>
Audience + intent: …
Example variants (5–10): …
Template structure (sections): …
Required dynamic fields: …
Uniqueness rules (anti-thin-content): …
Internal linking: …
Risks: …
```

Plus a master CSV across all variants:

```
pattern,variant_keyword,volume,kd,intent,estimated_traffic_potential,recommended_template
```

Sorted by `pattern, estimated_traffic_potential desc`.

---

## 6. AI Mention Gap Analysis

*Maps to workflow 6 in the article.*

AI mention gap for **[your brand]** in **[your topic area]**.

**Inputs**
- Competitors: **[competitor 1], [competitor 2], [competitor 3]** (3–10)
- Target market: **[country / language if relevant]**
- Your positioning: **[1 paragraph on what you want to be known for]**

**Tasks**
1. Build candidate prompts/questions where competitors appear in AI answers.
2. For each, mark whether your brand is fully present, partially present, or absent.
3. Prioritize by prompt demand × competitor frequency × positioning fit.
4. For each top gap, propose how to earn the mention.

**Output — CSV**

```
prompt,competitors_mentioned,my_brand_status,prompt_demand,priority_score,gap_reason,recommended_content_angle,target_page_type,success_criteria
```

`my_brand_status ∈ {present, partial, absent}`. Sort by `priority_score desc`. Cap at 10–30 rows.

---

## 7. AI Citation Freshness Audit

*Maps to workflow 7 in the article.*

AI citation freshness for **[your domain]** in **[your topic area]**.

**Inputs**
- Competitors: **[5–10 competitor domains]**
- Freshness threshold: **["update if older than 12–24 months"]** (or your own rule)
- Your update cadence: **[how often you can refresh]**

**Tasks**
1. Identify pages currently cited by AI systems for this topic (yours + competitors).
2. Assess freshness from: last-updated/publish date, likely-outdated claims (pricing, features, policy), coverage vs current best practice.
3. Rank by staleness × reliance magnitude.
4. Recommend refresh / replace / retire per stale page.

**Output — CSV**

```
cited_url,domain,citation_source,last_updated,freshness_label,staleness_evidence,reliance_score,recommended_action,effort,expected_ai_visibility_impact
```

`freshness_label ∈ {fresh, somewhat_stale, stale}`. Sort by `reliance_score desc, last_updated asc`. End with "**Top 3 pages to refresh first**" + 1 sentence each.

---

## 8. EEAT Audit

*Maps to workflow 8 in the article.*

EEAT audit for **[your domain]**.

**Inputs**
- Page types to prioritize: **[homepage / category / product / guides / FAQs / location pages]**
- Evidence resources: **[1–2 links or notes about internal research, customer proof, author bios, credentials]**

**Tasks**
1. For each page type, audit Experience / Expertise / Authoritativeness / Trustworthiness signals.
2. Identify concrete gaps (no generic advice).
3. For each gap propose exactly what to add and where in the page structure.
4. Prioritize by impact × feasibility.

**Output — scorecard table per page type**

| Page type | Exp score (1–5) | Exp gaps | Exp fixes | Expt score (1–5) | Expt gaps | Expt fixes | Auth score (1–5) | Auth gaps | Auth fixes | Trust score (1–5) | Trust gaps | Trust fixes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|

(`Exp` = Experience, `Expt` = Expertise, `Auth` = Authoritativeness, `Trust` = Trustworthiness.) Add a single "**Top 3 fixes this week**" list plus a 1–2 week action plan with owner + deadline columns.

---

## Bonus. Reddit Listener (Demand + Outreach Briefs)

*Maps to the Bonus workflow in the article.*

Monitor Reddit for demand discovery for **[brand + category]**.

**Inputs**
- Subreddits: **[list]**
- Keywords: **[brand / category / pains / competitors]**
- Time window: **[last 7 / 30 days]**
- Response style: **[helpful / expert / short / long-form]**

**Tasks**
1. Find high-signal threads (real questions, repeated pains, decision moments; exclude self-promo/spam).
2. For each thread extract: intent, exact debate / claim, what users tried, why existing answers fall short.
3. Write a thread brief with a non-salesy comment draft and a "linkable" resource type to point to.
4. Categorize the opportunity (content idea / outreach / long-tail keyword idea).

**Output — CSV + thread blocks**

CSV header:

```
thread_url,subreddit,intent,why_it_matters,opportunity_type,suggested_resource_type,priority
```

Then a Markdown block per top thread:

```
### <thread title>
Summary (4–6 bullets):
Proposed response angle:
Suggested comment draft (non-salesy, ≤ 120 words):
Linkable resource to create or point to:
```

Cap at 8–20 briefs. End with a weekly outreach/content plan grouped by `opportunity_type`.
