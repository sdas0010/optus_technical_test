## Glossary of Terms

This section explains the key data science, behavioral, and engineering concepts used in the NASA Access Log Analysis project.

---

### Sessionization  
**Definition**:  
Grouping sequential user requests into "sessions" using a 30-minute inactivity threshold.  
**Use**: Captures user journeys, enables behavior modeling (e.g., bounce detection, clustering).

---

### Bounce Rate  
**Definition**:  
The percentage of sessions with only one page request.  
**Formula**: `bounces / total_sessions`  
**Use**: High bounce rates signal poor engagement or irrelevant entry pages.

---

### Bounce-Heavy Sessions  
**Definition**:  
Sessions that ended after a single request.  
**Use**: Identified `/ksc.html` and `/` as bounce-prone; targeted for redesign.

---

### Power Users  
**Definition**:  
Users with long sessions, many requests, and strong engagement signals.  
**Use**: Segmented via clustering to deliver in-depth mission content.

---

### Error-Prone Sessions  
**Definition**:  
Sessions with a high ratio of failed HTTP responses (e.g., 404s, 403s).  
**Use**: Guided broken link resolution and fallback asset strategies.

---

### Long-Tail Assets  
**Definition**:  
Assets with low frequency but high byte size or load time.  
**Use**: Identified using `bytes_bucket`; helps optimize caching.

---

### Session Duration  
**Definition**:  
Total time between the first and last request in a session.  
**Use**: Behavioral signal for clustering, bounce prediction, and user profiling.

---

### Z-Score Anomaly Detection  
**Definition**:  
Identifies request volume spikes by comparing daily counts to the mean using standard deviation.  
**Formula**: `z = (x - mean) / std`  
**Use**: Detected anomalies on July 13 and August 29.

---

### KMeans Clustering  
**Definition**:  
An unsupervised algorithm that groups sessions based on behavioral similarity.  
**Use**: Segmented users into 4 clusters (bouncer, power, error-prone, balanced).

---

### PCA (Principal Component Analysis)  
**Definition**:  
Dimensionality reduction technique to project features into 2D for visualization.  
**Use**: Enabled visual interpretation of clustered user behavior.

---

### Random Forest Classifier  
**Definition**:  
An ensemble ML model that uses multiple decision trees for classification.  
**Use**: Predicted bounce likelihood using features like `entry_url`, `session_duration`, `asset_group`.

---

### Market Basket Analysis (Apriori Algorithm)  
**Definition**:  
Discovers association rules from co-accessed pages within a session.  
**Use**: Found patterns like `/ksc.html → /NASA-logosmall.gif` to drive prefetching and smart bundling.

---

### Entry/Exit URLs  
**Definition**:  
- **Entry URL**: First page visited in a session.  
- **Exit URL**: Last page before leaving or timing out.  
**Use**: Used to detect bounce points and guide content placement.

---

### Co-Access Patterns  
**Definition**:  
Pages frequently accessed together in the same session.  
**Use**: Informed recommendations and predictive caching logic.

---

### Asset Grouping  
**Definition**:  
Classifying URLs into content types like `image`, `html`, `text`, `misc`.  
**Use**: Analyzed traffic by content category to optimize performance and detect error-prone groups.

---

### Parquet Format  
**Definition**:  
Columnar storage format optimized for large datasets.  
**Use**: Reduced I/O overhead while loading 3M+ rows with Dask.

---

### Dask  
**Definition**:  
Parallel computing framework for scalable data processing.  
**Use**: Enabled efficient Parquet ingestion and preprocessing at scale.

---

### Peak Hour Optimization  
**Definition**:  
Scheduling content updates during peak traffic windows (e.g., 12PM, 8PM).  
**Use**: Informed A/B testing, campaign launches, and traffic load balancing.

---

### Anomaly Monitoring (Z-Score Based)  
**Definition**:  
Tracks spikes in volume or errors using standard deviation thresholds.  
**Use**: Flagged traffic anomalies and crawlers for alerting.
