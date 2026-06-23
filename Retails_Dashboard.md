# Global Retail Electronics — Power BI Dashboard

> **Portfolio project** | Power BI | DAX | Data Modeling | Business Storytelling  
> Dataset: Global Electronics Retailer (2016–2021) · 5 tables · 62,884 transactions

---

## Overview

This project demonstrates an end-to-end BI workflow applied to a global electronics retailer operating across 8 countries with 67 physical stores and an online channel.

The dashboard answers the strategic question:  
**"Where is the business growing, what drives it, and what should leadership prioritize next?"**

---

## Dataset

| Table | Rows | Description |
|-------|------|-------------|
| `sales` | 62,884 | Fact table — order number, date, qty, FK keys |
| `products` | 2,517 | Products with category, brand, unit cost & price |
| `customers` | 15,266 | Demographics, geography, continent |
| `stores` | 67 | Country, state, store size (sqm), open date |
| `DIM_Date` | Generated | Full date dimension with time intelligence |

**Source:** SQL Server · Schema: `retails` · Period: Jan 2016 – Feb 2021

---

## Data Model — Star Schema

```
                    DIM_Date
                  (generated)
                      │ order_date
                      │
DIM_Customers ────── FACT_Sales ────── DIM_Products
(customer_key)    (order_number         (product_key)
                   line_item
                   quantity)
                      │
                 DIM_Stores
                 (store_key)
```

**Design decisions:**
- Revenue and Cost computed via SUMX at measure level (not stored as columns) — avoids redundancy
- DIM_Date generated via `CALENDARAUTO()` and marked as Date Table
- Surrogate keys used for all relationships; descriptive FK columns hidden from Report View
- `_Measures` table created as an empty reference table — all 28 DAX measures live here

---

## Business Questions & Dashboard Pages

### Page 1 — Executive Overview
**"Is the business growing? Who is driving it?"**

Answers:
- Total Revenue, Orders, Gross Margin, Customers — selected period vs PoP vs YoY
- Revenue trend by month (bar chart, current vs prior year overlay)
- Top N breakdown switchable by dimension (Category / Brand / Country) via Field Parameter tab

---

### Page 2 — Channel & Time Analysis
**"Which channel and time period are driving revenue?"**

Answers:
- Online vs Physical split — Online channel = 1 store, $11.4M revenue (20.5% of total)
- Seasonality pattern — month-over-month fluctuation
- Growth % comparison: YoY vs PoP side-by-side by month

---

### Page 3 — Category & SKU Performance
**"Which category brings revenue? Which brings margin? Which product leads within each category?"**

Answers:
- Revenue share by category (donut) + margin heatmap
- Top 10 products ranked — with rank change PoP (▲▼)
- Drill-down: Category → Subcategory → Product (matrix)
- AOV by category — identifies high-ticket vs high-volume segments

---

### Page 4 — Customer & Geography
**"Who buys? Which market is most valuable? Where should we expand?"**

Answers:
- Revenue by continent and country
- Revenue per customer by market — identifies customer quality, not just quantity
- Customer count vs revenue contribution — find high-value segments
- Store efficiency: Revenue per store by country — flags underperforming markets

---

### Page 5 — Store Operations
**"Is our store network healthy? Online vs physical — which model wins?"**

Answers:
- Revenue per store by country — Online at $11.4M/store vs France $176K/store
- Store size (sqm) vs Revenue scatter — identifies whether bigger stores sell more
- Delivery performance: avg days + % of orders with missing delivery tracking
- Store open date vs revenue maturity — older stores ≠ better performance

---

## Key Insights Discovered

### 1. Growth Story: Boom → Peak → COVID Collapse

| Year | Revenue | YoY | Orders | Gross Margin |
|------|---------|-----|--------|-------------|
| 2016 | $6.9M | — | 2,865 | 59.1% |
| 2017 | $7.4M | +6.8% | 3,280 | 58.4% |
| 2018 | $12.8M | **+72.3%** | 5,965 | 58.4% |
| 2019 | $18.3M | +42.8% | 9,083 | 58.6% |
| 2020 | $9.3M | **-49.1%** | 4,635 | 58.6% |
| 2021 | $1.0M | -88.8%* | 498 | 58.5% |

*2021 = Jan–Feb only (partial year)

**Insight:** Gross margin held remarkably stable at 58–59% across all years — cost structure is disciplined. Revenue collapse in 2020 was demand-driven (COVID), not a margin or pricing problem.

---

### 2. Category: Computers Lead Revenue, But Cameras & Music Lead Margin

| Category | Revenue | Share | Gross Margin | AOV |
|----------|---------|-------|-------------|-----|
| Computers | $19.3M | 34.6% | 58.4% | $1,756 |
| Home Appliances | $10.8M | 19.4% | 58.3% | **$2,078** |
| Cameras | $6.5M | 11.7% | **60.1%** | $1,305 |
| Cell phones | $6.2M | 11.1% | 56.6% | $733 |
| TV & Video | $5.9M | 10.6% | 59.7% | $1,783 |
| Audio | $3.2M | 5.7% | 57.7% | $478 |
| Music/Movies/Books | $3.1M | 5.6% | **61.0%** | $402 |
| Games & Toys | $0.7M | 1.3% | 54.7% | $118 |

**Insight:** Music/Movies has the highest margin (61%) but lowest AOV ($402) — high frequency, low ticket. Home Appliances has the highest AOV ($2,078) — best upsell potential. Games & Toys is the weakest: lowest revenue, lowest margin, lowest AOV — candidate for rationalization or bundle strategy.

---

### 3. Online Channel: The Hidden Champion

| Market | Revenue | Stores | Revenue / Store |
|--------|---------|--------|----------------|
| United States | $23.8M | 20 | $1.19M |
| **Online** | **$11.4M** | **1** | **$11.4M** |
| United Kingdom | $5.7M | 7 | $0.82M |
| Germany | $4.2M | 8 | $0.53M |
| Canada | $3.6M | 3 | $1.20M |
| Australia | $2.1M | 5 | $0.42M |
| Italy | $2.1M | 3 | $0.69M |
| Netherlands | $1.6M | 4 | $0.40M |
| **France** | **$1.2M** | **7** | **$0.18M** |

**Insight:** Online generates **9.6× the revenue per "store"** compared to the US average, and **64× more** than France. France has 7 stores but performs worse than the Netherlands (4 stores). France and Australia are over-stored relative to revenue output — capital reallocation opportunity.

---

### 4. Brand Analysis

| Brand | Revenue | Orders | Implied AOV |
|-------|---------|--------|------------|
| Adventure Works | $11.8M | 5,681 | $2,086 |
| Contoso | $10.8M | 12,155 | $889 |
| Wide World Importers | $9.2M | 7,538 | $1,217 |
| Fabrikam | $6.8M | 3,330 | $2,044 |
| The Phone Company | $5.4M | 5,424 | $993 |
| Proseware | $3.2M | 2,840 | $1,131 |

**Insight:** Adventure Works and Fabrikam drive high AOV — premium positioning. Contoso has 2× the orders of Adventure Works but generates similar revenue — high-volume, low-ticket brand. Different promotional strategies apply.

---

### 5. Data Quality Flag — Delivery Tracking Gap

- **79.1% of orders** (49,719 / 62,884) have NULL delivery date
- Available delivery data shows avg **4.5 days** delivery time
- This limits fulfillment analysis significantly — recommendation: enforce delivery date capture at order completion

---

## Dashboard Design Principles Applied

**Layout follows Design Thinking — top → bottom, overview → details:**

1. **Filter zone (top):** User sets period and dimension context before seeing numbers
2. **KPI zone:** Selected period value + %Growth PoP + %Growth YoY — always in triplets
3. **Overview zone:** Breakdown visuals with Field Parameter tab switcher (dynamic dimension)
4. **Trend zone:** Monthly bar chart + YoY vs PoP grouped bar side-by-side
5. **Detail zone (last):** Table with drill-through, full raw data, export

**Key techniques:**
- **Field Parameters** — single visual switchable between Category / Brand / Country / Continent via tab buttons
- **Rank PoP** — rank change indicators (▲▼) in Top N tables show momentum, not just position
- **Selected Period Label** — KPI subtitle always shows current filter context explicitly
- **Edit Interactions** — cross-filter disabled on KPI cards to prevent number distortion

---

## Full DAX Pack (28 Measures)

### Base
```dax
Total Revenue    = SUMX(FACT_Sales, FACT_Sales[quantity] * RELATED(DIM_Products[unit_price_usd]))
Total Cost       = SUMX(FACT_Sales, FACT_Sales[quantity] * RELATED(DIM_Products[unit_cost_usd]))
Total Profit     = [Total Revenue] - [Total Cost]
Total Orders     = DISTINCTCOUNT(FACT_Sales[order_number])
Total Quantity   = SUM(FACT_Sales[quantity])
Total Customers  = DISTINCTCOUNT(FACT_Sales[customer_key])
```

### Derived KPIs
```dax
Gross Margin %       = DIVIDE([Total Profit], [Total Revenue], 0)
Avg Order Value      = DIVIDE([Total Revenue], [Total Orders], 0)
Revenue per Customer = DIVIDE([Total Revenue], [Total Customers], 0)
Revenue per Store    = DIVIDE([Total Revenue], DISTINCTCOUNT(FACT_Sales[store_key]), 0)
```

### Time Intelligence
```dax
Revenue LY         = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(DIM_Date[Date]))
Revenue YoY%       = DIVIDE([Total Revenue] - [Revenue LY], [Revenue LY], BLANK())
Revenue PP         = CALCULATE([Total Revenue], DATEADD(DIM_Date[Date], -1, MONTH))
Revenue PoP%       = DIVIDE([Total Revenue] - [Revenue PP], [Revenue PP], BLANK())
Revenue MTD        = CALCULATE([Total Revenue], DATESMTD(DIM_Date[Date]))
Revenue YTD        = CALCULATE([Total Revenue], DATESYTD(DIM_Date[Date]))
Revenue Rolling3M  = CALCULATE([Total Revenue], DATESINPERIOD(DIM_Date[Date], LASTDATE(DIM_Date[Date]), -3, MONTH))

Selected Period =
    VAR _min = FORMAT(MIN(DIM_Date[Date]), "MMM YYYY")
    VAR _max = FORMAT(MAX(DIM_Date[Date]), "MMM YYYY")
    RETURN IF(_min = _max, _min, _min & " – " & _max)
```

### Ranking + Rank PoP
```dax
// Category
Category Rank        = RANKX(ALL(DIM_Products[category]), [Total Revenue], , DESC, DENSE)
Category Rank PP     = CALCULATE(RANKX(ALL(DIM_Products[category]), [Total Revenue], , DESC, DENSE), DATEADD(DIM_Date[Date], -1, MONTH))
Category Rank Icon   = VAR _d = [Category Rank PP] - [Category Rank] RETURN SWITCH(TRUE(), _d > 0, "▲ " & _d, _d < 0, "▼ " & ABS(_d), "─")

// Product
Product Rank         = RANKX(ALL(DIM_Products[product_name]), [Total Revenue], , DESC, DENSE)
Product Rank PP      = CALCULATE(RANKX(ALL(DIM_Products[product_name]), [Total Revenue], , DESC, DENSE), DATEADD(DIM_Date[Date], -1, MONTH))
Product Rank Icon    = VAR _d = [Product Rank PP] - [Product Rank] RETURN SWITCH(TRUE(), _d > 0, "▲ " & _d, _d < 0, "▼ " & ABS(_d), "─")

// Country
Country Rank         = RANKX(ALL(DIM_Stores[country]), [Total Revenue], , DESC, DENSE)
Country Rank PP      = CALCULATE(RANKX(ALL(DIM_Stores[country]), [Total Revenue], , DESC, DENSE), DATEADD(DIM_Date[Date], -1, MONTH))
Country Rank Icon    = VAR _d = [Country Rank PP] - [Country Rank] RETURN SWITCH(TRUE(), _d > 0, "▲ " & _d, _d < 0, "▼ " & ABS(_d), "─")
```

### % Share & Operations
```dax
Revenue % of Total    = DIVIDE([Total Revenue], CALCULATE([Total Revenue], ALL(DIM_Products)), 0)
Revenue % of Category = DIVIDE([Total Revenue], CALCULATE([Total Revenue], ALLEXCEPT(DIM_Products, DIM_Products[category])), 0)
Online Revenue        = CALCULATE([Total Revenue], DIM_Stores[country] = "Online")
Online Share %        = DIVIDE([Online Revenue], [Total Revenue], 0)
Avg Delivery Days     = AVERAGEX(FILTER(FACT_Sales, NOT ISBLANK(FACT_Sales[delivery_date])), DATEDIFF(FACT_Sales[order_date], FACT_Sales[delivery_date], DAY))
```

---

## Field Parameters Setup

**Dim Selector** (for dynamic Top N table + donut chart):
```
Modeling → New Parameter → Fields
Fields: category, brand, country, continent
Format axis slicer as: Tile / Buttons (single-select)
```

**Metric Selector** (for dynamic trend chart):
```
Modeling → New Parameter → Fields  
Measures: [Total Revenue], [Total Orders], [Gross Margin %], [Avg Order Value]
```

---

## Strategic Recommendations

Based on the analysis, three priority actions emerge:

**1. Scale Online Channel**
Online generates $11.4M from a single channel point — 9.6× the efficiency of the average US physical store. Invest in digital capabilities, online-exclusive assortment, and last-mile logistics.

**2. Rationalize France's Store Network**
France has 7 stores but delivers only $176K revenue per store — the worst in the network. Germany (8 stores, $531K/store) and Italy (3 stores, $686K/store) are more efficient with similar or smaller footprints.

**3. Prioritize Cameras + TV & Video for Margin Expansion**
These two categories combine high gross margin (59–60%) with meaningful revenue scale. Home Appliances offers the highest AOV ($2,078) — highest upsell potential per transaction.

---

## Technical Skills Demonstrated

| Area | Detail |
|------|--------|
| Data Modeling | Star schema design, DIM/FACT separation, surrogate keys, date table |
| DAX | 28 measures: base, KPI, time intelligence, ranking, % share, operations |
| Advanced DAX | RANKX + Rank PoP, SUMX, CALCULATE, DATEADD, VAR patterns |
| Power BI Features | Field Parameters, tab switcher, drill-through, Edit Interactions, conditional formatting |
| Dashboard Design | Design thinking: top→bottom, overview→detail; slicer-first layout |
| Business Analysis | Revenue attribution, margin analysis, store efficiency, channel comparison |
| Data Quality | Identified 79.1% null delivery dates — flagged as actionable data gap |
| SQL | Schema discovery, multi-table joins, aggregations via ODBC connection |

---

## How to Replicate

1. Connect Power BI Desktop to SQL Server: `45.124.94.158`, database `xomdata_dataset`
2. Load 4 tables from schema `retails`: `sales`, `products`, `customers`, `stores`
3. Generate `DIM_Date` using `CALENDARAUTO()` in DAX, mark as Date Table
4. Set relationships as described in Data Model section
5. Create `_Measures` empty table and paste all 28 DAX measures
6. Create 2 Field Parameters: Dim Selector and Metric Selector
7. Build 5 pages following the design principle: filter → KPI → overview → trend → detail

---

*Analysis period: 2016–2021 | Tool: Microsoft Power BI Desktop | DAX Engine: Tabular Model*
