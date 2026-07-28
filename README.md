# AB Test: Guest Checkout vs Mandatory Registration

Beauty e-commerce, checkout optimization.

## Hypothesis

Mandatory registration before purchase (first name, last name, phone, email, address) adds friction to the checkout funnel and lowers completion rate. Guest checkout removes this barrier and should raise the share of completed orders among users who start checkout, without a negative impact on average order value.

- H0: p_test = p_control. Conversion rates do not differ; the observed gap comes from sampling variability.
- H1: p_test != p_control (two-sided). Conversion rates differ between groups. We test both directions to catch a negative effect as well as a positive one.

Expected effect: completion rate rises from 50% to 55% (5 p.p).

Audience: users who reach the checkout step, on both mobile and desktop.

Metrics:

| Type | Metric |
|---|---|
| Primary | Checkout completion rate = completed / started checkout |
| Guardrail | AOV |
| Informational | 30-day repeat purchase rate |

## What it looks like

- Control: a form with required fields (first name, last name, phone, email, address). Payment is unavailable until the account is created .
- Test: the same form plus a "Continue as guest" button. Payment is available without registration; email is optional.

## Limitations

- Network effects do not apply: checkout is an individual action, and one user's behavior does not affect another's.
- A novelty effect is possible in the first days of the test. We check for it by reviewing conversion rate day by day.
- Long-term metrics (repeat purchase rate) are collected as informational, but a statistical conclusion on them needs an observation window beyond the 14-day test.

## Sample size

| Parameter | Value |
|---|---|
| Baseline conversion | 50% |
| MDE | 5 p.p |
| Alpha | 0.05 |
| Power | 0.8 |
| Test type | Two-proportion z-test, two-sided |
| n per group | 1,565 |
| Total | 3,130 |
| Duration | 14 days |

Calculated with Evan Miller's calculator (evanmiller.org/ab-testing/sample-size.html).

The full test design table is in `design_parameters.md`.

## Results

| Metric | Control | Test |
|---|---|---|
| Started checkout | 1,565 | 1,565 |
| Completed orders | 850 | 900 |
| Conversion rate | 54.3% | 57.5% |
| 95% CI | 51.9%-56.8% | 55.1%-60.0% |

z = 1.80, p-value (two-sided) = 0.072.

The guardrail (AOV) did not change significantly, so the conversion gain did not come at the cost of smaller orders.

- Full calculation with formulas: `calculation/AB_test_checkout_calculation.xlsx`
- Chart with confidence intervals: `images/cr_confidence_intervals.png`

## Validity check (SRM)

A Sample Ratio Mismatch check tests whether the actual traffic split matches the expected 50/50, independent of the conversion result.

- Calculator: socscistatistics.com/tests/goodnessoffit/calculator
- Formula: chi-square = sum((observed − expected)² / expected)
- Threshold: p >= 0.01 means the split is healthy; p < 0.01 means SRM is present.

Check result: chi-square = 0.29, p = 0.592. No SRM detected; randomization is working correctly.

## Conclusion

The result sits on the border: p = 0.072 against 0.05 threshold. The confidence intervals of the two groups partly overlap, so the difference in conversion rate is not statistically significant at this sample size.


Recommendation: do not roll out the feature to 100% of traffic based on this result. Two possible next steps: extend the test to collect a larger sample or launch a partial rollout on mobile, where the effect is expected to be larger with monitoring of the guardrail
