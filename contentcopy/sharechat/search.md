# Search

## Search Relevance Improvement  

**Situation**  
ShareChat’s initial search experience relied on a **simple token-matching algorithm**, which struggled with multi-word queries, misspellings, and contextual retrieval. This led to irrelevant search results and low user engagement.  

**Task**  
Enhance search relevance by adopting a more robust retrieval mechanism that could handle **n-grams, partial matches, and contextual overlaps**, thereby improving the user experience and interaction rate with search results.  

**Action**  
- Migrated the search pipeline from **exact token matching** to **n-gram based retrieval** in Elasticsearch.  
- Tuned n-gram sizes and similarity thresholds to balance recall and precision for diverse languages and query types.  
- Benchmarked query–document similarity with offline evaluation (precision@k, recall@k).  
- Ran online experiments measuring interacted searches (clicks/engagement after search).  

**Result**  
- Achieved **+16% uplift in interacted searches**, indicating significantly better relevance.  
- Improved discoverability of long-tail and misspelled queries, making the platform more engaging for users.  

---

## Search Ranking Improvement  

**Situation**  
Even with improved retrieval, search results lacked a **ranking layer**, leading to random ordering of candidates. This harmed user experience because highly relevant documents were buried below less useful ones.  

**Task**  
Design a **two-stage ranking framework** that would reorder retrieved results based on user engagement signals and contextual features, improving perceived relevance and satisfaction.  

**Action**  
- Implemented a **first-stage ranker** using lightweight counter-based features (click counts, impressions, CTR).  
- Introduced a **second-stage engagement-aware ranker** incorporating interaction features (likes, shares, dwell time).  
- Optimized ranking model for **Mean Reciprocal Rank (MRR)** to ensure top-ranked results matched user intent.  
- Validated improvements with both **offline MRR benchmarks** and **online A/B tests**.  

**Result**  
- Improved **MRR by +45%**, ensuring relevant results consistently surfaced in the top ranks.  
- Increased user trust in search, reflected in higher repeat usage of the feature.  

---

## Search as a Service  

**Situation**  
Different product teams at ShareChat (e.g., content discovery, creators, hashtags) were independently building search solutions, leading to **redundant infra, inconsistent quality, and lack of scalability**.  

**Task**  
Develop a **centralized Search Service** that could power all search use cases on the platform, ensuring consistency, scalability, and ease of integration for multiple teams.  

**Action**  
- Designed and implemented a **platform-wide Search Service** with modular APIs supporting different entity types (posts, users, tags).  
- Standardized retrieval (ElasticSearch backend) and ranking pipelines across all use cases.  
- Established monitoring dashboards and offline evaluation pipelines to measure precision, recall, and latency.  
- Coordinated rollout across multiple verticals, migrating legacy search systems to the new centralized service.  

**Result**  
- Enabled **a single scalable Search Service** used across all teams.  
- Reduced duplicate engineering effort and infra costs.  
- Provided a unified and improved search experience, strengthening ShareChat’s discovery ecosystem.  
