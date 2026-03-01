## User Fatigue Model  

**Situation**  
Ads were ShareChat’s primary revenue driver, but a **fixed ad-load policy (2 ads at slots 3 and 7)** ignored user heterogeneity. Many users experienced **fatigue**, leading to churn and reduced retention. Internal analysis showed that retention loss from excessive ads could outweigh revenue gains.  

**Task**  
Develop a **predictive fatigue model** that quantifies churn probability, enabling dynamic tuning of ad load to balance **retention vs revenue**.  

**Action**  
- Collected and cleaned a **large-scale session dataset** (1M+ users, multiple engagement and session-level features).  
- Engineered features for both **short-term fatigue (session drop-off)** and **long-term churn probability** (no login in next 3 days).  
- Trained multiple models (Logistic Regression, Multi-Layer Perceptron, XGBoost), comparing predictive performance across ROC-AUC and PR curves.  
- Validated fatigue score correlation with retention, time-spent, and number of posts viewed.  
- Applied fatigue score thresholds to tweak ad-load dynamically on the **Landing Feed** (e.g., 3 ads for low-fatigue users, 1 ad for high-fatigue users).  
- Ran A/B tests to measure trade-offs across **retention, ads revenue, and user engagement**.  

**Result**  
- Improved user retention by **+0.1% absolute** (translating to tens of thousands of users retained daily).  
- This came at the cost of **0.2% revenue drop (~INR 300k per month)**, but validated that controlled ad load can safeguard user base longevity.  
- Established **fatigue score** as a core signal for all future ad personalization efforts.
- Copulised a paper on this in SIGIR'23 conference. Here is the link to the paper - https://drive.google.com/file/d/1Zfb1HBNCHKvVMsVNem_zuTS6LKJSh7-7/view?usp=sharing. 

---

## Ad Policy Intervention (Contextual Bandits)  

**Situation**  
After fatigue-based thresholds, ad-load decisions were still **rule-based** and not context-sensitive. Different users, feeds, and sessions required different policies. There was no systematic way to balance **ad revenue and user satisfaction** dynamically.  

**Task**  
Design a **Contextual Bandit-based ad policy system** where ad policies (arms) represent different ad-load configurations, and the model learns to optimize a **multi-objective reward** (user satisfaction + ad revenue).  

**Action**  
- Modeled ad-load personalization as **Contextual Bandits**:  
  - **State (S):** user features (age, language, platform age, followers), session features (time spent, feeds scrolled), feed type.  
  - **Action (A):** ad policy (number of ads, positions in feed).  
  - **Reward (R):** weighted combination of ad impressions (revenue) and session continuation (satisfaction).  
- Used **random ad-slot experiments** (baseline policies) to build an offline dataset for **off-policy evaluation**.  
- Tested multiple learning approaches (Logistic Regression, Neural Networks) with **Inverse Propensity Weighting (IPW)** and **Doubly Robust (DR)** estimators for unbiased offline evaluation.  
- Shipped a **Neural Network + DR objective model** into production on VSF (Video Suggested Feed) with 1% traffic.  
- Designed multi-objective trade-offs via **linear scalarization and F1-style balancing** between ads and satisfaction.  

**Result**  
- Achieved **INR 2M per month incremental ad revenue** while keeping retention and engagement neutral to positive.  
- Proved that **bandit-based ad policy selection** can dynamically optimize across conflicting objectives.  
- This framework became the foundation for future **multi-objective reinforcement learning** efforts in ads optimization.  
