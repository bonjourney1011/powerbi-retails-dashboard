# Retails Dashboard — Portfolio Case Study
### Power BI | Data Modeling | DAX | Business Intelligence
**Author:** Mo Thuần &nbsp;|&nbsp; **Tool:** Microsoft Power BI Desktop &nbsp;|&nbsp; **Period:** 2016–2021

---

## Project Summary

> A global electronics retailer operates across 8 countries with 67 physical stores and 1 online channel. This dashboard was built to answer one strategic question: **"Where is growth coming from, and where should the business invest next?"**

| Metric | Value |
|--------|-------|
| Total Revenue (2016–2021) | $55.76M |
| Gross Margin | 58.58% |
| Total Orders | 26,326 |
| Avg Order Value | $2.12K |
| Countries | 8 + Online |
| Products | 2,517 across 8 categories |

---

## Dataset & Data Model

**Source:** SQL Server · Schema: `retails` · 4 tables

```
DIM_Date ──── FACT_Sales ──── DIM_Products
                  │
            DIM_Customers    DIM_Stores
```

| Table | Rows | Role |
|-------|------|------|
| FACT_Sales | 62,884 | Transactions: order, date, qty, keys |
| DIM_Products | 2,517 | Category, brand, price, cost |
| DIM_Customers | 15,266 | Geography, demographics, age group, gender |
| DIM_Stores | 67 | Country, state, size, open date |

**Key design decisions:**
- Revenue & Cost computed via SUMX at measure level — no stored columns
- DIM_Date generated via CALENDARAUTO(), marked as Date Table
- 28 DAX measures organized in `_Measures` table
- `Age Group` calculated column in DIM_Customers (Gen Z / Millennials / Gen X / Boomers / Silent)
- Field Parameters for dynamic dimension & metric switching across all pages

---

## Dashboard Structure — 4 Pages

| Page | Business Question |
|------|-----------------|
| Overview | Is the business growing? Which dimension is driving it? |
| Channel & Time | Which channel is growing? When does revenue peak? |
| Category & Product | Which category/SKU leads revenue and margin? |
| Customer & Geo | Where are customers? Which segment has highest value? |

---

## Page 1 — Overview

> **Business question:** "Is the business growing overall, and which dimension is driving it?"

### What this page shows
Filter zone at top (Year / Quarter / Channel Type / Category / Brand / Country + Reset). Two KPI groups: Revenue with %Growth PoP and %Growth YoY, then Orders + Gross Margin + AOV.

Below: monthly revenue bar chart on the left. On the right, a dynamic breakdown panel with 5 tabs (Channel / Category / Brand / Country / Continent) powered by a Field Parameter — clicking a tab switches both the treemap and the ranking table simultaneously. A grouped bar chart shows %Revenue YoY vs %Revenue PoP by the selected dimension.

### Key Insights

**Insight 1 — Growth peaked in 2019, collapsed in 2020**

| Year | Revenue | YoY |
|------|---------|-----|
| 2016 | $6.9M | — |
| 2017 | $7.4M | +6.8% |
| 2018 | $12.8M | **+72.3%** |
| 2019 | $18.3M | +42.8% |
| 2020 | $9.3M | **-49.1%** |

The business grew 2.6× from 2016 to 2019. Gross margin held stable at ~58% throughout — the 2020 drop was an external (COVID) shock, not a structural problem.

**Insight 2 — Adventure Works leads revenue; Contoso leads volume**

| Brand | Revenue | Orders | Implied AOV |
|-------|---------|--------|------------|
| Adventure Works | $11.8M | 5,681 | $2,086 |
| Contoso | $10.8M | 12,155 | $889 |
| Wide World Importers | $9.2M | 7,538 | $1,217 |

Two comparable revenue brands, two entirely different strategies — premium positioning (Adventure Works) vs. high-frequency promotions (Contoso).

---

## Page 2 — Channel & Time

> **Business question:** "Which channel is growing? When during the year does revenue peak?"

### What this page shows
Stacked bar chart: monthly revenue split by Online vs. Physical over the full 5-year period. On the right, a donut chart shows channel share, switchable between Revenue / Orders / Margin% / AOV via a Metric Parameter tab. Below: Top 10 Products table with cost, price, orders, revenue, margin, YoY%, country, state, subcategory.

### Key Insights

**Insight 3 — Online channel: 1 point, 20.45% of total revenue**

| Channel | Revenue | Share | Rev / "Store" |
|---------|---------|-------|--------------|
| Physical | $44.35M | 79.55% | $672K avg |
| **Online** | **$11.40M** | **20.45%** | **$11.40M** |

The single online channel outperforms the average physical store by 17×. The top-selling products (WWI Desktop X2330, Adventure Works XD233) are almost exclusively sold Online — confirming the channel is the primary revenue driver for high-ticket items.

**Insight 4 — Desktop PCs dominate the Top 10**
WWI Desktop PC2.33 X2330 Black ($153K) and Adventure Works XD233 series ($466K–$505K combined) account for the majority of top-product revenue, all through Online / Desktops subcategory.

---

## Page 3 — Category & Product

> **Business question:** "Which category brings the most revenue? Which brings the best margin? Which SKU leads within each category?"

### What this page shows
Treemap (category revenue hierarchy) on the left + subcategory bar chart on the right, both switchable between Revenue / Orders / Margin% / AOV via Metric Parameter tabs. A scatter chart below plots Gross Margin% (Y) vs Total Revenue (X) per category — immediately showing the margin-revenue quadrant. Top 10 Products table with full detail.

### Key Insights

**Insight 5 — Category scatter: Computers win revenue, Music wins margin**

| Category | Revenue | Share | Gross Margin | AOV |
|----------|---------|-------|-------------|-----|
| Computers | $19.3M | 34.6% | 58.4% | $1,756 |
| Home Appliances | $10.8M | 19.4% | 58.3% | **$2,078** |
| Cameras | $6.5M | 11.7% | **60.1%** | $1,305 |
| Music/Movies | $3.1M | 5.6% | **61.0%** | $402 |
| **Games & Toys** | **$0.7M** | **1.3%** | **54.7%** | **$118** |

The scatter chart makes this immediately visual: Music/Movies sits at the top-left (high margin, low revenue) — promotion opportunity. Games & Toys sits bottom-left (low margin, low revenue) — rationalization candidate. Home Appliances has the highest AOV ($2,078) — best upsell potential per transaction.

**Insight 6 — Desktops and Televisions lead subcategories**
Desktops ($9.9M), Televisions ($4.3M), Projectors & Screens ($3.8M), Water Heaters ($3.5M) are the top subcategories — a mix of computing and home appliance segments.

---

## Page 4 — Customer & Geography

> **Business question:** "Where are customers? Which segment and age group generates the most value?"

### What this page shows
Bubble map: revenue by store country (bubble size = revenue). On the right, a metric tab (Revenue / Orders / Margin% / AOV) switches the bar chart between "Revenue by state" and "AOV by state". A second tab (Age Group / Gender) switches the treemap between customer segmentation views. Below: Top 10 Customers table with name, age group, gender, revenue, margin, YoY%, state, country, continent, subcategory.

### Key Insights

**Insight 7 — Silent (70+) and Millennials lead revenue by age group**

| Age Group | Revenue |
|-----------|---------|
| Silent (70+) | $18.11M |
| Millennials (25–39) | $12.86M |
| Gen X (40–54) | $12.39M |
| Boomers (55–69) | $12.32M |

The Silent generation (70+) generates the highest revenue — likely reflecting higher disposable income and preference for premium electronics. Marketing and product mix should reflect this demographic weight.

**Insight 8 — Gender split is near-even: Male 50.09%**
No significant gender skew in purchasing. Campaigns can be broadly targeted without significant segmentation by gender.

**Insight 9 — California leads US states ($3.1M), but UK states have highest AOV**
North Somerset (UK) $12.4K AOV, Spelthorne (UK) $11.0K — European customers buy fewer but higher-value items vs US customers who buy more frequently at lower values.

**Insight 10 — Top customer concentration is low**
Top customer Karen Jones ($41.6K) represents <0.1% of total revenue — healthy sign of diversified customer base, no single-customer dependency risk.

---

## Strategic Recommendations

Three priority actions supported by data:

### 1. Scale the Online Channel
Online generates $11.4M from a single channel point — 17× the average physical store. Invest in digital capabilities, online-exclusive assortment, and last-mile logistics.

### 2. Target the Silent (70+) Segment
This age group drives the most revenue ($18.1M, ~32% of total). Product curation, UX for older users, and loyalty programs for this segment would have outsized impact.

### 3. Rationalize Games & Toys; Promote Music/Movies
Games & Toys: lowest revenue + lowest margin + lowest AOV = weakest category. Music/Movies: highest margin (61%) with room to grow — targeted promotions or bundling with Computers could shift mix profitably.

---

## Technical Skills Demonstrated

| Skill | Implementation |
|-------|---------------|
| Star Schema Design | FACT_Sales + 4 DIM tables, surrogate keys, hidden FKs |
| DAX — Base Measures | SUMX, DISTINCTCOUNT, SUM with RELATED |
| DAX — Time Intelligence | SAMEPERIODLASTYEAR, DATEADD, DATESYTD, DATESMTD, DATESINPERIOD |
| DAX — Ranking | RANKX + ALL(), Rank PoP with CALCULATE+DATEADD, SWITCH icon |
| DAX — Context | CALCULATE, ALLEXCEPT, DIVIDE with zero-safe denominator |
| DAX — Calculated Column | Age Group segmentation from birthday using DATEDIFF + SWITCH |
| Field Parameters | Dynamic dimension switching (5 tabs) + metric switching (4 tabs) |
| Dashboard Design | top→bottom hierarchy: filter → KPI → overview → detail |
| Data Quality Analysis | 79.1% null delivery dates flagged as operational gap |
| SQL + ODBC | Schema discovery, multi-join aggregation via SQL Server |
| Business Storytelling | Revenue → Channel → Category → Customer narrative arc |

---

## Files in This Portfolio

| File | Description |
|------|-------------|
| `Retails_Dashboard.pbix` | Power BI Desktop file — full interactive dashboard |
| `Retails_Dashboard.md` | Technical documentation: dataset, model, DAX pack |
| `Retails_Dashboard_Portfolio.md` | This file — business case study |

---

*Power BI Desktop · DAX · SQL Server · Star Schema · Field Parameters · 2016–2021 Global Electronics Retail*
