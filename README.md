**🏆 Nobel Legacy: 120 Years of Excellence
**From Raw Data to Decision Intelligence (1901–2023)

An end-to-end Data Analytics & Business Intelligence project that demonstrates the complete analytics lifecycle — Descriptive, Diagnostic, Predictive, and Prescriptive Analytics — using historical Nobel Prize data.
This project goes beyond static dashboards to deliver decision intelligence, combining Python-based data engineering with advanced Power BI analytics.

**📌 Project Overview
**The Nobel Prize represents the highest level of global achievement, yet patterns around recognition age, gender representation, and institutional dominance remain underexplored.
This project answers four critical analytical questions:
- What happened? → Descriptive Analytics
- Why did it happen? → Diagnostic Analytics
- What might happen next? → Predictive Analytics
- What should we do about it? → Prescriptive Analytics

**🧠 Key Objectives
**- Analyze 120+ years of Nobel Prize history
- Identify structural patterns in recognition age and diversity
- Forecast future Nobel trends
- Translate insights into actionable, policy-oriented recommendations

**🛠️ Tech Stack
**
| Tool                       | Purpose                                |
| -------------------------- | -------------------------------------- |
| **Python (Pandas, NumPy)** | Data cleaning, feature engineering     |
| **Power BI**               | Visualization, modeling, forecasting   |
| **DAX**                    | KPIs, indices, dynamic recommendations |
| **Power BI AI Visuals**    | Key Influencers, Forecasting           |
| **GitHub**                 | Version control & documentation        |

**📂 Dataset
**
Source: Kaggle – Nobel Prize Winners Dataset
Time Period: 1901–2023
Records: 911 rows
Final Laureates Analyzed: 904

**⚙️ Data Engineering & Feature Engineering (Python)
**
Key preprocessing steps:
- Removed null inconsistencies and standardized categorical values
- Created a logic-driven is_alive flag using death_date
- Set age = 0 for organizations to preserve statistical validity for human age analysis
- Created age groups and decade buckets
- Maintained separate date fields and display fields for Power BI compatibility
- Example:
  data["is_alive"] = data["death_date"].isnull()
  data.loc[data["laureate_type"] == "Organization", "age"] = 0

**📊 Dashboard Structure (Power BI)
****🟦 Page 1 — Descriptive Analytics (What Happened?)
**
KPIs:
- Total Laureates: 904
- Countries Represented: 122
- Avg Award Age: 59.46
- Female Representation: 5.31%
- Active Legacy (Alive): 34.85%
Visuals:
- Global distribution map
- Category-wise laureate count
- Decade-based award trends
- Gender distribution by field
DAX Measures:
- TOTAL LAUREATE = DISTINCTCOUNT(Nobel_Prize_Winners_Cleaned[laureate_id])
- TOTAL COUNTRIES = DISTINCTCOUNT(Nobel_Prize_Winners_Cleaned[birth_country])
- AVG AGE WINNER = AVERAGEX(FILTER(Nobel_Prize_Winners_Cleaned, Nobel_Prize_Winners_Cleaned[age]>0),Nobel_Prize_Winners_Cleaned[age])
- FEMALE% = DIVIDE([FEMALE WINNERS],[TOTAL LAUREATE],0)
- ACTIVE LEGACY% = DIVIDE([ACTIVE LEGACY],[TOTAL LAUREATE],0)

**🟦 Page 2 — Diagnostic Analytics (Why Did It Happen?)
**
Insights
- Award age increased by +6.92 years since 1900
- Gender age gap of 3.75 years
- 37.28% of Nobel Prizes awarded by just 5 institutions
Visuals
- Age vs. Year scatter (Aging of Genius)
- Gender gap by category
- Evolution of Collaboration
- Organization Nobel Winners
- country v/s category nobel prize
DAX Measures:
- TOP ORGANIZATION PRIZE = MAXX(VALUES(Nobel_Prize_Winners_Cleaned[organization_name]),CALCULATE(COUNT(Nobel_Prize_Winners_Cleaned[laureate_id])))
- AVG AGE DECADE SHIFT = CALCULATE(AVERAGE(Nobel_Prize_Winners_Cleaned[age]),Nobel_Prize_Winners_Cleaned[decade]==2000)- CALCULATE(AVERAGE(Nobel_Prize_Winners_Cleaned[age]),Nobel_Prize_Winners_Cleaned[decade]==1900)
- M-F AVG AGE GAP = CALCULATE(AVERAGE(Nobel_Prize_Winners_Cleaned[age]),Nobel_Prize_Winners_Cleaned[sex]=="Male")- CALCULATE(AVERAGE(Nobel_Prize_Winners_Cleaned[age]),Nobel_Prize_Winners_Cleaned[sex]=="Female")
- SOLO WIN% = DIVIDE([SOLO WIN],[TOTAL LAUREATE],0)
- SHARED WIN% = DIVIDE([SHARED WIN],[TOTAL LAUREATE],0)

**🟦 Page 3 — Predictive Analytics (What Might Happen?)
**
Forecasting & Trend Analysis
- Estimated 1000th Nobel Laureate by ~2030
- Projected average award age ≈ 70 years by 2030
- Forecasted award growth using Power BI Analytics pane
KPIs
- Estimated 1000th Year
- Projected Avg Age (2030)
- Future Growth Index
- Legacy Sustainability%
Visuals:
- Growth of Excellence
- Projected age at nobel award
- Female participation trend over time
- Life status outlook by category
- Shift towards collaborative wins
Dax Measures:
- ESTIMATED 1000TH YEAR = 
VAR CurrentTotal =
    CALCULATE(
        [TOTAL LAUREATE],
        ALL(Nobel_Prize_Winners_Cleaned)
    )
VAR Target = 1000
VAR Needed = Target - CurrentTotal
VAR StartYear = 1901
VAR EndYear =
    CALCULATE(
        MAX(Nobel_Prize_Winners_Cleaned[year]),
        ALL(Nobel_Prize_Winners_Cleaned)
    )
VAR YearsElapsed = EndYear - StartYear
VAR WinnersPerYear = DIVIDE(CurrentTotal, YearsElapsed)
RETURN
ROUND(EndYear + DIVIDE(Needed, WinnersPerYear), 0)

- PROJECTED AVG AGE 2030 = 
VAR BaseData =
    FILTER(
        ALL(Nobel_Prize_Winners_Cleaned),
        Nobel_Prize_Winners_Cleaned[age] > 0 &&
        Nobel_Prize_Winners_Cleaned[year] >= 1980
    )

VAR Avg1980s =
    CALCULATE([AVG AGE WINNER], BaseData, Nobel_Prize_Winners_Cleaned[decade] = 1980)

VAR Avg2010s =
    CALCULATE([AVG AGE WINNER], BaseData, Nobel_Prize_Winners_Cleaned[decade] = 2010)

VAR GrowthPerYear =
    DIVIDE(Avg2010s - Avg1980s, 2010 - 1980)

RETURN
ROUND(Avg2010s + (2030 - 2010) * GrowthPerYear, 2)
- LEGACY SUSTAINABILITY% = 
DIVIDE(
    CALCULATE([TOTAL LAUREATE], Nobel_Prize_Winners_Cleaned[is_alive] = TRUE),
    [TOTAL LAUREATE],
    0
)

- FUTURE GROWTH INDEX = 
VAR EarlyPeriod =
    CALCULATE(
        [TOTAL LAUREATE],
        ALL(Nobel_Prize_Winners_Cleaned),
        Nobel_Prize_Winners_Cleaned[decade] IN {1960, 1970, 1980}
    )

VAR RecentPeriod =
    CALCULATE(
        [TOTAL LAUREATE],
        ALL(Nobel_Prize_Winners_Cleaned),
        Nobel_Prize_Winners_Cleaned[decade] IN {1990, 2000, 2010}
    )

RETURN
DIVIDE(RecentPeriod - EarlyPeriod, EarlyPeriod, 0)

**🟦 Page 4 — Prescriptive Analytics (What Should We Do?)
**This page transforms insights into actions.
Prescriptive KPIs
- Early Career % → 5.53%
- Diversity Parity vs Target (20%) → 14.69%
- Recognition Delay Index → 1.44
- Top 5 Organization % → 37.28%
Charts:
- What drives early?
- Research funding realignment
- Path to female gender parity
DAX:
- EARLY CAREER% = 
DIVIDE(
    CALCULATE(
        DISTINCTCOUNT(Nobel_Prize_Winners_Cleaned[laureate_id]),
        Nobel_Prize_Winners_Cleaned[age] > 0 && Nobel_Prize_Winners_Cleaned[age] < 40
    ),
    DISTINCTCOUNT(Nobel_Prize_Winners_Cleaned[laureate_id]),
    0
)

- DIVERSITY PARITY TARGET(20%) = 
VAR Target = 0.20
VAR CurrentRatio = [FEMALE%]
RETURN
Target - CurrentRatio

- RECOGNITION DELAY INDEX = 
VAR IdealAge = 40
VAR CurrentAvg = AVERAGE(Nobel_Prize_Winners_Cleaned[age])
RETURN
DIVIDE(CurrentAvg, IdealAge, 0)

- TOP 5 ORGANIZATION% = 
VAR Total = [TOTAL LAUREATE]
VAR Top5 =
    CALCULATE(
        DISTINCTCOUNT(Nobel_Prize_Winners_Cleaned[laureate_id]),
        TOPN(
            5,
            VALUES(Nobel_Prize_Winners_Cleaned[organization_name]),
            [TOTAL LAUREATE]
        )
    )
RETURN
DIVIDE(Top5, Total, 0)

**🧠 Key Takeaways
**
- Nobel recognition is increasingly late-career, limiting early innovation incentives
- Female representation remains significantly below parity targets
- Institutional concentration restricts global research equity
- Data-driven interventions can guide funding, diversity, and policy reforms

**🎯 Final Outcome
**
This project delivers a decision intelligence framework that demonstrates how analytics can evolve from:
Data → Insight → Prediction → Prescription

It is designed for:
- Data Analyst / BI Analyst roles
- Analytics interviews
- Executive-style storytelling portfolios

**🤝 Feedback & Opportunities
**
I welcome feedback from the data community and am open to opportunities where analytics drives strategic decisions, not just reporting.

**🔖 Tags
**Power BI, Python, DAX, Data Analytics, Prescriptive Analytics, Decision Intelligence Dashboard, Design, Data Storytelling

PowerBI: https://app.powerbi.com/links/9rfcv9L3nk?ctid=1e3bab2c-ff49-4d6c-827a-5017e6fd859c&pbi_source=linkShare
LinkedIn: https://www.linkedin.com/in/jyotigupta-da/
Author: Jyoti Gupta
