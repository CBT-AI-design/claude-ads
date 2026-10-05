---
name: meta-ads-pixel-capi
description: "Set up and audit Meta measurement — the Meta Pixel, Conversions API (CAPI), datasets, event and parameter configuration, dataset quality and stats, and custom conversions in Events Manager. Use for Pixel/CAPI setup, event mapping, deduplication, data quality and event match quality review, custom conversion creation, or diagnosing why conversions are not tracking on Facebook and Instagram. Read-only by default."
---

# Meta Ads — Pixel, CAPI & Datasets

Configure and verify Meta's measurement layer: the Pixel/dataset, server-side
Conversions API events, their parameters, and custom conversions.

## Tools

- Datasets: `ads_get_datasets`, `ads_get_dataset_details`,
  `ads_get_dataset_quality`, `ads_get_dataset_stats`.
- Events: `ads_pixel_event_read`, `ads_pixel_event_create`,
  `ads_pixel_event_update`, `ads_pixel_event_delete`.
- Parameters: `ads_pixel_parameter_read`, `ads_pixel_parameter_create`,
  `ads_pixel_parameter_update`, `ads_pixel_parameter_delete`.
- Custom conversions: `ads_get_customconversions`.
- Catalog/commerce event sources: see `meta-ads-catalog`.

## Procedure

1. Identify the dataset/Pixel with `ads_get_datasets` and
   `ads_get_dataset_details`. Confirm which ad account and Page/domain it serves.
2. **Assess health first** with `ads_get_dataset_quality` and
   `ads_get_dataset_stats`: event volume, server vs. browser coverage,
   **deduplication** (matching `event_id` across Pixel + CAPI), event match
   quality (EMQ), and freshness. Report gaps before changing anything.
3. Review the event map with `ads_pixel_event_read` /
   `ads_pixel_parameter_read`: standard events (PageView, ViewContent,
   AddToCart, InitiateCheckout, Purchase, Lead) and their parameters (value,
   currency, content_ids, content_type).
4. For changes, preview the exact event/parameter diff, get approval, then use the
   create/update tools. Ensure Pixel and CAPI send the **same event with a shared
   event_id** so conversions deduplicate rather than double-count.
5. Review custom conversions with `ads_get_customconversions`; confirm their rule,
   value, and the optimization events campaigns actually use.
6. **Verify** by reading the dataset/event back and re-checking quality/stats.

## Common diagnostics

- "Conversions not showing" → check dataset freshness, event mapping, pixel firing
  (browser) vs. CAPI (server) coverage, and the campaign's optimization event.
- "Double-counted purchases" → missing/ mismatched `event_id` deduplication
  between Pixel and CAPI.
- "Low match quality" → missing customer-information parameters (hashed email,
  phone, external_id, fbp/fbc).

## Guardrails

- Never put raw PII or access tokens into events sent as examples, into files, or
  into logs. CAPI customer-info parameters must be hashed as Meta specifies.
- Deleting events/parameters/custom conversions can break optimization and
  reporting and may be irreversible — require explicit confirmation, show
  dependents, and prefer leaving them in place.
- Verify every claim about tracking against dataset quality/stats, not assumption.
