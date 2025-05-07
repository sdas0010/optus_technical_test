## Methodology & Reasoning

This section explains the rationale behind key design choices made during the NASA Access Log Analysis project.

---

### Why Use Parquet Format?

**Reason**:  
Parquet is a columnar storage format that significantly reduces I/O overhead and memory usage when working with large datasets.

**Applied Because**:  
- The dataset contains over 2.9 million rows.  
- Parquet + Dask allowed efficient lazy loading and parallel access.

---

### Why Start with Dask and Switch to Pandas?

**Reason**:  
Dask scales well for initial profiling and preprocessing. Pandas is better for modeling and row-level transformations.

**Strategy**:  
- Dask was used to ingest and explore the raw data.  
- Once filtered and sampled, the dataset was moved to Pandas for feature engineering and ML.

---

### Why Feature Engineering on Session Behavior?

**Reason**:  
Log data doesn’t contain session boundaries or user labels by default.

**Created Features**:  
- `session_id`, `session_duration`, `is_bounce`, `pages_per_session`  
- Temporal features like `hour`, `weekday`, `month`  
- Content features like `asset_group`, `file_extension`, `bytes_bucket`

**Impact**:  
These features enabled clustering, segmentation, bounce prediction, and time-based anomaly tracking.

---

### Why Sessionization with 30-Minute Timeout?

**Reason**:  
30 minutes is a standard industry heuristic (e.g., Google Analytics) for splitting user sessions.

**Use**:  
Captured real behavioral sessions for engagement and bounce analysis.

---

### Why Clustering with KMeans?

**Reason**:  
To uncover latent user behavior patterns without requiring labels.

**Used Because**:  
- KMeans is interpretable, fast, and scalable.  
- Combined with PCA for visualization.  
- Helped identify power users, bouncers, and error-prone visitors.

---

### Why Bounce Prediction with Random Forest?

**Reason**:  
Random Forest handles non-linear relationships, handles mixed datatypes, and provides explainable output.

**Use Case**:  
Predicted which sessions were likely to bounce based on early signals like entry URL, asset type, and time.

**Why Not XGBoost?**  
RF is faster to train for prototyping and provides good performance with fewer hyperparameters.

---

### Why Z-Score for Anomaly Detection?

**Reason**:  
Z-score is a lightweight, statistically sound method to detect spikes in traffic or error volume.

**Use**:  
- Flagged anomalies on July 13 and August 29.  
- Useful for real-time alerting systems with minimal infrastructure.

---

### Why Market Basket Analysis (Apriori)?

**Reason**:  
To discover frequently co-accessed assets and pages within a session.

**Use**:  
Identified predictable user flows like:
- `/ksc.html → /NASA-logosmall.gif`  
- `/elv/*.gif → /elv/
