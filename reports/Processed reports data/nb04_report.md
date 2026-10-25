# NB04 — SCALING EXPERIMENT

## Results (measured on Google Colab)

 target  actual  build_s  groupby_s  memory_mb
   2000    2200    0.000      0.024        1.4
  20000   19800    0.002      0.005       12.9
 200000  198000    0.034      0.036      128.7
2000000 1999800    0.308      0.355     1299.7

## Observation
At 1,999,800.0 rows, Pandas required 0.355s for a groupby-median, with 1300 MB memory footprint.

## Would Spark help? — measured justification

**At 2M rows: NO.** Pandas completed in 0.355s on a single machine. Spark's JVM startup + shuffle overhead would exceed any compute time saved.

**At 50M+ rows (or multi-TB datasets): LIKELY YES**, for three reasons:
1. Distributed storage (HDFS/S3) avoids loading the whole dataset into RAM
2. Parallel groupby across machines
3. Fault tolerance during long-running jobs

**Conclusion:** For this project's 2,200-row dataset, Pandas is correct. Spark should be introduced when data volume, latency, or distributed-storage requirements actually justify the added complexity — not by default.
