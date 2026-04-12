---
layout: single
title: "Creating an Olist e-commerce dashboard in Power BI"
excerpt: ""
header:
#   teaser: /assets/images/marvel_teaser.jpg
  overlay_color: "#dde3ea"
classes: wide project-page

---
<div class="project-content" markdown="1">

# Olist E-Commerce Dashboard — Power BI

A three-page interactive dashboard built on the [Olist Brazilian E-Commerce dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), covering ~100,000 orders placed between 2016 and 2018.

---

## Data Model

The report is built on a **star schema** with two fact tables and four dimension tables.

- **fact_orders** — one row per order (status, timestamps, review score)
- **fact_order_items** — one row per item (price, freight, seller, product)
- **dim_customers**, **dim_sellers**, **dim_products**, **dim_date**

The two fact tables share `order_id` but are not directly related — filters flow between them via DAX using `TREATAS` rather than a physical relationship, avoiding many-to-many ambiguity.

![Data Model](/assets/images/star_model_olist.png)
---

## Page 1 — Executive Overview

*"How is the business performing overall?"*

![Page 1 — Executive Overview](/assets/images/page1_olist.png)

### Visuals
- KPI cards: Total Revenue, Total Orders, Avg Order Value, Avg Review Score
- Monthly revenue trend (line chart)
- Orders by customer state (bar chart)
- Order status breakdown (donut chart)
- Late delivery rate and avg delivery days (KPI cards)
- Revenue by product category — top 10 (bar chart)

### Key measures

```dax
Total Revenue = 
SUMX(fact_order_items, fact_order_items[price] + fact_order_items[freight_value])

Late Delivery Rate = 
DIVIDE(
    CALCULATE([Total Orders], 
        fact_orders[order_delivered_customer_date] > fact_orders[order_estimated_delivery_date]),
    [Total Orders]
)

Avg Delivery Days = 
AVERAGEX(
    FILTER(fact_orders, fact_orders[order_status] = "delivered"),
    DATEDIFF(fact_orders[order_purchase_timestamp], fact_orders[order_delivered_customer_date], DAY)
)
```

---

## Page 2 — Customer Satisfaction

*"Does delivery performance drive review scores?"*

![Page 2 — Customer Satisfaction](/assets/images/page2_olist.png)

### Visuals
- KPI cards: Avg Review Score, Late Delivery Rate, Avg Days Late, Avg Delivery Days
- Delivery vs satisfaction scatter plot — states as bubbles, size = order volume
- Review score distribution (bar chart)
- Avg review score over time (line chart with baseline reference line)

### Key measures

```dax
Avg Days Late = 
AVERAGEX(
    FILTER(fact_orders, 
        fact_orders[order_delivered_customer_date] > fact_orders[order_estimated_delivery_date]),
    DATEDIFF(fact_orders[order_estimated_delivery_date], fact_orders[order_delivered_customer_date], DAY)
)

-- Distribution measure — works with review_scores disconnected table on axis
%Star_Review = 
DIVIDE(
    [Total Orders],
    CALCULATE([Total Orders], ALL(fact_orders[review_score]))
)
```

The scatter plot uses `dim_customers[customer_state]` as the detail field, revealing that states with longer average delivery times consistently show lower satisfaction scores.

---

## Page 3 — Seller Performance

*"Are sellers with high scores reliable?"*

![Page 3 — Seller Performance](/assets/images/page3_olist.png)

### Visuals
- KPI cards: Total Sellers, Revenue per Seller, Avg Review per Seller, % At Risk Sellers
- Delivery vs satisfaction scatter plot — sellers as bubbles, quadrant lines at global averages
- Top 20 sellers table with conditional formatting on review score and delivery days
- Revenue by seller state (bar chart)
- Breakdown of review scores by top 20 sellers (Stacked bar chart) 

### Key measures

```dax
Avg Review per Seller = 
AVERAGEX(
    VALUES(fact_order_items[seller_id]),
    CALCULATE(
        AVERAGE(fact_orders[review_score]),
        TREATAS(VALUES(fact_order_items[order_id]), fact_orders[order_id])
    )
)

Avg Delivery Days per Seller = 
CALCULATE(
    AVERAGEX(
        fact_orders,
        DATEDIFF(fact_orders[order_purchase_timestamp], fact_orders[order_delivered_customer_date], DAY)
    ),
    TREATAS(VALUES(fact_order_items[order_id]), fact_orders[order_id])
)

% At Risk Sellers = 
DIVIDE(
    COUNTROWS(
        FILTER(
            ALL(dim_sellers),
            [Avg Review per Seller] < [Avg Review Score] &&
            [Avg Delivery Days per Seller] > [Avg Delivery Days]
        )
    ),
    [Total Sellers]
)
```

### Key finding

The scatter plot reveals that several top-revenue sellers fall below the average review score threshold — representing significant revenue at risk from poor customer experience.

> *"Some of the highest-revenue sellers have below-average satisfaction scores — a compounding business risk."*

---

## Technical Notes

- **Date table** generated in DAX via `CALENDAR()`, marked as official date table
- **TREATAS** used throughout to bridge `fact_orders` and `fact_order_items` without a direct relationship
<!-- - **Disconnected table** (`review_scores`) used to drive the score distribution visual without filter collision -->
- Custom JSON theme applied for consistent visual identity across all pages
- All measures stored in a dedicated `_Measures` table

---

*Dataset: Olist Brazilian E-Commerce Public Dataset — Kaggle*
