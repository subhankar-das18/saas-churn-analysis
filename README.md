# SaaS Churn Analysis Dashboard 

A comprehensive Power BI dashboard analyzing subscription-based SaaS customer churn, revenue trends, and retention drivers using 12 months of transaction data.

## 📊 Project Overview

This project investigates why SaaS customers churn and their financial impact. By analyzing 500+ customer subscriptions and monthly aggregate metrics, it identifies high-risk segments, key churn drivers, and actionable retention strategies.

**Key Metrics:**
- Monthly churn rate: 4.52%
- Total active customers: 281
- Total MRR: $8.39M
- Total customers churned: 313

## 📁 Data Sources

Two CSV files power this analysis:

### `monthly_revenue.csv`
- Monthly aggregated metrics from 2022-01 to 2025-02
- Columns: month, total_active_customers, new_customers, churned_customers, monthly_churn_rate_pct, total_mrr, avg_revenue_per_customer, customer_acquisition_cost

### `subscriptions.csv`
- 600 individual customer records with churn labels
- Columns: customer_id, plan, billing_cycle, industry, company_size, seats, monthly_revenue, acquisition_channel, region, signup_date, churned, churn_date, churn_reason, support_tickets_12mo, nps_score, feature_usage_pct, upgraded

## 🛠️ Tools & Methods

- **BI Tool:** Power BI Desktop
- **Data Transformation:** Power Query
- **Analysis:** DAX measures for churn rate, tenure, lost revenue, and segmentation
- **Visualization:** Multi-page interactive dashboard with slicers

### Key Calculated Columns (DAX)

```DAX
Is Churned = IF('Subs'[churned] = "Yes", 1, 0)

Tenure Days = 
IF(
    'Subs'[churned] = "Yes",
    DATEDIFF('Subs'[signup_date], 'Subs'[churn_date], DAY),
    DATEDIFF('Subs'[signup_date], DATE(2025,12,31), DAY)
)

Churn Rate = DIVIDE([Churned Customers], [Customers])
Lost Revenue = SUMX(FILTER('Subs', 'Subs'[Is Churned] = 1), 'Subs'[monthly_revenue])
```

## 📈 Key Insights

### 1. Churn Trends Over Time
- Monthly churn rate fluctuates between 15% and 30%, averaging 4.52%.
- Peak churn observed in early 2024; slight decline in recent months.
- Despite churn, total MRR grows from $50K (Jan 2022) to $8.39M (Feb 2025), indicating strong new customer acquisition.

### 2. High-Churn Segments

**By Plan & Billing Cycle:**
- **Starter + Monthly:** ~30% churn rate (highest risk)
- **Business + Monthly:** ~20% churn rate
- **Professional/Enterprise + Annual:** <10% churn rate (lowest risk)

**By Industry:**
- Real Estate and Finance churn at 25–28% (highest)
- Consulting and Healthcare at 18–20%
- Technology and Manufacturing at 15–18% (most stable)

**By Acquisition Channel:**
- Referral: ~18% (best-performing channel)
- Organic Search: ~19%
- Paid Ads & Partner: ~22–24% (highest churn)

### 3. Product Usage & Support Quality
- **Churned customers** have significantly lower feature_usage_pct (avg: ~30%) vs active customers (avg: ~65%).
- **NPS gap:** Churned avg 4.6 vs active avg 8.5—major difference in satisfaction.
- **Support tickets:** Churned customers with low support tickets often cite "Missing Features"; those with high tickets cite "Poor Support."

### 4. Top Churn Reasons & Revenue Impact

| Churn Reason | # Customers | % of Total | Revenue Lost |
|---|---|---|---|
| **Budget Cuts** | ~80 | 26% | Moderate |
| **Price Too High** | ~75 | 24% | Moderate–High |
| **Company Closed** | ~40 | 13% | Low |
| **Poor Support** | ~35 | 11% | Moderate |
| **Missing Features** | ~35 | 11% | High (larger deals) |
| **Switched Competitor** | ~30 | 10% | Moderate |
| **No Longer Needed** | ~18 | 5% | Low |

---

## 💡 Recommendations

### 1. Pricing & Billing Strategy
- **Experiment with annual discounts** for Starter and Business plans to shift Monthly → Annual (currently 20–30% lower churn on annual).
- Test tiered pricing for Starter plan (entry-level tier to reduce "Price Too High" churn).
- Target high-value customers (Professional/Enterprise) in Real Estate & Finance with retention discounts.

### 2. Product & Feature Development
- Prioritize feature roadmap for top 3 missing features cited in churn reasons.
- Create feature onboarding campaigns for low feature_usage_pct customers (especially Starter and Monthly cohorts).
- Segment by industry: Finance and Real Estate may need industry-specific features.

### 3. Support & Customer Success
- Implement SLA improvements for "Poor Support" cohorts (Education, Healthcare, Finance).
- Launch proactive outreach for customers with:
  - NPS < 5 and feature_usage_pct < 40%
  - Support tickets > 10 (indicating friction)
  - Acquisition via Paid Ads (highest churn channel)

### 4. Retention Campaigns
- **30-day at-risk:** Identify churners 2–3 months before expiry (low usage + low NPS + price-sensitive plan).
- **Win-back:** Re-engage churned customers with "Budget Cuts" reason (likely to return post-recovery).
- **Upgrade funnel:** Promote Starter → Business → Professional (lower churn at higher tiers).

### 5. Acquisition Channel Optimization
- Double down on **Referral** and **Organic Search** (lowest churn).
- Audit **Paid Ads** and **Partner** acquisition quality (highest churn → likely low product-market fit for those segments).

---

## 📊 Dashboard Pages

1. **Overview** – KPI cards, churn rate trend, MRR growth, CAC over time.
2. **Segmentation** – Churn by plan, billing_cycle, industry, region, acquisition_channel, and feature usage vs NPS.
3. **Churn Reasons** – Customer count and revenue lost by churn_reason.

Interactive slicers on all pages: Plan, Billing Cycle, Region, Industry.

---

## 📂 Repository Structure

```
saas-churn-analysis/
├── README.md                    # This file
├── subscriptions.csv            # Customer subscription data
├── monthly_revenue.csv          # Monthly aggregate metrics
├── SaaS_Churn_Dashboard.pbix    # Power BI dashboard file
└── insights_summary.md          # Detailed findings (optional)
```

---

## 🎯 How to Use This Project

1. **Download the `.pbix` file** and open in Power BI Desktop.
2. **Review the Overview page** for headline metrics and trends.
3. **Use slicers** to drill into specific plans, regions, or industries.
4. **Cross-reference findings** with the insights section above to support business decisions.
5. **Adapt recommendations** to your SaaS product's context (pricing model, market, etc.).

---

## 🚀 Next Steps (Optional)

- **Predictive churn modeling:** Train a classification model (logistic regression / decision tree) on subscription features to predict churn probability.
- **Cohort analysis:** Analyze churn by signup month cohorts to track retention over customer lifetime.
- **RFM segmentation:** Combine Recency, Frequency, Monetary metrics for targeted retention offers.
- **Win/loss analysis:** Interview churned customers to validate quantitative findings.

---

## 📝 Notes

- Data spans Jan 2022 – Feb 2025.
- Snapshot date for active customer tenure: 31 Dec 2025.
- All financial figures in USD.
- Churn rate calculated as: churned_customers / total_active_customers per month.

---

## 📧 Contact & Portfolio

This project demonstrates:
- ✅ Data cleaning and transformation (Power Query)
- ✅ DAX formula development (calculated columns & measures)
- ✅ Multi-page dashboard design with slicers
- ✅ Business insight derivation and storytelling
- ✅ Actionable recommendations from data

**Perfect for:** Data Analyst, Business Intelligence, Analytics Engineer roles.
