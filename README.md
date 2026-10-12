# 📉 Customer Churn Analytics (Python + SQLite)

An end-to-end customer churn analysis for a subscription-based streaming service. The project pulls data from a **SQLite database with three tables**, cleans and merges it with **pandas**, engineers churn-related features, calculates key business KPIs, and visualises the patterns behind customer cancellations.

---

## 📂 Repository Structure

```
├── Customer_churn_analytics.ipynb   # Full analysis: import → clean → feature engineering → KPIs → visuals
├── exported_churn_data.csv          # Final cleaned & merged dataset (21 customers, 21 columns)
├── customer_churn.db                # Source SQLite database (3 tables)  [add if you want the notebook to be re-runnable]
└── README.md
```

---

## 🗄️ Data Sources

The raw data lives in a SQLite database (`customer_churn.db`) with three related tables, linked by `customerid`:

| Table | Description | Key columns |
|---|---|---|
| `db_customer` | Customer demographics | name, country, state, gender, dob |
| `db_subscription` | Subscription & billing details | subscription_type, plan_type, contract_type, monthly_charges, cltv, churn_score, cancellation_date/reason |
| `db_support` | Support complaints | complaint_date, escalations, csat_score |

> ⚠️ This is a small sample dataset (**21 customers**), built to practise the full analytics workflow. Findings are illustrative, not statistically conclusive.

---

## 🛠️ Tools & Libraries

- **Python 3** – `pandas`, `numpy`
- **SQLite** (`sqlite3`) – reading source tables
- **Matplotlib & Seaborn** – visualisation
- **Jupyter Notebook**

---

## 🔄 Project Workflow

### 1. Import Data
Connected to the SQLite database, listed all tables from `sqlite_master`, and loaded each one into its own DataFrame.

### 2. Data Cleaning
- **Customer table:** renamed `name` → `customer_name`; dropped the empty `interests` and `pincode` columns; converted `dob` to datetime; standardised gender labels (`Men`/`Women` → `Male`/`Female`); filled missing `country` values by mapping from `state`.
- **Subscription table:** converted `subscription_start_date`, `renewal_date`, and `cancellation_date` to datetime.
- **Support table:** dropped the empty `col_1` and `comment` columns; converted `complaint_date` to datetime.

### 3. Feature Engineering
- **`churn_flag`**: 1 if a customer has a cancellation date, otherwise 0
- **`complaint_count`**: number of complaints per customer
- Kept each customer's **latest complaint** only, so the tables merge to one row per customer
- Merged all three tables on `customerid` and exported the result to `exported_churn_data.csv`
- Later additions: **`customer_age`**, **`tenure_days`**, and **`churn_risk`** (low `<50`, med `50–69`, high `≥70`, based on `churn_score`)

### 4. KPI Analysis

| KPI | Result |
|---|---|
| Churn rate | **28.57%** (6 of 21 customers) |
| Retention rate | **71.43%** |
| Average revenue per user (monthly) | **$18.85** |
| Average tenure | **~1,542 days** |
| Monthly revenue at risk (churned customers) | **$73.94** |
| Escalation rate | **19.05%** |
| Average complaints per customer | **0.43** |
| Correlation: escalations vs. churn | **0.77** |

### 5. Visualisation
- Monthly churn trend (line chart)
- Churn rate by plan type and by state (bar charts)
- Correlation heatmap, pair plot, and a multi-panel `catplot` of monthly charges by plan, gender, and churn risk

---

## 🔍 Key Insights

- **Basic plan customers churn the most:** 60% churn vs. 22% on Standard and 14% on Premium.
- **Referral customers are the biggest risk:** 83% churned (5 of 6), versus 0% for Organic and 17% for Paid.
- **Contract type matters:** Monthly contracts show 56% churn, against 8% for Annual contracts.
- **Support problems predict churn:** every customer with an escalated complaint churned (4 of 4), and escalations correlate strongly with churn (0.77).
- **Churn score works as a warning signal:** churned customers averaged a score of 86, versus 26 for retained customers.
- **Lower lifetime value:** churned customers have an average CLTV of about 341, compared with about 1,016 for retained ones.
- **Cancellation reasons are varied:** switching to a competitor was most common (2 of 6), followed by price, content, streaming quality, and a forgotten trial.

---

## 💡 Recommendations

- Offer **onboarding and upgrade incentives** to Basic-plan users.
- Review the **referral programme**, since referred customers may be joining for a one-time incentive and leaving.
- Encourage **annual plans** with discounted pricing.
- Set up **fast-track resolution for escalated complaints** and use `churn_score ≥ 70` as a trigger for retention outreach.

---

## 🚀 How to Run

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn jupyter

# 3. Launch the notebook
jupyter notebook Customer_churn_analytics.ipynb
```

The notebook expects `customer_churn.db` in the same folder. If you only want to explore the final data, open `exported_churn_data.csv` directly with pandas.

---

## ⚠️ Limitations

- **Very small sample (21 customers, 6 churned),** so percentages swing a lot with each customer and should be read as patterns to investigate, not proven effects.
- `escalations` is blank for customers with no complaint; the notebook treats these as "no escalation".
- `customer_age` and `tenure_days` for active customers use today's date, so values change each time the notebook runs.

---

## 💡 Skills Demonstrated

- Reading and combining multiple SQL tables in Python
- Data cleaning and standardisation
- Feature engineering and KPI design (churn, retention, ARPU, tenure, revenue at risk)
- Exploratory data analysis and storytelling with visuals
- Translating findings into business recommendations

---

