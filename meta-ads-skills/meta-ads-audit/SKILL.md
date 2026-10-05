---
name: meta-ads-audit
description: "Run a full Meta Ads account health audit across Facebook and Instagram — account structure, campaigns and objectives, audiences, placements, creative diversity and fatigue, budgets and bidding, Pixel/CAPI measurement and attribution, catalog/dynamic-ads health, and policy exposure. Use for a Meta Ads audit, account review, health check, or 'what's wrong with my Meta account' request. Strictly read-only."
---

# Meta Ads — Account Audit

A structured, **read-only** review of a Meta ad account. Produce observations,
diagnoses, prioritized recommendations, and unscored opportunities — never apply
changes from this skill.

## Scope and tools

Work through each area, reading only. Hand any resulting changes to the relevant
action skill (`meta-ads-optimization`, `meta-ads-creative`, `meta-ads-pixel-capi`,
`meta-ads-catalog`).

1. **Account & structure** — `ads_get_ad_accounts`, `ads_get_ad_entities`,
   `ads_account_get_activity_logs`. Check objective fit, naming, campaign/ad-set
   sprawl, budget model (CBO vs. ad-set), and recent material changes.
2. **Performance** — `ads_insights_performance_trend`,
   `ads_insights_anomaly_signal`, `ads_get_opportunity_score`,
   `ads_insights_auction_ranking_benchmarks`,
   `ads_insights_industry_benchmark`, `ads_insights_advertiser_context`. Assess
   CPM/CPC/CTR/CPA/ROAS vs. objective and contextual benchmarks.
3. **Audiences** — `ads_get_ad_account_custom_audiences`,
   `ads_get_custom_audience_adsets`. Check overlap, over-segmentation, stale
   seeds, and exclusions.
4. **Creative** — `ads_get_creatives`, `ads_get_ad_images`, `ads_get_ad_videos`,
   `ads_get_ad_preview`. Check diversity, fatigue (frequency, CTR decay), format
   coverage (feed/stories/reels), and policy risk.
5. **Measurement** — `ads_get_datasets`, `ads_get_dataset_quality`,
   `ads_get_dataset_stats`, `ads_get_customconversions`. Check Pixel+CAPI
   coverage, deduplication, event match quality, and attribution settings.
6. **Catalog / dynamic ads** (if applicable) — `ads_catalog_get_diagnostics`,
   `ads_catalog_get_dynamic_ads_health`.
7. **Policy & account state** — surface disapprovals, restrictions, and activity-log
   anomalies from the reads above.

## Method

- Fix the audit window, timezone, currency, and attribution setting; state them.
- For each control, record: observation, diagnosis, severity, confidence, and the
  evidence (which tool + what it returned). Mark unknowns where data is missing —
  an unknown reduces coverage; it is not a failure.
- Keep health, evidence coverage, and opportunities separate. Do not score an
  account health number off memory — base every rating on retrieved data.
- Benchmarks are contextual, qualified by objective/geography/method; never a hard
  pass/fail threshold.

## Output

Deliver: account health summary, evidence coverage (what you could and could not
verify), prioritized findings (issue → impact → recommended fix → owner →
measurement), and a list of **unscored opportunities** (eligible-but-unused
features). Note contradictions and missing inputs. Produce **no account changes**
from this skill.
