# Customer-Churn-Dashboard
An interactive Power BI dashboard that shows which telecom customers are leaving, and why, built on the Telco Customer Churn dataset. It uses Power Query for cleaning, DAX for calculations, and an RFM-style risk score to flag at-risk customer segments.
**Table of contents**
Business problem
Dataset
Tools used
Project workflow
Risk scoring (RFM-style)
DAX measures
Dashboard contents
Key findings
Recommendations
How to reproduce
Project structure
Limitations
Business problem

Winning a new customer typically costs far more than keeping an existing one, so a subscription business needs to know:

Who is most likely to cancel?
What do those customers have in common?
Where should the retention team focus its effort and budget?

This dashboard answers those questions at a glance for a non-technical audience.

Dataset
Source: IBM Telco Customer Churn dataset (WA_Fn-UseC_-Telco-Customer-Churn.csv)
Size: 7,043 customers, 21 columns, no duplicate customer IDs
Target column: Churn (Yes = customer left, No = stayed)
Group	Columns
Demographics	gender, SeniorCitizen, Partner, Dependents
Account	tenure, Contract, PaperlessBilling, PaymentMethod
Services	PhoneService, MultipleLines, InternetService, OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies
Billing	MonthlyCharges, TotalCharges
Outcome	Churn
Tools used
Power BI Desktop (data model, DAX, visuals)
Power Query (data cleaning and calculated columns)
DAX (measures and score columns)
Project workflow
1. Data cleaning (Power Query)
TotalCharges was stored as text and 11 rows contained a blank space. All 11 customers have tenure = 0 (brand new), so blanks were replaced with 0 and the column converted to Decimal Number.
Data types checked for every column (tenure as Whole Number, MonthlyCharges as Decimal, the rest as Text).
Text columns trimmed; column quality checked for errors and empties.
SeniorCitizen (0/1) converted to a readable Senior / Non-Senior label.
2. Calculated columns
Column	Purpose
Churn Flag	1 if churned, 0 otherwise (makes averages and sums easy)
Tenure Group	0-12 mo, 13-24 mo, 25-48 mo, 49-72 mo
Services Count	Number of services a customer uses (usage pattern)
Contract Order	Sort key so contracts display Month-to-month, One year, Two year
Charge Band	Low, Medium, High, Very High based on monthly charge quartiles
Risk scoring (RFM-style)

Classic RFM (Recency, Frequency, Monetary) is built for repeat purchases. A subscription business has no purchase dates, so it was adapted. A first attempt using tenure, service count, and monthly charges (higher = better) did not separate churners well, so the final score uses the factors that actually differ between churners and stayers.

**Score	Based on	Points**
R (Recency-style)	Tenure	0-12 mo = 1, 13-24 = 2, 25-48 = 3, 49+ = 4
F (Frequency-style)	Contract commitment	Month-to-month = 1, One year = 3, Two year = 4
P	Payment method	Electronic check = 1, all others = 2
M (Monetary)	Monthly charges (higher bill = higher risk)	<=35.5 = 4, <=70.35 = 3, <=89.85 = 2, above = 1

Risk Score = R + F + P + M (range 4-14, higher = safer)

Risk Segment	Score	Customers	Churn rate
Critical	4-5	729	70.2%
High Risk	6-7	1,863	46.6%
Medium Risk	8-10	2,256	18.7%
Low Risk	11-14	2,195	3.1%
**DAX measures**
dax
Total Customers      = COUNTROWS(Telco)
Churned Customers    = CALCULATE(COUNTROWS(Telco), Telco[Churn] = "Yes")
Churn Rate           = DIVIDE([Churned Customers], [Total Customers])
Monthly Revenue Lost = CALCULATE(SUM(Telco[MonthlyCharges]), Telco[Churn] = "Yes")
Avg Tenure           = AVERAGE(Telco[tenure])
**Dashboard contents**
Visual	Fields	Purpose
KPI cards (4)	Total Customers, Churn Rate, Churned Customers, Monthly Revenue Lost	Headline numbers
Slicers	Contract, Internet Service, Senior, Tenure Group, Risk Segment	Filter the whole report
Matrix heatmap	Rows: Contract, Columns: PaymentMethod, Values: Churn Rate	Show where churn concentrates
Combo chart	X: Contract, Columns: Total Customers, Line: Churn Rate	Commitment vs. retention
Clustered bars	Churn Rate by InternetService, PaymentMethod, TechSupport (by Contract)	Compare customer profiles
Risk donut	Risk Segment by Total Customers	Size of each risk group
Critical table	customerID, tenure, Contract, MonthlyCharges (filtered to Critical)	Retention call list

The dashboard uses a dark theme (dark-churn-theme.json) with red for churn, teal for healthy, and neutral blue for volume.

**Key findings**
Overall churn is 26.5% (1,869 of 7,043 customers), costing about $139K in monthly revenue (30.5% of the $456K total). Churners pay more per month on average ($74 vs. $61).
Contract is the strongest signal: Month-to-month churns at 42.7%, One year at 11.3%, Two year at 2.8%. Month-to-month customers make up about 89% of all churners.
The first year is the danger zone: customers with 0-12 months tenure churn at 47.4%, versus 9.5% after 4 years.
Electronic check payers churn most (45.3%). Month-to-month plus electronic check reaches 53.7%.
Fiber optic customers churn at 41.9%, versus 19.0% for DSL and 7.4% with no internet service.
Support matters: customers without tech support churn at 41.6% versus 15.2% with it.
Senior citizens churn more (41.7% vs. 23.6%). Gender shows no meaningful difference (26.9% vs. 26.2%).
**Recommendations**
Offer incentives to move month-to-month customers onto 1-2 year plans.
Invest in onboarding and check-ins during the first 12 months.
Encourage auto-pay (bank transfer or credit card) over electronic check.
Investigate fiber optic pricing and service quality.
Bundle tech support and online security into plans.
Use the Critical segment table as the priority list for retention outreach.
**Limitations**
The dataset has no region or location column, so the "regional heatmap" uses Contract by Payment Method instead.
The risk score is a simple, explainable rule-based score, not a predictive model. Weights were chosen by comparing churn rates across groups.
Findings show association, not causation. For example, fiber customers churning more could reflect price, competition, or service quality.
The data is a single snapshot, so trends over time cannot be analyzed.
**Author**
Khushi goel
Findings show association, not causation. For example, fiber customers churning more could reflect price, competition, or service quality.
The data is a single snapshot, so trends over time cannot be analyzed.
Author
