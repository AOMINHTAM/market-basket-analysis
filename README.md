# 🛒 Market Basket Analysis & Product Bundling Strategy

An algorithmic E-commerce analytics project utilizing the **Apriori Algorithm (Association Rules)** to discover hidden purchasing patterns, frequent itemsets, and high-conversion cross-sell combinations. The extracted insights power UI/UX recommendation engines and strategic product bundling.

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
* **Lift:** **4.25** (Customers buying the antecedent are 4.25x more likely to buy the consequent compared to baseline probability).
* **Confidence:** **45%** of customers who add Candles and Masks will proactively add Nail Care or Bath Oils to close the cart.

### 2. Emerging Shopping "Ecosystems"
* **Ecosystem A - "Home Spa":** A purely experiential basket comprising *Candles + Face Masks + Bath Oils + Nail Care*. These items are bought together to simulate a weekend wellness retreat.
* **Ecosystem B - "Beauty from Within":** A wellness-driven basket combining *Vitamins/Supplements + Hand Creams + Face Care*. Indicates a highly disciplined customer demographic focused on holistic self-care.

---

## 🚀 4. Actionable Business Recommendations

1. **Strategic Product Bundling:** Launch the **"Weekend Reset Kit"** packaging Ecosystem A items together with a 10% bundle discount, effectively forcing a higher Average Order Value (AOV).
2. **Dynamic UI Cross-Selling:** Configure the checkout logic: When a user adds *Candles*, dynamically trigger a pop-up recommending *Bath Bombs* rather than displaying competing candle brands.
3. **Content Marketing Alignment:** Direct the social media team to produce short-form video content titled *"The 4-Step Post-Deadline Self-Care Routine"*, seamlessly placing the exact 4 items from Ecosystem A into the narrative.

---

## 🛠️ 5. Technical Audit & Iterative Improvements

During the analytical sprint and peer code review, several critical data engineering bottlenecks were identified and resolved to ensure production readiness:

### 1. Out-of-Memory (OOM) Prevention via Support Tuning
* **The Risk:** The initial `min_support` parameter was set to `0.001` (0.1%). On large-scale transactional datasets, this exponentially generates millions of insignificant frequent itemsets, overloading system RAM and causing kernel crashes.
* **The Fix:** Recalibrated `min_support` to `0.005` (0.5%), striking a balance between discovering niche long-tail rules and ensuring robust computational stability.

### 2. In-Basket Duplication Noise
* **The Mistake:** Grouping items directly to lists allowed duplicate subcategories within the same `OrderID` (e.g., buying 3 different types of *Candles* registered as `[Candles, Candles, Candles]`).
* **The Fix:** Refactored the aggregation logic using `.unique()` to strictly deduplicate items at the basket level prior to passing them into the `TransactionEncoder`.

### 3. Missing Exploratory Data Analysis (EDA)
* **The Mistake:** Executing the machine learning algorithm immediately without understanding the underlying dataset topology.
* **The Fix:** Engineered an EDA pipeline preceding the Apriori execution, generating subplots to visualize the *Top 10 High-Velocity Subcategories* and the *Distribution of Unique Items per Basket*.