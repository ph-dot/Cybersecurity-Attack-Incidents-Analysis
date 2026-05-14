# Cybersecurity Attack Incident Analysis
**Analytical insights of 100,000 cybersecurity attack incidents.**

## 1. Project Overview
[cite_start]This repository documents the data analytics techniques used to preprocess, analyze, and interpret 100,000 cybersecurity attack records[cite: 7]. [cite_start]The project aims to solve real-world security challenges by transforming raw data into actionable insights using Power BI and the Pyramid Framework[cite: 3, 500, 477].

## 2. Data Preprocessing (CLEAN Framework)
[cite_start]We followed the **CLEAN Framework** to ensure high-quality data for analysis[cite: 15, 16].

### C - Conceptualize
* [cite_start]**Row Representation**: Each row represents a unique cybersecurity attack event, from initiation to mitigation[cite: 19].
* [cite_start]**Key Metrics**: `data_compromised_GB`, `attack_severity`, and `response_time_min`[cite: 21, 22].
* [cite_start]**Key Dimensions**: `attack_type`, `outcome`, `target_system`, `date_stamp`, `security_tools_used`, `user_role`, `location`, and `industry` [cite: 23-31].

### L - Locate Solvable Problems
[cite_start]An issues log was created to track and resolve data inconsistencies[cite: 36].
* [cite_start]**Data Quality**: Checked for null values, empty fields, and inconsistent formats using Power Query Editor[cite: 37, 40].
* [cite_start]**Validation**: All columns showed 100% validity with 0% errors or empty fields[cite: 41].
* [cite_start]**Integrity Check**: Verified that `attacker_ip` and `target_ip` were distinct for every record using a DAX comparison formula [cite: 188-192].

### E - Evaluate Unsolvable Issues
* [cite_start]The dataset contained no unsolvable outliers or business logic conflicts that required exclusion[cite: 378].

### A - Augment the Data
* [cite_start]**Time Grains**: Separated the original timestamp into distinct `Date` and `Time` columns to allow for deeper temporal slicing[cite: 383, 384].
* [cite_start]**New Measures**: Developed a **Failure Rate** measure to calculate the percentage of successful vs. failed attack outcomes[cite: 430, 431].

### N - Note and Document
* [cite_start]All resolutions, including the removal of duplicate checks (which resulted in 0 duplicates found), have been documented in the project logs[cite: 388, 402].

---

## 3. Exploratory Data Analysis (EDA) & Metrics
[cite_start]We applied the **Pyramid Framework** to categorize our findings into three levels of altitude[cite: 477, 478].

### The Metrics Pyramid
1.  [cite_start]**OKRs (Objectives)**: Minimize impact and harm from cyberattacks[cite: 491, 492].
2.  **KPIs (Key Performance Indicators)**: 
    * [cite_start]**Failure Rate**: 49.97%[cite: 462].
    * [cite_start]**Total Data Compromised**: 5.01 Million GB[cite: 417].
    * [cite_start]**Average Response Time**: 90.45 Minutes[cite: 419].
3.  [cite_start]**Metrics**: Individual tracking of attack duration, severity, and response times across 100,000 incidents [cite: 479-484].

### Key Visualizations
* [cite_start]**Trends**: Average response time analyzed by Year and Month[cite: 442].
* [cite_start]**Impact**: Average data compromised categorized by `attack_type`[cite: 446].
* [cite_start]**Tool Effectiveness**: Average attack severity vs. `security_tools_used`[cite: 464].

---

## 4. Technical Requirements & Deadlines
* [cite_start]**Tools**: Power BI (Dashboarding), GitHub (Documentation)[cite: 500, 514].
* [cite_start]**Schema**: Implementation of Star or Snowflake data modeling[cite: 495].
* [cite_start]**Final Submission**: May 15, 2026[cite: 515].

## 5. Group Members
* [cite_start]**Data Collection/Preprocessing**: Phibs [cite: 11]
* [cite_start]**EDA/Pyramid Framework**: Phibs [cite: 407]
* [cite_start]**Data Modeling/Analytics**: Ivan & Erwin [cite: 493]
* [cite_start]**Visualization/Dashboard**: Mark [cite: 498]
* [cite_start]**Insights/Recommendations**: Ivan & Erwin [cite: 507]