# Big Data Analytics Project 
## Social Media Engagement Analysis

**Author:** Syed Muhammad Waiz
**Course:** Big Data Analytics
**Date:** 2026-10-25

---

## 1. Business Question

Which platforms, content sentiments, and posting days are associated with
higher engagement on a ~2,000-post social-media dataset, and what should a
brand test next in its publishing strategy?

This project demonstrates the full Big Data Analytics workflow:
load → inspect → clean → EDA → interpret → scale → justify. It does not
build a production model.

---

## 2. Dataset & Provenance

- **Source:** `mrudukp/social-media-sentiment-analysis`
- **Permission:** granted by the repository author on 2026-10-25
- **Shape:** 2,260 rows × 13 columns
- **Columns:** `post_id, timestamp, platform, user, followers_count,
  verified, text, hashtags, country, likes, retweets_shares, comments, sentiment`
- **Provenance note:** The dataset is multi-platform, multi-country
  social-media posts. It is **not** verified as Pakistani clothing-brand
  data. All findings describe this dataset only and are not claims about
  Pakistan.

---

## 3. Data Quality & Cleaning (NB01)

Five quality issues were identified and documented:

1. **Duplicate post_ids:** 44 fully identical rows, 60 duplicated `post_id`
   values (16 partial duplicates — rows sharing an ID but with different
   content). Decision: keep partial duplicates (they may be legitimate
   cross-platform re-posts); drop only fully identical rows.
2. **Platform case inconsistency:** "TWITTER" vs "Twitter", etc. —
   10 raw values collapse to 5 clean platforms. Without cleaning,
   ~10–15% of rows would be misclassified.
3. **Country case + whitespace inconsistency:** "usa  " vs "USA", etc. —
   24 raw values collapse to 12 clean countries.
4. **Numeric columns stored as strings** (comma-formatted like "1,125") —
   converted to `float64` via `pd.to_numeric(errors="coerce")`.
5. **Impossible negative like value** (`likes = -1`) — treated as a
   sentinel value and converted to `NaN`.

**Missing values** (documented, not auto-dropped):

| Column | Nulls | Interpretation |
|---|---|---|
| hashtags | 788 (34.87%) | **Legitimate blank** — post had no hashtag |
| country | 126 (5.58%) | True missing |
| followers_count | 90 (3.98%) | True missing |
| comments | 46 (2.04%) | True missing (not zero) |
| verified | 44 (1.95%) | True missing |
| sentiment | 33 (1.46%) | True missing |

All four engagement metrics (likes, shares, comments, followers) are
**right-skewed** (skewness > 1). Therefore **median** is used throughout
NB02–NB04 for group comparisons — the mean would be inflated by a
handful of high-engagement posts.

---

## 4. EDA & Findings (NB02)

After cleaning and re-deduplication, the final dataset contains **2,200
rows**.

### Finding 1 — TikTok leads median engagement

| Platform | n | Median likes | Median shares | Median comments |
|---|---|---|---|---|
| **TikTok** | 390 | **44.0** | **8.0** | **4.0** |
| Instagram | 595 | 37.0 | 6.0 | 3.0 |
| Twitter | 726 | 32.0 | 5.0 | 3.0 |
| Facebook | 370 | 26.0 | 5.0 | 2.0 |
| LinkedIn | 119 | 20.0 | 3.0 | 2.0 |

TikTok posts receive a median of **+7 likes, +3 shares, +2 comments** over
Instagram — the next-best platform by median likes.

### Finding 2 — Positive sentiment gets more engagement

| Sentiment | n | Median likes | Median shares |
|---|---|---|---|
| **Positive** | 879 | **39.0** | **7.0** |
| Neutral | 652 | 28.0 | 4.5 |
| Negative | 636 | 32.0 | 5.0 |

Positive-sentiment posts receive **+7 median likes** and **+2 median shares**
over negative-sentiment posts. Neutral posts are the lowest — suggesting
that *clear* emotional framing (either direction) outperforms neutral
messaging, with positive winning.

### Finding 3 — Thursday is the strongest day

| Day | n | Median likes |
|---|---|---|
| Thursday | 333 | **34.0** |
| Friday | 339 | 34.0 |
| Sunday | 267 | 34.0 |
| Wednesday | 291 | 33.0 |
| Saturday | 306 | 33.0 |
| Monday | 326 | 32.0 |
| Tuesday | 338 | 32.0 |

The delta between best and worst day is only **2 median likes** — small,
but Thursday/Friday/Sunday consistently outperform Monday/Tuesday. The
pattern is directional, not dramatic.

### Finding 4 — Follower count is a moderate predictor of likes

Log-log Pearson correlation (followers vs. likes): **r = 0.493**
(moderate positive). Audience size explains roughly **r² ≈ 24%** of the
variance in likes — meaning content and platform choice explain the
remaining ~76%. **Audience size alone does not determine engagement.**

### Charts produced

- Chart 1: Posts per platform (volume)
- Chart 2: Median engagement by platform (likes / shares / comments)
- Chart 3: Median engagement by sentiment
- Chart 4: Top 10 countries by posts (USA 466, India 389, UK 217, Canada 176, ...)
- Chart 5: Likes vs followers (log-log scatter with correlation)
- Chart 6: Median likes by day of week
- Chart 7: Scaling benchmark (see §6)

*[Insert 4–6 charts here — the PNGs are in `figures/`]*

---

## 5. Business Recommendation (NB03)

> **Prioritize TikTok with positive-sentiment content, published mid-to-late
> week, and measure the result with a controlled 4-week test.**

### Supporting evidence

1. **Platform:** TikTok → median **44 likes** per post (n=390) vs Instagram
   **37** (n=595). Delta = **+7 likes**. The sample size is large enough to
   trust the direction.
2. **Sentiment:** Positive → median **39 likes** vs Negative **32** and
   Neutral **28**. Delta (Positive − Negative) = **+7 likes**. Neutral is
   the worst-performing, so *emotional clarity* matters more than
   positivity alone.
3. **Timing:** Thursday / Friday / Sunday show median **34 likes** vs
   Monday / Tuesday at **32**. The effect is small (2 likes) but
   directionally consistent.

### Action plan

- **Week 1–4:** Publish 1 TikTok-native post per week with positive tone,
  on Thursday. Publish a matched control post on Instagram the same week.
- **Measurement:** Compare median likes across the 4-week window. Track
  reach if available (currently missing from the dataset).
- **Guardrail:** Do not compare total likes — use **median** and **per-post**
  metrics, because the data is right-skewed.

### Limitations of this recommendation

- No `reach` or `impressions` → **engagement rate** cannot be computed;
  only raw counts are available.
- `followers_count` is missing for **88 posts** — audience-controlled
  comparison is possible for ~96% of the data but not all.
- **Sentiment labels are pre-supplied** by the dataset — their provenance
  is unknown, so "positive sentiment → more likes" should be re-verified
  on the brand's own labelled content.
- The dataset is **not Pakistani clothing-brand data** — these findings
  are directional and should be re-validated on the brand's own account.

---

## 6. Scaling Experiment & Tool Justification (NB04)

The same `groupby("platform").median()` pipeline was benchmarked at
increasing dataset sizes by concatenating the cleaned 2,200-row dataset
N times. **This is a scaling test only — the repeated rows are not new
real-world evidence.**

### Results (measured on Google Colab)

| Target rows | Actual rows | Build (s) | Groupby (s) | Memory (MB) |
|---|---|---|---|---|
| 2,000 | 2,200 | 0.000 | **0.024** | 1.4 |
| 20,000 | 19,800 | 0.002 | **0.005** | 12.9 |
| 200,000 | 198,000 | 0.034 | **0.036** | 128.7 |
| 2,000,000 | 1,999,800 | 0.308 | **0.355** | 1,299.7 |

The groupby operation scales **roughly linearly** in wall-clock time up to
2 million rows, and memory grows linearly at approximately **650 KB per
row** (1.3 GB at 2M rows). Even at 2M rows, a single Pandas groupby runs
in **0.355 seconds** — well within interactive tolerance.

### Would Spark help? — measured, not guessed

- **At 2M rows (measured): NO.** Pandas completes in 0.355s on a single
  Colab VM. Spark's JVM startup (~10–20s), shuffle overhead, and
  cluster-management cost would exceed the actual compute saved.
- **At 50M+ rows (extrapolated from linear trend): LIKELY YES.** Reasons:
  1. **Distributed storage** (HDFS/S3) avoids needing the whole dataset
     in RAM (2M rows = 1.3 GB; 50M rows ≈ 32 GB, exceeding typical
     single-node RAM).
  2. **Parallel groupby** across machines cuts wall-clock time.
  3. **Fault tolerance** during multi-hour jobs.
  4. **Cross-team access** — shared storage + SQL-like interface.
- **Conclusion:** For this project's 2,200-row dataset, Pandas is strictly
  correct. Spark becomes a candidate tool when data volume, latency, or
  distributed-storage requirements actually justify the added operational
  complexity — not by default for any dataset labeled "big."

---

## 7. Analytics Preview (Conceptual — Step 9)

The dataset supports three natural follow-up modeling paths. Each is
**previewed conceptually**, not built in Project 1:

**Classification** — Predict "high vs low engagement" (label defined by
median split, not by the target metric itself — data-leakage guardrail).
Features: platform, sentiment, `has_hashtag`, `followers_count`. Train/test
split mandatory. Output: confusion matrix + accuracy.

**Clustering** — Group posts by standardized (likes, shares, comments) to
discover engagement archetypes. Try k=3 to start. Interpret cluster centers
as "low / mid / high engagement segments."

**Regression** — Predict likes from platform, sentiment, and log(followers).
Use log(likes) as the target given right-skew. Train/test split mandatory.
Regression is the **strongest fit** here because it quantifies *which
factors predict engagement* — directly answering the business question.

---

## 8. Limitations (Consolidated)

1. No `reach` or `impressions` → **engagement rate is not computable**.
2. `followers_count` missing for **88 posts** (4% of cleaned data).
3. **Sentiment labels** are pre-supplied by the dataset — provenance unknown.
4. The dataset is **not Pakistan-specific** — findings are directional and
   should be re-validated on the brand's own data.
5. Only 2,200 cleaned rows — adequate for EDA but not for training a
   reliable predictive model. Large-scale ML would need 50k+ rows.
6. Scaling results are measured on Colab hardware (single VM) and should
   be re-benchmarked on any deployment target.

---

## 9. Conclusion

A 2,260-row social-media dataset was cleaned (case normalization,
deduplication, sentinel-value handling), explored with median-based
comparisons across platform / sentiment / country / day-of-week, and
benchmarked at up to 2 million rows to establish where Pandas suffices
and where Spark would help. The data strongly suggests prioritizing
**TikTok**, **positive sentiment**, and **Thursday** as the highest-leverage
combination. A 4-week controlled test would convert these dataset-level
patterns into brand-level validation — and adding paid-campaign data
(spend, impressions, clicks) would enable ROAS analysis in Project 2.

---

**Exit question:** *Which technique best answers the marketing question?*
→ **Regression** — because "how many likes can we expect from this post,
given platform + sentiment + audience?" is a numerical prediction problem
that directly informs content-investment decisions.
