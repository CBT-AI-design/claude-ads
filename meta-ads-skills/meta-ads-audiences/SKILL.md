---
name: meta-ads-audiences
description: "Create and manage Meta Ads audiences — custom audiences, website/engagement/customer-list audiences, lookalikes, and retargeting on Facebook and Instagram. Use to build or update a custom audience, add or remove users from a list, inspect which ad sets use an audience, or plan targeting. Handles customer data with privacy care; read-only by default."
---

# Meta Ads — Audiences

Build and manage custom and lookalike audiences and retargeting segments.

## Tools

- Read: `ads_get_ad_account_custom_audiences`, `ads_get_custom_audience`,
  `ads_get_custom_audience_adsets`.
- Create / edit: `ads_create_custom_audience`, `ads_update_custom_audience`,
  `ads_update_custom_audience_users` (add/remove members),
  `ads_delete_custom_audience`.
- Supporting: `ads_get_datasets` / pixel skills for website custom audiences,
  `ads_get_field_context` for allowed subtypes.

## Audience types

- **Customer list** — hashed emails/phones uploaded as members.
- **Website / Pixel** — built from Pixel or dataset events (needs a dataset; see
  `meta-ads-pixel-capi`).
- **Engagement** — people who engaged with a Page, IG account, video, or lead
  form.
- **Lookalike** — modeled from a seed custom audience + country + ratio.

## Procedure

1. List existing audiences with `ads_get_ad_account_custom_audiences` to avoid
   duplicates and to reuse seeds.
2. Clarify the **source**, **size/retention window**, and intended use
   (prospecting vs. retargeting vs. exclusion).
3. For a new audience, preview the definition (type, source, rule, retention),
   get approval, then `ads_create_custom_audience`.
4. For a customer list, confirm consent/lawful basis, then add members with
   `ads_update_custom_audience_users`. **Only upload already-hashed identifiers**;
   never place raw customer emails/phones into files, logs, or reports.
5. For a lookalike, pass a valid seed audience ID, country, and ratio.
6. Verify with `ads_get_custom_audience`; check usage with
   `ads_get_custom_audience_adsets` before any change or deletion.

## Privacy and safety

- Treat any supplied customer list as sensitive PII. Minimize handling, do not
  echo it back, and never commit it anywhere.
- Confirm there is consent / a lawful basis before uploading customer data.
- Before deleting an audience, check `ads_get_custom_audience_adsets` — deletion
  breaks ad sets that depend on it. Prefer leaving it unused over deleting;
  deletion of audiences is generally irreversible, so require explicit
  confirmation and a stated reason.
- Deliver audience IDs and definitions; never embed membership data in output.
