# Agentic SEO — Agent A workflow prompts

Companion repo for the article "What is agentic SEO and how do you get started?". Each section below is a copy/paste prompt for **Agent A** to recreate one of the workflows in the post.

Replace anything in **[brackets]** with your own values.

---

## 1. Site Audit Discovery
Run an Ahrefs Site Audit for **[your domain]** (use the site's primary crawl scope; if you have a country/language focus, include it).

Deliverables (ranked top 10–15):
1) **Issue** (technical SEO category, short label)
2) **Evidence** (what the audit detected; include the exact issue summary Ahrefs provides)
3) **Scope** (how many URLs affected, and sample URLs)
4) **Impact estimate** (expected impact on traffic/crawl; high/med/low + 1–2 sentences)
5) **Effort estimate** (low/med/high) and dependencies
6) **Recommended fix** (concrete next step)
7) **Acceptance criteria**

Rules: finish this sprint; remove duplicates/low-signal; consolidate similar issues.

---

## 2. Site Audit Issue Fixer
Fix this technical SEO issue on **[your domain]**: **[paste issue details + URL(s)]**.
Repo/location: **[GitHub repo URL + branch or local path]**.

Task: diagnose root cause (which file/template generates it), implement smallest safe change, preserve SEO-critical behavior (canonical/redirect/hreflang/pagination/robots), verify via tests or a minimal smoke render.

Output: root cause summary, PR diff/patch summary, before/after checks, rollback plan, review checklist.

---

## 3. Declining Content Detection
Weekly scan: diagnose pages on **[your domain]** whose organic traffic is declining in **[last 30/60/90 days]**.

For each top page: priority (1–10), traffic-drop summary, evidence bullets, pick the most likely 1–3 causes (content freshness, backlink loss, SERP/feature shift, internal changes, cannibalization), and one next action + 2nd-best fallback.

Keep queue to top 10–20.

---

## 4. Keyword Cannibalization Fix
Find and fix **keyword cannibalization** on **[your domain]** (scope: [subfolder/entire domain]).

Task: find intent/keyword overlap where multiple URLs compete; cluster into groups; choose a canonical winner using ranking/traffic, topical relevance, authority/link strength, and ability to expand.

Output per group: keywords (5–20), ranked URLs, winner rationale, consolidation plan (merge/redirect/de-opt) + risks + what to verify.

---

## 5. Trending Keyword Research
Trend hunt for **[topic/seed]** targeting **[country + language]**.
Growth rule: keywords with **25%+ growth over the last 3 months**.

Task: semantic expansion (meaning/intent adjacency, not only exact-match variants), pull monthly volume history, filter by growth and guardrails (remove brand/local unless requested, remove non-starters), cluster into themes.

Output: theme map; per theme: representative keywords, trend summary, why it's heating up, and suggested content angle; top 5 themes to pursue.

---

## 6. Programmatic SEO Keywords
Build programmatic SEO opportunities for **[domain/niche]**.

Task: identify templatable keyword patterns (e.g., [X] in [city], [A] vs [B], role salary in [country], best [X] for [Y]); pull full variant demand; estimate intent + ranking friction; propose a template content model (required fields/sections, anti-thin-content/uniqueness rules, internal linking plan).

Flag risks: duplicate intent + cannibalization.

---

## 7. Anchor Text Analysis
Anchor text risk check for **[your domain]** focused on **[money page URLs]**.

Inputs: competitors **[3–5 competitor domains]**, time window **[last 6–12 months]** (if supported).

Task: pull anchor distribution; categorize anchors (brand/exact/partial/generic/supporting); detect over-optimization patterns; benchmark vs competitors; recommend a practical diversification plan.

Output: risk table per money page + summary top 3 to address first.

---

## 8. AI Mention Gap Analysis
AI mention gap analysis for **[your brand]** in **[topic area]**.

Inputs: competitors **[competitor list]**, target market **[country/language if relevant]**, and your positioning **[1 paragraph]**.

Task: build candidate prompts where competitors show up; determine whether your brand is missing/partial; score by demand + competitor frequency + positioning fit; for top gaps propose how to earn the mention (content type + angle + target page type).

Output: prioritized list of 10–30 target prompts with success criteria.

---

## 9. AI Citation Freshness Audit
AI citation freshness for **[your domain]** in **[topic area]**.

Inputs: competitors **[domains]**, freshness threshold **[e.g. older than 12–24 months]**, and your update cadence.

Task: identify cited pages (yours + competitors), assess freshness (recency + likelihood of outdated claims + coverage vs best practice), rank by staleness risk and reliance magnitude; recommend refresh/replace/retire.

Output: cited page list with evidence + action + effort + expected impact.

---

## 10. EEAT Audit
EEAT audit for **[your domain]**.

Inputs: page types to prioritize ([homepage/categories/product pages/guides/FAQs/location pages]) + evidence resources.

Task: for each page type identify concrete Experience/Expertise/Authoritativeness/Trust gaps (avoid generic advice). For each gap propose exactly what to add/change and where; prioritize by impact + feasibility.

Output: EEAT scorecard per page type + top 3 fixes + 1–2 week plan.

---

## Bonus. Reddit Listener (Demand + Outreach Briefs)
Monitor Reddit for demand discovery for **[brand + category]**.

Inputs: subreddits **[list]**, keywords **[brand/category/pains/competitors]**, time window, and response style.

Task: find high-signal threads; extract intent + exact debate + what tried + why incomplete; write thread briefs (summary, response angle, linkable resource type, suggested comment) and categorize opportunities.

Output: 8–20 thread briefs + one-line why it matters + suggested weekly plan.
