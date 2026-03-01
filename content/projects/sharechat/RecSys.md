

## Content Curation Revamp  

**Situation**  
ShareChat’s recommendation recall pipeline was heavily dependent on pre-defined candidate generation, which often failed to capture the evolving lifecycle of posts and did not fully leverage embedding maturity. The absence of robust offline evaluation metrics (true recall, NDCG) also made it hard to judge whether recall models were truly improving relevance.  

**Task**  
Redesign candidate corpus generation to better reflect post maturity, fix dataset issues impacting model training, and set up stronger evaluation metrics that connect offline performance to online retention.  

**Action**  
- Audited corpus generation pipelines and identified biases caused by immature post embeddings and incorrect dataset sampling. Fixed corpus filtering and lifecycle-aware sampling to ensure fair representation of posts across maturity stages.  
- Introduced **true recall** and **NDCG@K** as evaluation metrics for recall layer relevance. This allowed testing not just on AUC but on ranking sensitivity.  
- Experimented with adding **watch-time based signals** (instead of engagement-only signals like likes/favs) to improve recall for “watch-worthy” posts.  
- Collaborated with infra teams to integrate these metrics into dashboards, enabling continuous tracking across variants.  
- Deployed multiple A/B tests with revamped corpus strategies and signal enrichments.  

**Result**  
- Fixed training data bugs improved model ROC AUC by **+2% absolute**.  
- Recall/NDCG evaluation pipeline became standard for new candidate generators, ensuring offline–online alignment.  
- Deploying the best variant with watch-time enriched signals improved retention by **+0.4% absolute (~150k DAUs daily uplift)**.  

---

## Recent Events-based Candidate Generator  

**Situation**  
The personalization stack relied on a **costly personalization layer**, which slowed down feed serving and was not always accurate. Candidate generators often failed to capture “short-term” user intent (e.g., if someone binge-watched a comedy clip, the system was slow to adapt).  

**Task**  
Build a **lightweight, high-precision candidate generator** based on recent user interactions that could replace the expensive personalization layer while improving recall quality.  

**Action**  
- Designed a **post-post cosine similarity model** using normalized embeddings, queried via ShareChat’s Hamsa ANN index.  
- Built an event-driven system that used most recent user interactions (e.g., last N video plays) to fetch nearest-neighbor posts in real-time.  
- Benchmarked against existing FFM/ISP candidate generators on **recall@100** and **time-to-surface** metrics.  
- Supervised a **3-member team** (1 MLE, 1 SDE, 1 Analyst), dividing responsibilities across model implementation, infra optimization, and experiment analytics.  
- Ran online experiments comparing engagement/time-spent against the old personalization-heavy stack.  

**Result**  
- Successfully replaced the personalization layer, cutting infra cost by **INR 8M per month**.  
- Improved online user metrics: **+0.5% increase in per-user time spent**, showing higher relevance of fresh content.  
- Established this as a blueprint for future “recency-aware” candidate generators.  

---

## Learnt Weight Tuning  

**Situation**  
Candidate generators and rankers used **hand-tuned or heuristically fixed weights** to combine multiple signals (views, likes, shares, SVP, etc.). These manual weights often led to sub-optimal recall quality and made A/B testing inconsistent.  

**Task**  
Develop a **data-driven weight tuning framework** that could learn optimal weights across signals, evaluate offline and online, and ship variants that improve retention and engagement.  

**Action**  
- Implemented **bounded randomization logging** in production to collect unbiased training data across weight distributions.  
- Trained multiple models (Linear, Logistic Regression, GBDTs, Statistical functions) on randomized data to predict optimal weight combinations.  
- Designed offline evaluation pipelines (ROC AUC, NDCG, Recall) to rank candidate weight sets, inspired by sensitivity-optimised A/B metrics.  
- Shipped **two successful weight tuning variants** after A/B testing, carefully balancing Type-I/II error control for experiment decisions.  

**Result**  
- Shipped new weights that improved **absolute retention by +0.1%**.  
- Boosted **post views by +2%**, showing stronger recall quality without overfitting to engagement-only signals.  
- Institutionalized **learnt weight tuning** as a systematic process in the CG pipeline, later extended into real-time FFM weight tuning.  
