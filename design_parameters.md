# Test design parameters

## Randomization unit

user_id

## Group assignment moment

A user is assigned a variant (control/test) on the first visit to the checkout page during the test period. The assigned variant sticks with that user for the rest of the test.

## Exposure moment

A user counts as exposed once the checkout form appears on screen. Earlier events, like adding an item to the cart, fall outside the experiment: the form variant couldn't have influenced them yet.

## Duration calculation

At roughly 300 checkout starts per day, the required sample (3,130 observations) is reached in 10-11 days by the raw math.

We round up to 14 days (two full weeks) for three reasons:

- Day-of-week effect: conversion differs between weekdays and weekends, so the test needs at least two full weekly cycles.
- Novelty effect: users need time to adjust to the new form, and the first days can skew the result.
- Seasonality: the test period is checked against the company's promo calendar and does not overlap with sales or holidays.

## Full parameter table

| Parameter | Value |
|---|---|
| Metric (primary) | Checkout completion rate |
| Guardrail | AOV |
| H0 | p_test = p_control |
| H1 | p_test != p_control (two-sided) |
| Randomization unit | user_id |
| Split | 50/50, stratified by device |
| Baseline conversion | 50% |
| MDE | 5 percentage points |
| Alpha | 0.05 |
| Power | 0.8 |
| Test type | Two-proportion z-test |
| n per group | 1,565 |
| Total | 3,130 |
| Duration | 14 days |
