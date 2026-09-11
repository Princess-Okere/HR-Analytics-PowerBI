# HR-Analytics-PowerBI
HR analytics project analyzing workforce composition, employee turnover, retention, demographics, and compensation using Power BI.

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Dataset & Tools](#2-dataset--tools)
3. [Data Cleaning & Transformation](#3-data-cleaning--transformation)
4. [Data Quality Notes & Assumptions](#4-data-quality-notes--assumptions)
5. [Data Model](#5-data-model)
6. [Dashboard Walkthrough](#6-dashboard-walkthrough)
   - [6.1 Workforce Overview](#61-workforce-overview)
   - [6.2 Turnover & Retention](#62-turnover--retention)
   - [6.3 Engagement, Performance & Recruitment](#63-engagement-performance--recruitment)
7. [Key Findings](#7-key-findings)
8. [Recommendations](#8-recommendations)
9. [Limitations](#9-limitations)
10. [Skills Demonstrated](#10-skills-demonstrated)
11. [Contact / Links](#11-contact--links)

12. # HR Analytics Dashboard — Workforce, Turnover & Retention Analysis

## 1. Project Overview

This project analyzes an organization's HR dataset (311 employees) to answer a set
of business questions around workforce composition, employee turnover, and the
factors associated with retention risk. The goal was to move beyond simple
reporting into defensible, root-cause-aware analysis — every metric and design
decision in this project can be traced back to the underlying data and explained.

**Business objective:** provide HR leadership with a clear picture of workforce
composition, quantify the scale and drivers of employee turnover, and surface
actionable, data-backed recommendations for improving retention.

## 2. Dataset & Tools

**Dataset:** HR Analytics employee dataset (`HR_Analytics.xlsx`) — 311 employee
records across 30 fields, plus six supporting lookup tables (Department, Position,
Manager, Employment Status, Performance Score, State).

**Tools used:**
- **Power Query** — data cleaning, transformation, and column enrichment
- **DAX / Power BI Data Model** — star-schema relationships, calculated columns,
  measure library
- **Power BI Desktop** — dashboard build and interactivity
- **Figma** — dashboard wireframing and layout planning prior to build, including
  the color palette and canvas proportions carried into the final Power BI report

## 3. Data Cleaning & Transformation

Key cleaning steps performed in Power Query:
- Removed redundant/duplicate-encoding columns (`GenderID`, `MarriedID`,
  `MaritalStatusID`, `FromDiversityJobFairID`) after confirming 100% redundancy
  with existing text fields
- Trimmed and cleaned inconsistent text fields (`Employee_Name`, `Sex`,
  `HispanicLatino`, department names)
- Standardized `RecruitmentSource` by merging two duplicate entries for the same
  channel ("Website" and "On-line Web application") into a single category
- Anchored all time-based calculations (Age, Tenure, hiring/termination trends)
  to a fixed **snapshot date (Feb 28, 2019)** — the latest recorded activity in
  the source data — rather than the live system date, ensuring the analysis
  doesn't silently "age" employees on refresh
- Categorized 17 raw termination reason values into 8 grouped categories via a
  mapping table, to support chart-level analysis
- Built a full calendar **Date table**, related to `DateofHire` (active) and
  `DateofTermination` (inactive, accessed via `USERELATIONSHIP`)

## 4. Data Quality Notes & Assumptions

- **Conflicting employment status records:** two employee records show
  `EmpStatusID = Active` alongside a populated termination date and reason.
  Source data was left unmodified; these records are treated as terminated in
  all headcount/attrition measures (using `Termd`), based on the stronger
  evidence of an actual termination date.
- **Duplicate manager name:** two distinct `ManagerID` values both display as
  "Brandon R. LeBlanc" — likely a data entry coincidence, not a report error.
- **Small sample sizes:** rates calculated on fewer than ~5 employees (certain
  managers, recruitment sources, tenure bands) are directional, not
  statistically conclusive, and are flagged as such on the report.
- **Performance Score distribution:** 78% of employees are coded "PIP" in the
  source performance field — an unusually high proportion for a formal
  Performance Improvement Plan designation, flagged for validation with HR
  rather than treated as a confirmed finding.

## 5. Data Model

The model follows a star-schema design: a central `Employees` table connected to
six dimension tables (Department, Position, Manager, Employment Status,
Performance Score, State) plus a calculated Date table for time intelligence.

![Model view showing star schema relationships](images/model-view.png)

## 6. Dashboard Walkthrough

### 6.1 Workforce Overview
Headcount, composition, hiring trend, and pay equity snapshot.

![Workforce Overview dashboard page](images/page1-workforce-overview.png)

### 6.2 Turnover & Retention
Attrition rate, voluntary/involuntary split, tenure-band risk, department and
manager hotspots, termination trend.

![Turnover and Retention dashboard page](images/page2-turnover-retention.png)

### 6.3 Engagement, Performance & Recruitment
Active vs. terminated comparisons, recruitment source effectiveness, salary vs.
performance analysis.

![Engagement, Performance and Recruitment dashboard page](images/page3-engagement-performance.png)

## 7. Key Findings

**Workforce composition**
Production accounts for 67.2% of total headcount, functioning as the
organization's primary labor force.

![Headcount by Department chart](images/finding-headcount-by-department.png)

**Turnover scale**
Overall attrition sits at 33.4%, with voluntary departures (28.3%) far
outweighing involuntary ones (4.5%).

![Attrition Rate KPI cards](images/finding-attrition-kpis.png)

**Early-tenure risk**
Attrition is heavily concentrated in the first 1–3 years of tenure.

![Attrition Rate by Tenure Band chart](images/finding-attrition-by-tenure-band.png)

**Department and manager hotspots**
Production (39.7%) and Software Engineering (36.4%) show the highest
departmental attrition; at the manager level, two managers with substantial
teams (21 direct reports each) show 61.9% attrition — the strongest, most
credible hotspot signal in the data.

![Attrition Rate by Department chart](images/finding-attrition-by-department.png)
![Manager attrition table](images/finding-manager-attrition-table.png)

**Recruitment source effectiveness**
Website and Employee Referral show the strongest retention profile; Google
Search shows comparable engagement scores but more than triple the attrition
rate of the stronger channels.

![Recruitment source effectiveness table](images/finding-recruitment-source-table.png)

**Performance and compensation**
Salary tracks job role far more closely than performance rating — isolating a
single role (Production Technician I) shows near-identical pay across all four
performance bands, indicating the aggregate "lowest-paid = Fully Meets" pattern
is a composition effect, not a performance-linked pay gap.

![Average Salary by Performance Rating chart](images/finding-salary-by-performance.png)

## 8. Recommendations

1. Investigate Production and Software Engineering retention — the highest
   department-level attrition in the two largest operational/technical teams.
2. Review management practices under the two managers with 61.9% attrition on
   substantial teams — the clearest credible hotspot in the data.
3. Shift recruiting investment toward Website and Employee Referral channels;
   audit the Google Search hiring pipeline given its disproportionate attrition.
4. Strengthen first 1–3 year onboarding and retention support, where departure
   risk is highest across the organization.
5. Validate Performance Score data with HR before further use, given the
   unusually high proportion of employees coded "PIP."

## 9. Limitations

- All findings are associational, not causal — the dashboard can show that a
  pattern exists, not why it exists.
- Several department, manager, and recruitment-source subgroups have small
  sample sizes; rates on these should be read directionally.
- The dataset is a static, single-point-in-time extract (through Feb 2019); no
  longitudinal or multi-year comparison beyond what's captured in hire/term
  dates is possible.
- Race-based pay comparisons are limited by extreme sample imbalance across
  categories.

## 10. Skills Demonstrated

- Power Query data cleaning, transformation, and mapping-table design
- Star-schema data modeling with active/inactive relationships and
  `USERELATIONSHIP`
- DAX measure design, including dynamic tie-safe titles, small-sample-aware
  logic, and reusable risk-color measures
- Dashboard UX: tooltip pages, drill-down hierarchies, conditional formatting,
  and layout/wireframing in Figma prior to build
- Root-cause analytical reasoning: identifying and correcting confounding
  variables (e.g., department/role composition effects on salary)

## 11. Contact / Links

- LinkedIn: [add link]
- Portfolio: [add link]
- Live Power BI report: [add link, if published]
