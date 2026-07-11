# Capstone Report: Predicting Content Decay
**A Proactive Decision-Support Tool for SEO Teams**

## 1. Abstract
Content teams often struggle to monitor thousands of pages manually, leading to reactive fixes only after search traffic and revenue are already lost. Using the FlyRank ML Internship dataset, this project sought to predict which articles are at the highest risk of traffic decay (where `trend_direction == 'down'`). By extracting historical search performance and engagement features, we trained a Random Forest classification model to output decay probabilities. These probabilities were then used to rank pages, creating a prioritized action queue. The resulting model outperforms fixed, rule-based baselines, offering a scalable **decision-support tool** for editorial teams.

## 2. Data & Safety
This research is built on the **FlyRank ML Internship dataset** (`content_refresh_anonymized.csv`). 
* **Safety First:** To ensure public safety and privacy, all client IDs and content IDs have been strictly anonymized. No private client names, URLs, or raw search queries are present in this analysis.
* **Unit of Analysis:** One row represents a single piece of content for a specific client, evaluated over a trailing 90-day window.

## 3. Methodology
* **Label Definition:** Our target proxy is `trend_direction == 'down'`, representing an observed historical drop in search visibility.
* **Feature Engineering:** We utilized structural signals (`word_count`), historical performance (`impressions_90d`, `clicks_90d`, `ctr`), and velocity signals (`impressions_last_30d`).
* **Validation Design:** To prevent data leakage and simulate real-world conditions, we used a **Group-Based Split** (grouping by `client_id`). This ensures the model is evaluated on entirely unseen clients, proving its ability to generalize rather than just memorizing domain-specific data.
* **Model Choice:** A Random Forest Classifier was selected because content decay involves complex, non-linear interactions between multiple variables (e.g., CTR drops matter more for high-impression pages).

## 4. Results: Model vs. Baseline
When evaluated on unseen clients, the Machine Learning model significantly outperformed the traditional rule-based baseline (which relied on static age and impression thresholds).
* **Measured Performance:** The ML model observed a significantly higher **Precision** in the top-ranked results. This means fewer "false alarms," saving the editorial team from wasting time updating pages that do not actually need it.

## 5. Limitations & Honest Framing
It is critical to explicitly state what this model **cannot** do:
1. **No Causality:** This model uses cross-sectional observational data. We do not make any causal claims. We **cannot** prove that updating a page will guarantee a traffic increase, nor do we claim to have reverse-engineered any search engine algorithms.
2. **Missing Seasonality:** The data lacks seasonal context. A drop in traffic might simply be an "ice cream" article in December, rather than actual content decay.
3. **External Factors:** The model cannot measure competitor updates or broad algorithm shifts.

## 6. Action Playbook (Ranked Recommendations)
The model outputs a ranked queue intended strictly as a **decision-support tool**. It should never fully automate editorial actions. 

**Human-in-the-Loop Workflow:**
1. **High-Risk, High-Volume:** Pages flagged with a high decay probability AND high historical search volume are placed at the top of the queue (`declining_with_demand`).
2. **Review:** A human editor must check the top pages to rule out natural seasonality.
3. **Action:** If decay is structural, the page is sent for a content refresh (updating facts, improving intent match, or adding fresh insights).

## 7. Reproducibility
All code, exploratory notebooks, signal audits, and the final model pipeline are publicly available in the GitHub repository associated with this project. The notebooks are designed to run top-to-bottom without errors.
