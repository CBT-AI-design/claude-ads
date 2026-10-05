---
name: meta-ads-experiments
description: "Design, run, and read Meta Ads experiments — A/B tests (split tests) and conversion/brand lift studies on Facebook and Instagram. Use to set up a valid test, check experiment eligibility, compare creatives/audiences/strategies with a controlled test, measure incremental lift, or interpret a test readout. Read-only by default; creating a test is draft-first."
---

# Meta Ads — Experiments (A/B & Lift)

Run controlled tests instead of guessing. Pick the right method, size it, and read
it honestly.

## Tools

- Eligibility: `ads_experiment_check_eligibility`.
- A/B (split) tests: `ads_experiment_abtest_create_test`,
  `ads_experiment_abtest_get_test`, `ads_experiment_abtest_update_test`.
- Lift studies: `ads_experiment_lift_create_test`, `ads_experiment_lift_get_test`.
- List: `ads_experiment_list_tests`.

## Choosing the method

- **A/B / split test** — compare two or more cells that differ in **one** variable
  (creative, audience, placement, optimization goal, bid strategy). Answers
  "which option performs better?"
- **Conversion/brand lift** — holdout vs. exposed to measure **incrementality**
  ("did the ads cause additional conversions?"). Use when you need true lift, not
  just relative comparison.

## Procedure

1. State the **hypothesis** and the single variable under test. Fix everything
   else.
2. Check feasibility with `ads_experiment_check_eligibility` (budget, audience
   size, conversion volume, duration).
3. **Power the test**: ensure enough expected conversions and a long enough run
   (typically at least one to two full conversion cycles) to detect a meaningful
   difference. If it is underpowered, say so and propose adjustments rather than
   launching a test that cannot conclude.
4. Preview the design (cells, split, metric, duration, budget), get approval, then
   create with `ads_experiment_abtest_create_test` or
   `ads_experiment_lift_create_test`.
5. **Do not peek-and-stop early.** Let it reach its planned sample/duration before
   declaring a winner. Monitor with the `get_test` tools; avoid edits that
   contaminate cells.
6. Read out with `ads_experiment_abtest_get_test` / `ads_experiment_lift_get_test`:
   report the point estimate, confidence/significance, and whether the result is
   conclusive. An inconclusive test is a valid outcome — label it as such.

## Guardrails

- One variable per A/B cell; confounded tests are not reported as clean wins.
- Never claim significance the tool output does not support.
- Rolling a winner out to the full account is an optimization change — route it
  through `meta-ads-optimization` with its own approval.
