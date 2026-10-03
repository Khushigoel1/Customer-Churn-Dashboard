# Customer-Churn-Dashboard

An interactive **Power BI dashboard** that analyzes telecom customer churn, identifies high-risk customer segments, and highlights the key factors associated with customer cancellations.

## 🎯 Business Problem

Customer retention is critical for subscription businesses. This dashboard helps answer:

* Who is most likely to churn?
* What characteristics are common among churned customers?
* Which customer segments should the retention team prioritize?

## 📂 Dataset

**Telco Customer Churn Dataset**

* **Customers:** 7,043
* **Columns:** 21
* **Target:** Churn (Yes/No)
* Includes demographics, account details, services, billing, and contract information.

## 🛠️ Tools Used

* **Power BI** – Dashboard & data visualization
* **Power Query** – Data cleaning and transformation
* **DAX** – Measures and calculations
* **RFM-style Risk Score** – Customer risk segmentation

## 🔄 Project Workflow

**Data → Cleaning → Transformation → Risk Scoring → DAX Measures → Dashboard → Insights**

Key transformations include:

* Cleaning `TotalCharges`
* Creating Churn Flag
* Creating Tenure Groups
* Calculating Services Count
* Creating Contract Order and Charge Bands
* Assigning customers to Risk Segments

## ⚠️ Risk Segmentation

A rule-based RFM-style score was created using **tenure, contract, payment method, and monthly charges**.

| Risk Segment   | Score | Churn Rate |
| -------------- | ----: | ---------: |
| 🔴 Critical    |   4–5 |      70.2% |
| 🟠 High Risk   |   6–7 |      46.6% |
| 🟡 Medium Risk |  8–10 |      18.7% |
| 🟢 Low Risk    | 11–14 |       3.1% |

**Higher score = lower churn risk.**

## 📈 Dashboard

The dashboard includes:

* KPI cards for customers, churn rate, churned customers and revenue lost
* Interactive slicers
* Contract vs. payment method churn heatmap
* Churn analysis by contract and services
* Risk segment distribution
* Critical customer retention call list

## 🔍 Key Findings

* Overall churn rate is **26.5%**.
* **Month-to-month contracts** have the highest churn.
* Customers in their **first 12 months** are at greater risk.
* **Electronic check** users show higher churn.
* **Fiber optic** customers have higher churn than DSL customers.
* Customers without **Tech Support** show higher churn.
* Churned customers generate higher average monthly charges.

## 💡 Recommendations

* Encourage customers to move to longer-term contracts.
* Strengthen onboarding during the first year.
* Promote automatic payment methods.
* Investigate fiber service pricing and quality.
* Offer Tech Support and Online Security bundles.
* Prioritize **Critical Risk** customers for retention campaigns.

## ⚠️ Limitations

The risk score is a **rule-based scoring system, not a predictive ML model**. The dataset is a single snapshot, so changes over time cannot be analyzed. Findings indicate **associations, not causation**.

## 👩‍💻 Author

**Khushi Goel**

B.Tech – Artificial Intelligence & Machine Learning
