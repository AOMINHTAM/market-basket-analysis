# 🛒 Market Basket Analysis & Product Bundling Strategy

> 🌐 **Portfolio Case Study:** [View full Case Study & Association Thresholds →](https://aominhtam-portfolio.vercel.app/projects/market-basket.html)

An e-commerce market basket mining case study utilizing the **Apriori Algorithm (Association Rules)** across **25,728 unique order baskets** to identify co-occurring product affinities and design A/B-testable bundle hypotheses.

---

## 📦 Dataset & Association Rules Scope

* **Project Type:** Personal Project / Simulated Business Case
* **Dataset Source:** Simulated E-Commerce CSVs (`EcomSales`, `Product`)
* **Granularity & Deduplication:** `51,290` line-items grouped into `25,728` unique `OrderID` baskets; duplicate `Subcategory` entries removed per basket via `.unique()` prior to `TransactionEncoder`.
* **Thresholds (Verified in Code):** `min_support = 0.005` (&ge;129 co-occurring orders), `min_confidence = 0.1`, and `lift > 1.2`.
* **Caveats:** `EcomSales.csv` lacks a return/refund table, and **association rules measure historical co-occurrence—not causal incremental AOV lift** (requires controlled A/B testing).

---

## 🎯 1. Business Context & Strategic Objectives

* **Business Scenario:** The e-commerce platform aims to increase the Average Order Value (AOV) by shifting away from random product recommendations to data-driven cross-selling.
* **Analytical Objectives:**
  * Perform Exploratory Data Analysis (EDA) to understand basket size distributions and top-moving subcategories.
  * Apply Association Rules mining to calculate Support, Confidence, and Lift for product combinations.
  * Identify cohesive "Shopping Ecosystems" to design compelling Product Bundles (e.g., *Weekend Reset Kit*).

---

## 🛠️ 2. Tech Stack & Methodology

* **Language:** Python 3.x
* **Core Libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `mlxtend`.
* **Methodology:** 
  * Data preprocessing and basket grouping via `groupby()` and One-Hot Encoding (`TransactionEncoder`).
  * Threshold tuning for `min_support` and `min_threshold` to extract statistically significant rules while preventing algorithm memory crashes.

---

## 📈 3. Key Findings & Behavioral Insights

### 1. The Strongest Product Linkage (Highest Lift)
* **Rule:** `[Candles, Room Sprays, Face Masks] ---> [Nail Care, Bath Oils/Bombs]`
* **Lift:** **4.25** (Customers buying the antecedent set are 4.25x more likely to co-purchase the consequent set compared to baseline probability).
* **Confidence:** **45%** of baskets containing **Candles, Room Sprays, and Face Masks** also include Nail Care or Bath Oils/Bombs within the same `OrderID`.

### 2. Emerging Shopping "Ecosystems"
* **Ecosystem A - "Home Spa":** A purely experiential basket comprising *Candles + Face Masks + Bath Oils + Nail Care*. These items are bought together to simulate a weekend wellness retreat.
* **Ecosystem B - "Beauty from Within":** A wellness-driven basket combining *Vitamins/Supplements + Hand Creams + Face Care*. Indicates a highly disciplined customer demographic focused on holistic self-care.

---

## 🚀 4. Proposed Commercial Experiments & Validation

1. **Bundle Pilot ("Weekend Reset Kit"):** Test packaging Ecosystem A items with a 10% bundle discount against standalone pricing in a 50/50 checkout A/B test to measure **Incremental Gross Profit per Order**, **Basket Attach Rate**, and **AOV Lift**.
2. **Dynamic UI Cross-Selling:** When a user adds *Candles* to cart, test triggering a complementary prompt for *Bath Bombs* instead of competing candle brands.
3. **Content Marketing Alignment:** Align short-form campaign creatives around the 4-item Ecosystem A self-care routine.

---

## 🛠️ 5. Methodology & Analysis Fixes

Several data engineering guardrails were added prior to publishing:

### 1. Out-of-Memory (OOM) Prevention via Support Tuning
* **The Risk:** The initial `min_support` parameter was set to `0.001` (0.1%). On large-scale transactional datasets, this exponentially generates millions of insignificant frequent itemsets, overloading system RAM and causing kernel crashes.
* **The Fix:** Recalibrated `min_support` to `0.005` (0.5%), striking a balance between discovering niche long-tail rules and ensuring robust computational stability.

### 2. In-Basket Duplication Noise
* **The Mistake:** Grouping items directly to lists allowed duplicate subcategories within the same `OrderID` (e.g., buying 3 different types of *Candles* registered as `[Candles, Candles, Candles]`).
* **The Fix:** Refactored the aggregation logic using `.unique()` to strictly deduplicate items at the basket level prior to passing them into the `TransactionEncoder`.

### 3. Missing Exploratory Data Analysis (EDA)
* **The Mistake:** Executing the machine learning algorithm immediately without understanding the underlying dataset topology.
* **The Fix:** Engineered an EDA pipeline preceding the Apriori execution, generating subplots to visualize the *Top 10 High-Velocity Subcategories* and the *Distribution of Unique Items per Basket*.