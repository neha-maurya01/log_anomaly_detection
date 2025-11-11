# HDFS Log Anomaly Detection

Overview
- This workspace contains notebooks and a structured HDFS log dataset used to build and evaluate a log-anomaly detection pipeline based on TF‑IDF vectorization and clustering.
- Main files:
  - [log_anomaly.ipynb](log_anomaly.ipynb) — exploratory notebook with preprocessing, vectorization, clustering and representative extraction.
  - [Log_file_model.ipynb](Log_file_model.ipynb) — (model notebook) additional experiments and model steps.
  - [HDFS_100k.log_structured.csv](HDFS_100k.log_structured.csv) — structured HDFS log dataset used by the notebooks.

Key components (defined in log_anomaly.ipynb)
- Data structures and variables:
  - [`struct_log`](log_anomaly.ipynb) — original DataFrame loaded from [HDFS_100k.log_structured.csv](HDFS_100k.log_structured.csv).
  - [`data_df`](log_anomaly.ipynb) — aggregated DataFrame of BlockId → EventSequence.
  - [`X`](log_anomaly.ipynb) — numeric matrix after term-frequency, TF‑IDF and normalization.
- Preprocessing & feature extraction:
  - [`term_frequency`](log_anomaly.ipynb) — builds per-sequence event counts.
  - [`tf_idf`](log_anomaly.ipynb) — converts counts to TF‑IDF.
  - [`normalise`](log_anomaly.ipynb) — column-wise mean-centering.
  - [`preprocessing`](log_anomaly.ipynb) — wrapper that returns the processed matrix `X`.
- Clustering & similarity:
  - [`distance_metric`](log_anomaly.ipynb) — cosine-distance implementation used as the similarity metric.
  - [`get_min_cluster_dist`](log_anomaly.ipynb) — finds nearest cluster representative for an instance.
  - [`get_representative`](log_anomaly.ipynb) — returns the representative log index closest to a centroid.

How to run
1. Open [log_anomaly.ipynb](log_anomaly.ipynb) or [Log_file_model.ipynb](Log_file_model.ipynb) in VS Code or Jupyter.
2. Ensure dependencies are installed:
   - pandas, numpy, regex, scikit-learn, scipy, matplotlib
   - Example:
     ```sh
     pip install pandas numpy regex scikit-learn scipy matplotlib
     ```
3. Run cells sequentially. The notebook reads [HDFS_100k.log_structured.csv](HDFS_100k.log_structured.csv) and executes the preprocessing and clustering pipeline.

Notes & tips
- The notebook extracts block IDs from the `Content` column using regex and builds event sequences per block.
- Clustering is initially done on a bootstrap subset (`X_bootstrap`) to compute initial representatives; the rest of the data is assigned incrementally.
- Adjust `max_dist` and bootstrap size to tune cluster granularity and runtime/memory footprint.

References
- Open the implementation and helper functions in: [log_anomaly.ipynb](log_anomaly.ipynb)
- Dataset: [HDFS_100k.log_structured.csv](HDFS_100k.log_structured.csv)

