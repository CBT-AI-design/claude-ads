---
name: meta-ads-campaigns
description: "Create and manage Meta Ads campaign structure — campaigns, ad sets, and ads on Facebook and Instagram. Use to launch a new Meta/Facebook/Instagram campaign, build an ad set with targeting, placements, optimization goal, bid strategy, budget and schedule, attach creatives to ads, duplicate structure, or pause/activate/edit existing campaign objects. Draft-first and read-only by default."
---

# Meta Ads — Campaign Builder

Build and manage the campaign → ad set → ad hierarchy. **Draft-first**: assemble
the full plan, show it, get explicit approval, and create objects **paused**.

## Tools

- Read / inspect: `ads_get_ad_entities`, `ads_get_ad_accounts`,
  `ads_get_ad_account_pages`, `ads_get_ig_accounts`, `ads_get_field_context`,
  `ads_get_opportunity_score`.
- Create: `ads_create_campaign`, `ads_create_ad_set`, `ads_create_ad`,
  `ads_create_creative` (see `meta-ads-creative`).
- Edit / state: `ads_update_entity`, `ads_activate_entity`.
- Diagnose failures: `ads_get_errors`, `ads_get_help_article`.

## Procedure

1. **Confirm inputs** before building:
   - Ad account (`act_<id>`), Facebook Page, Instagram account.
   - Objective (`OUTCOME_SALES`, `OUTCOME_LEADS`, `OUTCOME_TRAFFIC`,
     `OUTCOME_AWARENESS`, `OUTCOME_ENGAGEMENT`, `OUTCOME_APP_PROMOTION`).
   - Budget model: Advantage+ / campaign budget optimization (CBO) vs. ad-set
     budget; daily vs. lifetime amount and currency.
   - Audience (from `meta-ads-audiences`), placements (Advantage+ placements vs.
     manual), optimization goal + conversion event, bid strategy, schedule.
   - Destination (URL, lead form, app, messaging) and the creative(s).
2. **Verify field values** with `ads_get_field_context` for objective,
   optimization goal, billing event, and bid strategy so you pass valid enums.
3. **Assemble the plan** top-down and present it as a preview:
   Campaign (objective, budget type) → Ad set(s) (audience, placements, goal,
   bid, budget, schedule) → Ad(s) (creative + destination). Show the exact
   parameters, estimated blast radius, and spend implications.
4. **Get explicit approval** of that exact plan and the spend ceiling.
5. **Create paused, top-down**, reading each returned ID before the next step:
   - `ads_create_campaign` (set `status: PAUSED`).
   - `ads_create_ad_set` referencing the new campaign ID (`status: PAUSED`).
   - `ads_create_creative` (or reuse an existing creative ID).
   - `ads_create_ad` referencing the ad set ID and creative ID (`status: PAUSED`).
6. **Verify** with `ads_get_ad_entities` that each object exists with the intended
   configuration. Generate an `ads_get_ad_preview` for the ad before launch.
7. **Activation is a separate approval.** Only after the user confirms go-live,
   set objects active via `ads_activate_entity` / `ads_update_entity`. Activate
   bottom-up or per Meta's requirement so nothing spends before the structure is
   complete.

## Editing existing campaigns

- Read current state with `ads_get_ad_entities` first; show a before/after diff.
- Material ad-set edits (audience, optimization goal, budget swing) can reset the
  **learning phase** — call this out in the diff before applying.
- Apply the minimal change with `ads_update_entity`, then read the object back to
  confirm. Keep a rollback note (prior field values).

## Guardrails

- Never launch (activate) without a distinct, explicit go-live approval.
- Do not invent audience, Page, pixel, or creative IDs — resolve each from a tool
  result.
- Prefer pause/archive over deletion. Refuse bulk permanent deletion; offer to
  pause instead.
- Respect account spend ceilings; if none is given, do not create spending
  objects in an active state.
