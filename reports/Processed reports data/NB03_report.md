# NB03 — BUSINESS INTERPRETATION

## Finding 1: Platform Effect on Engagement
**Finding:** Tiktok posts achieve the highest median engagement.
**Evidence:** Tiktok → median 44.0 likes, 8.0 shares, 4.0 comments (n=390). Instagram → median 37.0 likes (n=595). Delta = +7.0 likes.
**Action:** Test more Tiktok-native content in next 4-week cycle. Compare against current Instagram baseline using median likes.
**Limitation:** No reach/impressions → engagement rate cannot be computed. Volume effects uncontrolled.

## Finding 2: Sentiment Effect
**Finding:** Positive-sentiment posts show higher median likes than negative.
**Evidence:** Positive median = 39.0, Neutral = 28.0, Negative = 32.0. Delta (Pos − Neg) = +7.0 likes.
**Action:** Shift content tone toward positive/value-led framing. Track sentiment of posted content.
**Limitation:** Correlation ≠ causation. Sentiment labels were pre-supplied by the dataset — provenance unknown.

## Finding 3: Day-of-Week Effect
**Finding:** Thursday shows the highest median likes.
**Evidence:** Best day = Thursday (34.0 median likes, n=333). Worst = Monday (32.0, n=326).
**Action:** Schedule high-priority posts on Thursday. Validate with 4-week A/B test.
**Limitation:** Day-of-week coverage uneven — small n on some days biases the result.

## Marketing Recommendation
Based on 2200 cleaned posts:
1. Prioritize **Tiktok** for content investment (highest median likes = 44.0).
2. Lean into **positive-sentiment** posts (Δ=+7.0 vs negative).
3. Schedule high-stakes posts on **Thursday**.

Measurement plan: 4-week controlled test — one post per platform per week, same content. Compare median likes + shares. Track by platform × day × sentiment to validate on the brand's own audience.

## Limitations (top-level)
- No reach or impressions → engagement RATE not computable
- followers_count missing for 88 posts
- Dataset is NOT Pakistani brand data → findings are general, not Pakistan-specific
- Sentiment labels pre-supplied (provenance unknown)
- Final N = 2200 rows after cleaning

## Analytics Preview (Step 9 — Conceptual)
- **Classification:** Predict high/low engagement (label = median split). ⚠️ Data leakage guardrail: do not use `likes` in both label and features. Train/test split mandatory.
- **Clustering:** Group posts by standardized (likes, shares, comments) — reveals content archetypes.
- **Regression:** Predict likes from platform + sentiment + followers_count. Use log(likes) for right-skew. Train/test split.
- **Exit question:** Which technique best answers YOUR marketing question? (Regression fits best here — quantifies which factors predict engagement.)

## Next Step
Add paid-campaign data (spend, impressions, clicks, conversions) to enable ROAS analysis.
