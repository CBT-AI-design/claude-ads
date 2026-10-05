---
name: meta-ads-optimization
description: "Diagnose and apply Meta Ads optimizations — budget reallocation, bid strategy changes, pausing or activating ads, scaling winners, fixing underperformers, and addressing creative fatigue on Facebook and Instagram. Use to improve CPA/ROAS, reallocate spend, scale or pause campaigns/ad sets/ads, or act on an opportunity score. Draft-first; every change needs explicit approval."
---

# Meta Ads — Optimization

Turn performance evidence into changes. **Default to draft.** Propose, get
approval, apply the smallest reversible change, verify, and keep a rollback note.

## Tools

- Diagnose: `ads_insights_performance_trend`, `ads_insights_anomaly_signal`,
  `ads_insights_auction_ranking_benchmarks`, `ads_get_opportunity_score`,
  `ads_get_ad_entities`.
- Apply: `ads_update_entity` (budgets, bids, status, schedule),
  `ads_activate_entity` (resume), creative rotation via `meta-ads-creative`.
- Diagnose failures: `ads_get_errors`, `ads_get_field_context`,
  `ads_get_help_article`.

## Procedure

1. **Load evidence first** (see `meta-ads-insights`): current structure, spend,
   results, trend, frequency/fatigue, economics (margin, target CPA/ROAS),
   attribution window, and learning-phase state.
2. Identify the real decision and its **cause** — do not optimize one metric in
   isolation. Distinguish a delivery problem (CPM/auction), a relevance problem
   (CTR/ranking), a conversion problem (CVR/landing/tracking), and fatigue
   (rising frequency, falling CTR).
3. Compare options: **no change**, **experiment** (hand to `meta-ads-experiments`),
   and **mutation**. Account for learning-phase reset, budget-swing limits, policy
   risk, and opportunity cost.
4. Produce **ranked recommendations** with expected effect, confidence, and a
   success measure. Present each as a before/after diff with explicit IDs.
5. On approval, apply the **minimal** change with `ads_update_entity`
   (e.g. a bounded budget step, not a large swing that resets learning). Create
   nothing new in an active state without approval.
6. **Verify** remote state by reading the object back, and record the prior values
   as the rollback.

## Decision guardrails (conditional, not universal)

- Do not pause an ad set solely because CPA crossed a fixed multiple — check
  sample size, conversion lag, and whether it is still in learning.
- Do not apply one fixed budget-to-CPA ratio across different objectives.
- Avoid frequent ad-set edits that repeatedly reset the learning phase.
- Scale winners in bounded steps (e.g. moderate increments with a stabilization
  window) rather than large jumps.
- For fatigue, prefer refreshing creative (`meta-ads-creative`) over only cutting
  budget.

## Destructive-action boundary

Refuse **permanent deletion** of campaigns, ad sets, ads, audiences, or custom
conversions — it cannot be made safe by confirmation. Offer reversible
alternatives: pause, archive where supported, apply labels, or export a backup
with a review date. Prefer pause over archive, and archive over delete.
