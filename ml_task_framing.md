# ML Task Framing

**1. ML Task Type**
* This is a **Ranking** (and implicitly, **Scoring**) task. The model will calculate a relevance score for various items and then rank them to surface the highest-scoring items to the user.

**2. Target and Proxy**
* **Ideal Target:** Long-term user satisfaction and platform retention.
* **Proxy:** Since "satisfaction" is abstract and hard to measure in real-time, our measurable proxy is **User Engagement/Clicks**. If a user clicks on or interacts with an item presented in the recommendation carousel, we assume it was a successful and relevant recommendation.

**3. Success Metric**
* **Offline Metric (Technical):** **Precision@K** (e.g., Precision@5) or **NDCG**. These tell us how good the model is at placing the most relevant items at the very top of the list during our testing phase.
* **Online Metric (Business):** **Click-Through Rate (CTR)** on the recommendation carousel. If the ML model is working, more users will click these items compared to a random or generic baseline.

**4. Supported Content Action**
* **Output:** A ranked array of item IDs specific to a User ID.
* **Action Supported:** The front-end system automatically populates a personalized "Recommended for You" section on the user's dashboard, actively putting relevant content in front of them so they don't have to search for it.

**5. Why ML Beats a Fixed Rule**
* A fixed rule (e.g., `SELECT * FROM items ORDER BY views DESC LIMIT 5`) provides a "one-size-fits-all" solution. It shows the same popular items to everyone and completely ignores individual user tastes. 
* ML (such as collaborative filtering) inherently maps out hidden, non-linear relationships between users and items based on historical behavior. It allows us to mathematically deliver personalization at scale, dynamically adapting to a user's unique profile in a way that static `IF/THEN` rules or standard SQL queries cannot achieve.
