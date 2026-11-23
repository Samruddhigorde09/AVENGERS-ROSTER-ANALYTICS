# Avengers Roster Analytics

An end-to-end data analytics project that studies **who joins the Avengers, who matters most, how often members die, and how often they come back**. It covers the full workflow: a Business Requirements Document (BRD), data cleaning in **SQL Server**, preparation and segmentation in **Python**, and a three-page **Power BI** dashboard.

> This is an unofficial, educational portfolio project. It is not affiliated with or endorsed by Marvel.

---

## Dashboard Preview

| Home | Roster Overview | Mortality & Resurrection |
|---|---|---|
| ![Home](images/home.png) | ![Roster Overview](images/roster_overview.png) | ![Mortality](images/mortality.png) |

---

## Business Problem

Over six decades the Avengers roster has grown to **173 characters**, and many have died and returned more than once. There was no structured view of which members matter most, how dangerous membership is, or how reversible it is. This project answers those questions so that editorial, content-strategy and marketing teams can decide with evidence instead of intuition.

### Business questions answered
1. Which members are the most prominent (by comic appearances)?
2. How many members are active, and how many have left?
3. How has recruitment changed by decade?
4. How does membership differ by gender and membership type?
5. How often do members die, and how many come back?
6. Does tenure affect the chance of dying?
7. What action should be taken for each member segment?

The full requirements are in [`docs/Avengers_Roster_Analytics_BRD.pdf`](docs/Avengers_Roster_Analytics_BRD.pdf).

---

## Tools and Skills

| Area | Tools |
|---|---|
| Data cleaning | SQL Server (T-SQL), SSMS |
| Preparation and analysis | Python (pandas, NumPy, SQLAlchemy), Jupyter Notebook |
| Visualization | Power BI Desktop, DAX |
| Documentation | Business Requirements Document, Markdown |

---

## Dataset

- **File:** `data/Avengers.xlsx` (173 rows, 22 columns, one row per character)
- **Contents:** appearances, active status, gender, membership type, join year, tenure, up to five deaths and returns per character, and notes
- **Source:** based on the public Avengers roster dataset published by FiveThirtyEight. Check the original source and licence before reusing the data.

### Data quality issues found and fixed

| Issue | Treatment |
|---|---|
| One row (Nicholas Fury Jr.) had its columns shifted | Re-aligned manually in SQL |
| 14 rows had a placeholder join year of 1900 | Set to NULL and excluded from tenure analysis |
| Intro dates were corrupted by Excel (many show 2024) | Dropped; the `Year` column is used instead |
| 10 rows had no name | Filled in from the wiki URL |
| Empty trailing column and a misspelled header | Removed and renamed |
| Death and return fields spread over 10 columns | Converted to counts and flags |

---

## Project Workflow

```
Avengers.xlsx  ->  SQL Server (clean)  ->  Python (segment + features)  ->  Power BI (dashboard)
```

1. **SQL cleaning** (`sql/avengers_cleaning_sqlserver.sql`): builds a staging table, repairs the broken row, standardises text, fills missing names and creates `dbo.avengers_clean` with validation checks.
2. **Python preparation** (`notebooks/Avengers_Python_Preparation.ipynb`): loads the clean table, runs data checks, adds features (join decade, tenure group, appearance share, death status) and assigns member segments. Saves `avengers_final.csv` and the SQL table `dbo.avengers_final`.
3. **Power BI dashboard** (`powerbi/`): three pages built on `avengers_final` with DAX measures.

### Member segments

Each member belongs to exactly one segment, checked in this order:

| Segment | Rule |
|---|---|
| Core Veterans | Full member, active, top-quartile appearances, tenure over 10 years |
| Resurrected | Died at least once and returned |
| Reserve & Academy | Honorary, Academy or Probationary member |
| Former Members | Not currently active |
| New Recruits | Active, joined within the last 10 years |
| Active Regulars | All other active members |

---

## Key Findings

- The roster has **173 members**; **82 (47%)** are currently active.
- **69 members (40%)** have died at least once, and **46 of them (67%)** came back. **16** died more than once.
- **Core Veterans are only 18 members (10%) but account for about 41% of all appearances.**
- **Peter Parker** has the most appearances (4,333), followed by Steven Rogers (3,458).
- The **2010s were the biggest recruitment decade** with 52 members joining.
- The death rate **rises with tenure**, from **29%** (0-5 years) to **76%** (40+ years).
- By membership type, the death rate is 44% for Full members, 31% for Honorary and 12% for Academy. Probationary shows 50%, but it has only 2 members.
- Male members have a slightly higher death rate (42%) than female members (36%).

### Recommended actions

| Segment | Action |
|---|---|
| Core Veterans | Anchor flagship storylines and campaigns |
| Former Members | Plan legacy and nostalgia content |
| Resurrected | Feature comeback arcs and study what makes returns work |
| New Recruits | Use spotlight stories to build familiarity |
| Reserve & Academy | Use as supporting cast and crossover entry points |
| Active Regulars | Review for a larger role |

---

## Repository Structure

```
avengers-roster-analytics/
|-- README.md
|-- data/
|   |-- Avengers.xlsx
|   `-- avengers_final.csv
|-- docs/
|   `-- Avengers_Roster_Analytics_BRD.pdf
|-- sql/
|   `-- avengers_cleaning_sqlserver.sql
|-- notebooks/
|   `-- Avengers_Python_Preparation.ipynb
|-- powerbi/
|   |-- Avengers_Roster_Analytics.pbix
|   `-- Avengers_Navy_Theme.json
`-- images/
    |-- home.png
    |-- roster_overview.png
    `-- mortality.png
```

---

## How to Reproduce

1. **Load the data:** import `data/Avengers.xlsx` into SQL Server as `dbo.avengers` in a database named `Avengers_Analytics`.
2. **Clean it:** open `sql/avengers_cleaning_sqlserver.sql` in SSMS and run the whole script. Confirm the checks match: 173 rows, 82 active, 69 died, 46 returned, 16 died more than once.
3. **Prepare it in Python:**
   ```bash
   pip install pandas numpy sqlalchemy pyodbc jupyter
   jupyter notebook notebooks/Avengers_Python_Preparation.ipynb
   ```
   Edit the settings cell (`SERVER`, `DATABASE`, `DRIVER`) and choose **Kernel > Restart & Run All**. To work without SQL Server, set `USE_SQL = False` and use the CSV.
4. **Open the dashboard:** open `powerbi/Avengers_Roster_Analytics.pbix` in Power BI Desktop and update the data source to your server if needed.

---

## Author

**[Your Name]** - Data Analyst
[LinkedIn](https://www.linkedin.com/) | [GitHub](https://github.com/) | [Email](mailto:your.email@example.com)

Feedback and suggestions are welcome.
