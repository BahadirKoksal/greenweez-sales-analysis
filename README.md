
# 🛒 Greenweez Sales Analysis

BigQuery SQL project analyzing sales performance across product categories for **Greenweez**, a French organic e-commerce brand.

---

## 🎯 Objective

Greenweez's product team needed a clear picture of category-level performance. This project answers:

- Which categories generate the most revenue?
- Which categories have the most repeat customers?
- What drives the success of the top revenue category?
- Which subcategories have the highest average purchase cost?

---

## 🗃️ Dataset

One table from BigQuery (`course14` dataset):

| Table | Rows | Description |
|-------|------|-------------|
| `gwz_sales` | 1,486,388 | One row per product per order with category, promo, and financial data |

Key columns in `gwz_sales`:

| Column | Description |
|--------|-------------|
| `date_date` | Sale date |
| `orders_id` | Order identifier |
| `products_id` | Product identifier |
| `customers_id` | Customer identifier |
| `category_1` | Top-level product category |
| `category_2` | Mid-level product category |
| `category_3` | Subcategory |
| `promo_name` | Promotion name (null if no promo) |
| `turnover_before_promo` | Revenue before discount (€) |
| `turnover` | Final revenue after discount (€) |
| `purchase_cost` | Product purchase cost (€) |
| `qty` | Quantity sold |

---

## 📋 Project Steps

### 1. Data Exploration
Previewed the table to understand structure, volume, column types, and the 3-level category hierarchy.

### 2. Overall Statistics
Pulled a single-row summary of the entire dataset using aggregation functions:

| Metric | Value |
|--------|-------|
| Total orders | 178,974 |
| Unique products | 16,982 |
| Unique customers | 105,360 |
| Total turnover | ~€13,698,013 |
| Total purchase cost | ~€9,265,498 |
| Total quantity sold | 2,471,245 |

### 3. Category_1 Analysis
Grouped the same metrics by `category_1` to compare all 11 top-level categories side by side, then ranked by `sum_turnover` to identify the top performer.

> **Finding:** `Bébé & Enfant` is the #1 revenue category (~€3,258,352) despite ranking 3rd in order volume — driven by high unit prices.

### 4. Subcategory Deep-Dive — Bébé & Enfant
Drilled into `category_2` and `category_3` within `Bébé & Enfant` across three dimensions:

**Revenue breakdown** — which subcategories generate the most turnover?
- Top: Couches jetables T4, 8-14 kg (~€432,430)
- Runner-up: Lait infantile (~€392,553)

**Repeat purchase rate** — `nb_orders / nb_customers` per subcategory:

| Subcategory | Orders/Customer |
|-------------|----------------|
| Lait de chèvre bébé | 2.19x |
| Boisson végétale bébé | 2.14x |
| Lait infantile | 2.04x |
| Couches jetables T5-6 | 1.77x |

**Average purchase cost** — which subcategories have the highest unit cost?
- Top: Equipement bébé — Sécurité bébé (€117.88 avg)
- Runner-up: Boisson végétale bébé (€66.93 avg)

---

## 📊 Key Findings

- **Top revenue category:** Bébé & Enfant (~€3.26M) — high unit price drives revenue despite lower order volume
- **Most orders:** Epicerie salée (80,064 orders) but ranks 2nd in revenue — lower average order value
- **Highest repeat purchase:** Baby food and infant formula (2x+ reorder rate) — consumable products create strong customer loyalty
- **Diapers are a dual engine:** High revenue (~€432K for a single subcategory) AND strong repeat purchase (~1.7x) — the most strategic product group
- **Most expensive subcategory:** Baby safety equipment (€117.88 avg purchase cost) — high margin potential

---

## 🛠️ SQL Techniques Used

| Technique | Purpose |
|-----------|---------|
| `COUNT(DISTINCT ...)` | Counting unique orders, products, customers |
| `SUM()` | Total turnover, purchase cost, quantity |
| `AVG()` | Average purchase cost per subcategory |
| `GROUP BY` | Grouping by one or multiple category levels |
| `ORDER BY ... DESC` | Ranking categories by revenue |
| `LIMIT 1` | Isolating the single top-performing category |
| `WHERE` | Filtering to a specific category for deep-dive |
| Derived metric | `nb_orders / nb_customers` = repeat purchase rate |

---

## 🔧 Tools

- **Google BigQuery** — SQL engine and data warehouse
- **SQL** — All analysis done in standard SQL with BigQuery-specific functions
