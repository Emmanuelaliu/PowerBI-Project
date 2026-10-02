# PowerBI-Project
A full-stack data cleaning, analysis and visualizing  project using Microsoft PowerBI

# Customer Satisfaction Analysis for JET Electros

KPI analysis of a promotional sale for **JET Electros**, an e-commerce electronics company. The project turns 10,999 customer orders into a single-page Power BI dashboard, a slide deck and a set of business recommendations.

<a href ="https://drive.google.com/file/d/1IllBGr-L9MRuTwui1HFf5wU1WxH4eWA6/view?usp=drive_link" target="_blank" rel="noopener noreferrer">Dashboard Overview</a>

---

## Table of contents

1. [Project overview](#project-overview)
2. [Business problem and objectives](#business-problem-and-objectives)
3. [Dataset](#dataset)
4. [Tools used](#tools-used)
5. [Data preparation](#data-preparation)
6. [DAX measures](#dax-measures)
7. [The dashboard](#the-dashboard)
8. [Key insights](#key-insights)
9. [Recommendations](#recommendations)
10. [Assumptions and limitations](#assumptions-and-limitations)
11. [Project structure](#project-structure)
12. [How to open the project](#how-to-open-the-project)
13. [Author](#author)

---

## Project overview

After its promotional sale, JET Electros wanted to know how the sale performed and how well its operations coped. As the data analyst, I reviewed the customer order database across six KPIs:

- Customer demography
- Customer satisfaction
- Importance of products
- Preferred mode of shipment
- Promptness of shipment completion
- Warehouse blocks usage (the company has 5 blocks)

**Headline results**

| Metric | Value |
|---|---|
| Orders analysed | 10,999 |
| Average review rating | 2.99 out of 5 |
| Normal-day revenue (no discount) | $2.31M |
| Total discount given | about $147.09K |
| Overall discount | 6.36% |
| Revenue after discount | $2.16M |
| Orders delivered late | 59.7% (6,563 orders) |

## Business problem and objectives

Each KPI was turned into a business question:

| KPI | Business question |
|---|---|
| Customer demography | Which customer groups make up the customer base, and does gender separate customers? |
| Customer satisfaction | How satisfied are customers overall, and how many are worried enough to contact customer care? |
| Importance of products | Which product importance levels get the most orders during the sale? |
| Preferred mode of shipment | Which shipment mode is used most? |
| Promptness of shipment | What share of orders is delivered late, and what is linked to lateness? |
| Warehouse blocks usage | How are orders spread across the 5 warehouse blocks? |

## Dataset

- **File:** `PBI Project - Customer Satisfaction.xlsx` (one sheet, "Customer Analytics")
- **Size:** 10,999 rows (one row per order) and 12 columns, with no missing values or duplicate IDs

| Field | Description |
|---|---|
| ID | Order ID |
| Warehouse block | Block A to E |
| Mode of Shipment | Ship, Flight or Road |
| Customer care calls | Number of calls made about the order |
| Customer rating | 1 to 5 |
| Cost of the Product (USD) | Normal price of the product |
| Prior purchases | Number of earlier purchases |
| Product importance | Low, medium or high |
| Gender | Female or male |
| Discount offered (%) | Discount on the order |
| Weight | Product weight |
| Shipment completion promptness | Late delivery or early delivery |

## Tools used

| Category | Tool |
|---|---|
| Data cleaning and transformation | Power Query |
| Modelling and measures | Power BI Desktop, DAX |
| Visualisation | Power BI |
| Presentation | PowerPoint (exported to PDF) |
| Version control | Git and GitHub |

## Data preparation

Done in Power Query before loading into the model:

- Added `Discount offer dec` (discount as a decimal) and `Discount USD` (cost × discount)
- Converted weight to kilograms (`Weight in Kgs`)
- Labelled the promptness field as **Late delivery** or **Early delivery**
- Labelled ratings 1 to 5 (worst, bad, fair, good, perfect)
- Grouped customer care calls into **worried** and **not worried** customers
- Checked data types, missing values and duplicates

## DAX measures

Adjust the table and column names to match your model.

```DAX
Number of Orders = COUNTROWS ( 'Customer Analytics' )

Review Rating = AVERAGE ( 'Customer Analytics'[Customer rating] )

Normal Revenue = SUM ( 'Customer Analytics'[Cost of the Product USD] )

Total Discount =
SUMX (
    'Customer Analytics',
    'Customer Analytics'[Cost of the Product USD] * 'Customer Analytics'[Discount offer dec]
)

Total Revenue = [Normal Revenue] - [Total Discount]

Percentage Discount = DIVIDE ( [Total Discount], [Normal Revenue] ) * 100
```

## The dashboard

A single report page with a slicer, four KPI cards and seven visuals:

- **Cards:** Number of orders, Review rating, Percentage discount, Total Revenue
- **Order by Gender** (donut chart)
- **Order by Customer rating** (line chart)
- **Order by Customer care** (funnel)
- **Order by Product importance** (column chart)
- **Order by Shipment mode** (bar chart)
- **Order by Shipment promptness** (pie chart)
- **Order by Warehouse block** (bar chart)

The final slide deck is in [`presentation/`](presentation/).

## Key insights

**1. Customer demography**
Orders are split evenly: 5,545 from women (50.41%) and 5,454 from men (49.59%). Gender does not separate customers.

**2. Customer satisfaction**
- The average rating is **2.99 out of 5**, in the "fair" range. Ratings are spread almost evenly: 2,235 worst, 2,165 bad, 2,239 fair, 2,189 good and 2,171 perfect.
- 7,144 customers (64.95%) were worried and contacted customer care four or more times, against 3,855 who were not.
- Customers who gave a good or perfect rating produced **$860.10K** of the $2.16M revenue, about 40%.

**3. Importance of products**
Customers ordered mostly low-importance (5,297) and medium-importance (4,754) products. Only 948 orders were high-importance.

**4. Preferred mode of shipment**
Ship carries most orders (7,462), against 1,777 by Flight and 1,760 by Road.

**5. Promptness of shipment**
- **59.7% of orders were delivered late** (6,563) and 40.3% were early (4,436).
- Lateness depends on weight: 68% for under 2 kg, **100% for 2–4 kg**, 43% for 4–6 kg and 100% for 6 kg and above (only 8 orders in the heaviest band).
- Orders with a discount above 10% were also late about 100% of the time (2,196 orders).

**6. Warehouse blocks usage**
Block E handles 3,666 orders, twice the roughly 1,833 handled by each of blocks A to D. Its late-delivery rate is the same as the others, so it appears to have spare capacity.

## Recommendations

1. **Reach back to customers who gave good and perfect ratings** and try to keep them, because they produced about $860.10K of revenue.
2. **Rebalance orders from Block E** to the other blocks to reduce bottleneck risk.
3. **Keep Ship as the default shipment mode**, since faster modes add no measurable benefit, and work on delivery speed instead.
4. **Collect age and location data** and segment customers by prior purchases rather than gender.

## Assumptions and limitations

- The 6.36% discount assumes the product cost is the price **before** discount. If it is already discounted, the overall discount is about 7.4%.
- The data has no age or location, so demography is limited to gender.
- There is no pre-sale baseline, so the report cannot say whether the sale paid off.
- The findings show patterns in the data, not proven causes.

## Project structure

```
customer-satisfaction-powerbi/
├── dashboard/
│   └── PROJECT_KPI.pbix
├── data/
│   └── PBI Project - Customer Satisfaction.xlsx
├── presentation/
│   └── Presentation_slides.pdf
├── images/
│   └── dashboard-overview.png
└── README.md
```

## How to open the project

1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/customer-satisfaction-powerbi.git
   ```
2. Open `dashboard/PROJECT_KPI.pbix` in **Power BI Desktop**.
3. If Power BI cannot find the data, go to **Transform data → Data source settings** and point it to `data/PBI Project - Customer Satisfaction.xlsx`.

## Author

**[emmanuel-aliu]**
Data analyst
[LinkedIn](https://www.linkedin.com/in/aliu-godwin-emmanuel) | [Email](mailto:your-email@aliuemmenuel31@gmail.com)

