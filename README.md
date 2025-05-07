# NASA Web Logs – User Behavior & Error Analysis

**Submitted for:** Optus Data Science Technical Interview  
**Prepared by:** Satabdi Dash  

## Objective
Analyze ~3 million NASA web access logs to uncover user behavior patterns, detect anomalies, and offer actionable recommendations to improve website experience using only server-side log data.

---

## Table of Contents

- Step 2: Data Loading & Structure Validation
- Step 3: Data Cleaning & Feature Engineering
- Step 4: Basic EDA
- Step 5: Intermediate Behavior Analysis
- Step 6: Asset Group Insights
- Step 7: Advanced Modeling
- Step 8: Final Recommendations
- Step 9: Implementation Roadmap

---

## Step 2 – Pandas-Based Processing

- Loaded ~2.96M rows from Parquet using `dask`
- Switched to `pandas` for modeling and analysis
- Validated structure: 7 columns, no missing values, log format with `host`, `url`, `method`, `response`, `bytes`, and `time`

---

## Step 3.1 – Data Cleaning

- Removed `Unnamed: 0` index column
- Converted UNIX `time` to datetime
- Filtered malformed rows and invalid HTTP methods
- Removed 0-byte records
- Cleaned and validated URL structure

---

## Step 3.2 – Feature Engineering

- Extracted: `hour`, `weekday`, `month`
- Session-level: `session_id`, `session_duration`, `pages_per_session`, `is_bounce`
- Parsed `file_extension` and created `asset_group`
- Created size buckets from `bytes`
- Derived: `entry_url`, `exit_url`, `z_score`, `error_count_per_session`

---

## Step 4 – Basic EDA

- Top URLs: `/`, `/ksc.html`, branding GIFs
- 90% of requests return HTTP 200; ~4% return errors (404, 304)
- Request sizes: Mostly <10KB, max = 6.8MB
- Top hosts include AOL proxies and `.edu` domains

---

## Step 5 – Intermediate Analysis

- Identified top 10 hosts and error-prone URLs
- 404s mostly caused by missing images
- Detected long-tail size distribution
- Recommendations: repair missing assets, bundle static content, throttle known error hosts

---

## Step 6 – Asset Group Insights

- Grouped by `asset_group` using file extensions
- 65% of traffic = `image` group
- Highest error rates in `text/html` files (~18%)
- Proxy-related 304s are common but non-critical
- Used asset grouping to prioritize caching and content delivery fixes

---

## Step 7 – Advanced Analysis Overview

Advanced ML and behavioral modeling components:
- Step 7.1: Time-based anomaly detection (Z-score)
- Step 7.2: Sessionization
- Step 7.3: Clustering (KMeans)
- Step 7.4: Bounce Prediction (Random Forest)
- Step 7.5: Market Basket Analysis
- Step 7.6: Error Diagnostics

---

### Step 7.1 – Time-Based Anomaly Detection

- Calculated daily request volume and z-scores
- July 13 & August 29 flagged as anomalies (Z > 3)
- Related to crawler traffic or external events
- Enabled proactive spike detection and monitoring

---

### Step 7.2 – Sessionization Analysis

- Sessions defined by 30-minute inactivity gap
- Extracted: `session_id`, `entry_url`, `exit_url`, `duration`, `bounce`
- 70% of sessions lasted under 5 minutes
- Bounce rate = 9.7%

---

### Step 7.3 – Clustering (User Segmentation)

- KMeans (K=4) on normalized session metrics
- Cluster 1: Bouncers
- Cluster 2: Power Users
- Cluster 3: Error-Prone
- Cluster 4: Balanced Users
- PCA visualization supported UX segmentation

---

### Step 7.4 – Bounce Prediction Model

- Model: Random Forest Classifier
- Accuracy: 91%
- Recall for bouncers: 19%
- Features: `entry_url`, `time`, `asset_group`, `request size`, etc.
- Used to trigger early UI intervention

---

### Step 7.5 – Market Basket Analysis

- Used Apriori algorithm on URL paths
- Example rule: `/ksc.html` → `/NASA-logosmall.gif`
- Used for smart asset prefetching and bundling
- Exported to CSV

---

### Step 7.6 – Error Diagnostics

- Mapped response codes by host
- 404s mostly caused by missing static assets
- 304s common from proxy IPs
- Heatmap of host × error used for targeted remediation

---

## Step 8 – Final Recommendations

### Behavioral Insights

- Bounce rate: 9.7%
- Peak usage: 12PM and 8PM
- Power user clusters show longer engagement
- Market basket rules reveal content pathways
- Error-prone hosts and assets clearly identifiable

### Strategic Recommendations

1. Redesign high-bounce entry pages (`/`, `/ksc.html`)
2. Predict bounce in real time and trigger UI nudges
3. Prefetch and bundle assets based on association rules
4. Personalize content per cluster
5. Schedule content for peak hours
6. Fix 404s (missing logos, assets)
7. Use z-score-based traffic anomaly monitoring
8. Retrain model biweekly and integrate flags into CMS

### Optimization by Category

| Area             | Recommendation                     | Impact                        |
|------------------|-------------------------------------|-------------------------------|
| Bounce Behavior  | Predict & intervene                 | Reduced exits                 |
| Entry UX         | Redesign `/ksc.html` and `/`        | Higher session time           |
| Static Assets    | Prefetch logos/images via MBA       | Faster UX, fewer reloads      |
| Error Handling   | Fix 404 `.gif` links                | Better reliability            |
| Traffic Peaks    | Schedule content @ 12PM/8PM         | Better engagement             |
| Monitoring       | Alert on Z-score spikes             | Catch crawler/DDOS issues     |

---

## Step 9 – Implementation Roadmap

| Phase   | Action                                      | Team         | Timeline |
|---------|---------------------------------------------|--------------|----------|
| Phase 1 | Deploy bounce model                         | DS + Frontend | Week 1–2 |
| Phase 2 | Redesign `/ksc.html` & homepage             | UX            | Week 2–3 |
| Phase 3 | Repair broken image assets                  | Infra/Dev     | Week 2–3 |
| Phase 4 | Prefetch and bundle static assets           | Infra         | Week 3–4 |
| Phase 5 | Personalize by clustering                   | Backend       | Week 4   |
| Phase 6 | Implement anomaly detection (Z-score)       | DevOps        | Week 4–5 |
| Phase 7 | Align campaigns with peak traffic hours     | Marketing     | Ongoing  |
| Phase 8 | Automate reporting for sessions/errors      | Analytics     | Week 5+  |

---

## Summary

This project demonstrates how to:

- Understand user behavior from raw logs  
- Predict bounce sessions and act early  
- Segment users for personalized UX  
- Detect traffic anomalies and bot spikes  
- Optimize delivery of static content  
- Improve error resilience and load efficiency  

All achieved using scalable, privacy-friendly methods with no tracking or cookies — only raw logs and machine learning.
