# Customer Segmentation using RFM Analysis
### E-Commerce Transactional Data | Python · Power BI · Pandas · Seaborn

---

## Overview

Most e-commerce businesses are sitting on a goldmine of transactional data and doing almost nothing with it. This project cuts through that, using the RFM (Recency, Frequency, Monetary) framework to turn raw purchase records into a clear, actionable customer hierarchy. The end goal: know exactly who your best customers are, who's slipping away, and what to do about each group.

The analysis covers ~540K transactions from a UK-based online retailer, segments customers into four behaviorally distinct groups, and delivers a Power BI dashboard alongside concrete marketing recommendations for each segment.

---

## Problem Statement

E-commerce companies generate enormous amounts of transactional data but rarely leverage it beyond basic reporting. Without a structured segmentation approach, marketing budgets get spread thin across all customers equally, which means your best customers aren't being rewarded and your churning customers aren't being caught in time.

This project addresses three core questions:
- Who are the customers contributing the most to revenue?
- Which customers are at risk of leaving, and how do we re-engage them?
- How should marketing budgets be allocated across customer groups to maximize ROI?

---

## Dataset

**Source:** [UCI Machine Learning Repository / Kaggle — Online Retail Dataset](https://www.kaggle.com/datasets/carrie1/ecommerce-data)

| Field | Description |
|---|---|
| `InvoiceNo` | Unique transaction identifier |
| `StockCode` | Unique product code |
| `Description` | Product name/description |
| `Quantity` | Units purchased per line item |
| `InvoiceDate` | Timestamp of transaction |
| `UnitPrice` | Price per unit (GBP £) |
| `CustomerID` | Unique customer identifier |
| `Country` | Customer's country |

**Size:** 541,909 rows × 8 columns  
**Timeframe:** December 2010 – December 2011  
**Geography:** Primarily United Kingdom, with international orders

---

## Project Structure

```
rfm-customer-segmentation/
│
├── data/
│   └── online_retail.csv            # Raw dataset (download from Kaggle)
│
├── notebooks/
│   └── Data_Cleaning_and_EDA.ipynb  # Full cleaning + EDA pipeline
│
├── dashboard/
│   └── RFM_Dashboard.pbix           # Power BI dashboard file
│
├── presentation/
│   └── Customer_Segmentation.pdf    # Final stakeholder presentation
│
└── README.md
```

---

## Methodology

### Step 1 — Data Cleaning

The raw dataset had several quality issues that needed to be resolved before any analysis:

**Missing Values**
CustomerID was null for roughly 135,000 rows (~24% of the data). Since the entire analysis revolves around individual customer behavior, rows without a CustomerID were dropped. This also resolved the 1,454 null Description values, which were tied to the same anonymous records.

**Duplicates**
5,225 exact duplicate rows were identified and removed using `df.drop_duplicates()`.

**Cancelled Orders**
8,872 rows had negative Quantity values. Cross-checking InvoiceNo confirmed all of them started with "C", the retailer's convention for cancellations. These were excluded since they don't represent actual completed purchases.

**Non-Product StockCodes**
Several StockCodes didn't correspond to real products, things like `POST` (postage), `C2` (carriage), `M` (manual adjustments), `DOT` (dotcom postage), and `BANK CHARGES`. These were filtered out to ensure the analysis reflected only genuine product transactions.

**Post-cleaning size:** ~392,000 rows retained for analysis.

---

### Step 2 — Exploratory Data Analysis

**Daily Sales**
Sales are heavily concentrated on a handful of peak days. The distribution is right-skewed, a few exceptional days drive a disproportionate share of annual revenue. The single highest day (December 9, 2011) recorded £184,170 in sales.

**Monthly Sales**
Average monthly revenue across the period was £672K, but most months came in below that average. There's a sharp ramp-up starting in September, peaking in November, and then a notable drop in December — consistent with Christmas ordering patterns where customers buy in November for December delivery.

**Pareto Analysis**
- Just **26% of customers** drive 80% of total revenue
- Just **21% of products** account for 80% of sales volume

These findings reinforce why segment-specific strategies matter, generic campaigns would be massively inefficient.

---

### Step 3 — RFM Scoring

RFM is a behavioral segmentation framework that scores customers across three dimensions:

| Metric | Definition |
|---|---|
| **Recency (R)** | Days since the customer's last purchase |
| **Frequency (F)** | Total number of purchases made |
| **Monetary (M)** | Total amount spent (£) |

Each customer was scored 1–5 on each dimension using equal-frequency binning (quintiles), then scores were combined to produce an overall RFM profile. The M-Score heatmap (plotted across R-Score vs F-Score) clearly revealed four behavioral clusters.

---

### Step 4 — Customer Segmentation

| Segment | Share of Customers | RFM Profile |
|---|---|---|
| At-Risk | 65.4% | Moderate recency, low frequency, variable spend |
| Loyal | 16.0% | Recent purchases, moderately high frequency, high spend |
| Dormant | 8.4% | Hasn't purchased in a long time, low frequency, low spend |
| High Value | 10.2% | Most recent, highest frequency, highest spend |

---

## Recommendations

### High Value Customers (10.2%)
These are the people who shop often, spend the most, and came back recently. They already love the brand, the job now is to make sure they feel it. A tiered loyalty program works well here: early access to new products, exclusive member pricing, or a simple "thank you" campaign with a personalised discount goes a long way. Losing one of these customers is expensive, so retention spend here is absolutely justified.

### Loyal Customers (16%)
Solid, reliable buyers who haven't quite hit the top tier yet. The opportunity is upselling and cross-selling, they're already comfortable purchasing, so introducing complementary products or bundle offers during checkout has a reasonable conversion probability. These customers are also strong candidates for referral programs since they tend to have positive brand perception.

### At-Risk Customers (65.4%)
The largest group and the one that needs the most attention. These customers have purchased before but their engagement has dropped off, maybe they found an alternative, maybe the last experience wasn't great, or maybe they just forgot. A time-limited promotion (something like "we miss you — here's 15% off for the next 7 days") tends to work better than a generic newsletter. Personalisation is key: if possible, email them about products in categories they've bought before rather than a blanket offer.

### Dormant Customers (8.4%)
These customers have essentially stopped engaging. A win-back campaign is worth trying, but expectations should be calibrated, the conversion rate will be low. A short customer satisfaction survey can at least surface whether there was a specific reason they left (pricing, product quality, bad experience). If they don't respond to one or two re-engagement attempts, it's reasonable to deprioritise them in the marketing budget rather than continuing to spend on unresponsive contacts.

---

## Dashboard

An interactive Power BI dashboard was built covering:
- Total revenue, active customers, and sales volume (KPIs)
- Revenue contribution by customer segment over time
- Customer distribution by segment (pie chart)
- Revenue by day of week
- Active customers trend over time
- RFM heatmap grid
- Top/Bottom customer table with sales and volume

Filters available: Country, Month, Quarter, Year

---

## Tools & Technologies

- **Python**: Data cleaning, EDA, RFM calculation (`pandas`, `numpy`, `matplotlib`, `seaborn`)
- **Jupyter Notebook**: Analysis environment
- **Power BI**: Interactive dashboard
- **Kaggle**: Dataset source

---

## Key Takeaways

1. A small fraction of customers (26%) are responsible for the bulk of revenue, protecting them is the highest-priority business objective.
2. The At-Risk segment (65% of customers) represents the largest re-engagement opportunity, but requires personalised, time-sensitive outreach rather than generic campaigns.
3. Revenue is strongly seasonal, Q4 campaigns (especially October–November) will consistently outperform the same spend at other times of year.
4. RFM segmentation gives the marketing team a concrete, data-backed framework to prioritise budget allocation instead of treating all customers the same.

---

## Author

**Abhijeeth S**  
Data Analyst  

Feel free to reach out if you'd like to discuss the methodology, the dashboard, or how this approach could be applied to other domains.

---

*Dataset sourced from Kaggle. All analysis performed on publicly available data for portfolio purposes.*
