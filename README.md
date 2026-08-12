# 📊 HR Workforce Analytics Project

_Analyze workforce turnover, department risk, and recruitment channel effectiveness for a mid-size company using Power BI_

**+ Business question:** Which departments have the highest employee turnover, which recruitment channels actually deliver retained employees, and where should HR focus retention efforts?

**+ Domain:** Human Resources / Workforce Analytics

---

## 📑 Table of Contents
1. [📌 Background & Overview](#-background--overview)
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)
3. [🧠 Design Thinking Process](#-design-thinking-process)
4. [⚒️ Main Process](#️-main-process)
5. [📊 Key Insights & Visualizations](#-key-insights--visualizations)
6. [🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)

---

## 📌 Background & Overview

### 📖 What is this project about?

HR data at most companies lives in scattered spreadsheets - employee records, positions, departments, managers, and performance scores are all tracked separately, making it hard to see the full picture. This project consolidates that data into a single Power BI dashboard so HR and management can monitor workforce health at a glance.

The objective is to:\
✔️ Quantify overall and department-level employee turnover\
✔️ Understand who the workforce is (gender, tenure, department, performance)\
✔️ Evaluate which recruitment sources actually bring in retained employees\
✔️ Give managers a searchable, filterable view of their own team

### 👤 Who is this project for?

✔️ HR business partners & workforce planning teams\
✔️ Department managers reviewing their own headcount and turnover\
✔️ Recruiting teams evaluating channel ROI\
✔️ Data/BI analysts building HR analytics portfolios

---

## 📂 Dataset Description & Data Structure

### 📌 Data Source
- **Source:** Self-compiled/internal HR workbook (`HRDataset.xlsx`)
- **Format:** Excel workbook, 7 related sheets (relational, star-schema style - not a single flat table)
- **Size:** 298 active/former employee records as the fact table, plus 5 lookup tables and 1 staging table for new hires

### 📊 Data Structure & Relationships

#### 1️⃣ Table Schema & Data Snapshot

**Table 1: `Employee` (fact table - 298 rows × 20 columns)**

| Column Name | Data Type | Description |
|---|---|---|
| Employee | Text | `EmployeeID-Name` combined (e.g. `10026-Adinolfi, Wilson K`) |
| ManagerID | Int | FK → `Manager` table |
| PositionID | Int | FK → `Position` table |
| MaritalStatusID | Int | FK → `MaritalStatus` table |
| PerformanceScoreID | Int | FK → `Performance` table |
| Location | Text | State abbreviation |
| Birthday | Date | Employee date of birth |
| Gender | Text | M / F |
| CitizenDesc | Text | US Citizen / Eligible NonCitizen / Non-Citizen |
| RaceDesc | Text | Race/ethnicity |
| DateofHire | Date | Hire date |
| RecruitmentSource | Text | Channel employee was hired through (Indeed, LinkedIn, etc.) |
| EmploymentStatus | Text | Active / Voluntarily Terminated / Terminated for Cause |
| Termination | Int (0/1) | Termination flag |
| DateofTermination | Date | Blank if still active |
| TerminationReason | Text | Reason for leaving |
| Salary | Numeric | Annual salary |
| EngagementScore | Numeric | Engagement survey score |
| SatisfactionScore | Numeric | Employee satisfaction score |
| AbsenceDays | Int | Days absent |

<details>
<summary>Click to toggle lookup/dimension tables</summary>

**`Position`** (30 rows) - `PositionID`, `Position`, `DeptID`
**`Department`** (6 rows) - `DeptID`, `Department` (Admin Offices, Executive Office, IT/IS, Software Engineering, Production, Sales)
**`MaritalStatus`** (5 rows) - `MaritalStatusID`, `MaritalDesc`
**`Performance`** (4 rows) - `PerfScoreID`, `PerformanceScore` (Not To Keep, Needs Improvement, Fully Meets, Exceeds)
**`Manager`** (23 rows) - `ManagerID`, `ManagerName`
**`EmployeeNew`** (13 rows) - a staging table of newly onboarded employees not yet merged into the main `Employee` table

</details>

#### 2️⃣ Data Relationships

This is a **star-schema model**: `Employee` is the fact table, joined to four dimension tables (`Position`, `MaritalStatus`, `Performance`, `Manager`) via their ID columns. `Position` in turn joins to `Department` via `DeptID`, giving a two-hop path from `Employee` to `Department` - this is what allows department-level turnover and headcount to be calculated correctly in Power BI.

---

## 🧠 Design Thinking Process

1️⃣ **Empathize** - HR and department managers could not answer basic questions ("which department is bleeding people?", "is LinkedIn worth the spend?") without manually cross-referencing six separate sheets.

2️⃣ **Define** - The core need: a single, filterable view that surfaces turnover risk by department and evaluates recruitment channels, without requiring anyone to touch raw data.

3️⃣ **Ideate** - Shortlisted the KPIs that matter most to HR stakeholders: overall & department turnover rate, headcount and gender mix, average tenure, engagement/satisfaction, and recruitment source performance.

4️⃣ **Prototype & Review** - Modeled the six tables into a star schema in Power BI, then split the report into two pages: a high-level **Executive Summary** for leadership and a detailed, filterable **Workforce Database Overview** for managers to drill into individual employees.

---

## ⚒️ Main Process

**1️⃣ Data Cleaning & Preprocessing (Power Query)**
- Related the `Employee` fact table to five lookup tables (`Position`, `Department`, `MaritalStatus`, `Performance`, `Manager`) using ID-based relationships instead of duplicating text fields.
- Standardized inconsistent `Gender` values (raw data mixed `"M "` with a trailing space against `"F"`).
- Fixed mixed date formats in `DateofHire` (stored as true dates in some rows, as text strings like `"3/30/2015"` in others).
- Handled blank `DateofTermination` for the 194 still-active employees so they wouldn't be miscounted as terminated.

**2️⃣ Exploratory Data Analysis**
- Profiled 298 employee records: 104 terminations (~35% turnover), headcount heavily concentrated in Production (203 of 298 employees), and a performance-score distribution skewed toward "Fully Meets" (78%).

**3️⃣ DAX Measures**
- Built calculated measures for Turnover Rate, Headcount, Average Tenure, Average Engagement/Satisfaction Score, and Recruitment Source counts, driving all visuals dynamically off the star-schema model rather than static pivot tables.

**4️⃣ Power BI Visualization**
- Built two report pages: an **Executive Summary** with org-wide KPI cards and breakdowns, and a **Workforce Database Overview** with slicers for Department, Performance Score, Manager Name, and Employee Name.

---

## 📊 Key Insights & Visualizations

### 🔍 Dashboard Preview

#### 1️⃣ Executive Summary
👉🏻 [![Executive Summary](https://github.com/quanghung20041/HR-Workforce-Dashboard/raw/main/HR_Workforce_Dashboard_images/executive-summary.png)](https://github.com/quanghung20041/HR-Workforce-Dashboard/blob/main/HR_Workforce_Dashboard_images/executive-summary.png)

📌 **Analysis 1:**
- **Observation:** Overall turnover sits at **34.9%**. Production is both the largest department (203 of 298 employees) and the highest-turnover department at **40.89%** - meaning it drives the majority of the company's total attrition. Women make up **56.71%** of the workforce. Indeed and LinkedIn are the two largest recruitment sources by volume.
- **Recommendation:** Prioritize a retention review for Production specifically, since it has an outsized effect on the company-wide turnover number. Audit whether Indeed and LinkedIn hires actually stay longer than other channels before allocating more recruiting budget to them.

#### 2️⃣ Workforce Database Overview
👉🏻 [![Workforce Database Overview](https://github.com/quanghung20041/HR-Workforce-Dashboard/raw/main/HR_Workforce_Dashboard_images/workforce-database.png)](https://github.com/quanghung20041/HR-Workforce-Dashboard/blob/main/HR_Workforce_Dashboard_images/workforce-database.png)

📌 **Analysis 2:**
- **Observation:** This page exposes employee-level detail with slicers for Department, Performance Score, Manager Name, and Employee Name, so a manager can filter down to their own team and see individual tenure, engagement, and satisfaction rather than only aggregate numbers.
- **Recommendation:** Use this page in 1:1 manager reviews to flag employees who combine low engagement/satisfaction scores with long absence counts - these are the earliest signals of voluntary turnover risk.

---

## 🔎 Final Conclusion & Recommendations

📌 **Key Takeaways:**\
✔️ Turnover is not evenly spread - it is concentrated in one department (Production), so a company-wide retention policy is less effective than a targeted one.\
✔️ Recruitment source volume (Indeed, LinkedIn) doesn't automatically mean recruitment source quality - retention by source should be tracked as a follow-up metric.\
✔️ Manager-level visibility (via the Workforce Database page) turns this from a reporting tool into an actionable one, since department-level averages alone can hide at-risk individuals.

---

## 📂 Files Included
- `HR_Workforce_Dashboard.pbix` - Power BI dashboard file
- `HR_Workforce_Dashboard.pdf` - Exported/static version of the dashboard
- `HRDataset.xlsx` - Dataset source
- `HR_Workforce_Dashboard_images/` - Dashboard screenshots
- `README.md` - Project documentation

## 🛠 Tools & Technologies
- Power BI
- Power Query
- DAX
- Excel

## 🚀 Project Features
- Star-schema data model (fact + 5 dimension tables)
- Interactive filtering by Department, Performance Score, Manager Name, Employee Name
- Org-wide KPI monitoring (turnover, headcount, gender mix, tenure)
- Recruitment source analysis
- Employee-level drill-down for manager use

## 👨‍💻 Author
Nguyen Quang Hung
