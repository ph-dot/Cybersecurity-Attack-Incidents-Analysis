# Cybersecurity Incident Analysis Dashboard
**An End-to-End Data Analytics Project for 100,000 Attack Records**

## 1. Project Overview
This project applies advanced data analytics to a dataset of 100,000 cybersecurity incidents. The goal is to solve real-world problems by transforming raw security logs into actionable insights through rigorous preprocessing, Star Schema modeling, and interactive visualization.

* **Dataset Profile**: 100,000 records tracking attack types, target systems, compromised data, and mitigation outcomes. It contains the following columns: `attack_type`, `target_system`, `outcome`, `timestamp`, `attacker_ip`, `target_ip`, `data_compromised_GB`, `attack_duration_min`, `security_tools_used`, `user_role`, `location`, `attack_severity`, `industry`, and `response_time_min`
![Alt Text](/images/raw_data.png) 
* **Core Objective**: To minimize impact and harm from cyberattacks by identifying high-risk patterns.

## 2. Data Preprocessing (CLEAN Framework)
We utilized the **CLEAN Framework** to ensure a clean enough valid dataset for analysis.

### C - Conceptualize
* **Grain**: Each row represents a unique cybersecurity attack event from initiation to mitigation.
* **Key Metrics**: `data_compromised_GB`, `attack_severity`, and `response_time_min`.
* **Dimensions**: `attack_type`, `outcome`, `target_system`, `industry`, and `location`.

### L - Locate Solvable Problems
| Table | Column | Issue | Row Count | Solvable? | Resolution |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Cybersecurity | All Columns | Inconsistent Format | 100,000 | Y | Verified via header dropdowns; none found. |
| Cybersecurity | All Columns | Null/Empty Values | 100,000 | Y | Checked Power Query Quality; 100% valid |
| Cybersecurity | All Columns | Duplicate Rows | 100,000 | Y | Used 'Keep Duplicates' tool; resulted in 0 values. |
| Cybersecurity | IP Columns | Identical Source/Target | 100,000 | Y | DAX formula confirmed 0 matches; column deleted. |

### E - Evaluate Unsolvable Issues
* The dataset contained no unsolvable outliers or missing values that required exclusion.

### A - Augment the Data
* **Time Slicing**: Separated `timestamp` into discrete `Date` and `Time` columns for deeper temporal analysis.
* **New Measures**: Developed the following measures:  
    - **Critical Incident %** = DIVIDE( [Critical Incidents], [Total Incidents] )
    - **Critical Incidents** = 
VAR CurrentSeverity = SELECTEDVALUE('attack_dim'[attack_severity])
RETURN
IF(
    ISBLANK(CurrentSeverity),
    CALCULATE([Total Incidents], 'attack_dim'[attack_severity] >= 8),
    IF(CurrentSeverity >= 8, [Total Incidents], 0)
)
    - **Failure Rate** = DIVIDE(COUNTROWS(FILTER('cybersecurity_incident_fact', 'cybersecurity_incident_fact'[outcome] = "Failure")), COUNTROWS('cybersecurity_incident_fact'))
    - **High Data Breach Incidents** = 
CALCULATE(
    [Total Incidents],
    'cybersecurity_incident_fact'[data_compromised_GB] > 50
)
    - **High Severity Incident %** = 
DIVIDE(
    [High Severity Incidents],
    [Total Incidents]
)
    - **High Severity Incidents** = 
CALCULATE(
    [Total Incidents],
    'attack_dim'[attack_severity] >= 5
)
    - **Mitigation Efficiency** = 
DIVIDE(
    CALCULATE([Total Incidents], 'cybersecurity_incident_fact'[outcome] = "Failure"), 
    [Total Incidents]
)
    - **Success Rate** = DIVIDE(COUNTROWS(FILTER('cybersecurity_incident_fact', 'cybersecurity_incident_fact'[outcome] = "Success")),COUNTROWS('cybersecurity_incident_fact'))
    - **System Vulnerability Index** = 
AVERAGEX(
    VALUES('target_system_dim'[target_system]),
    [Failure Rate]
)
    - **Total Incidents** = COUNTROWS('cybersecurity_incident_fact')
    - **Unique Attackers** = DISTINCTCOUNT('cybersecurity_incident_fact'[attacker_ip])
    - **Zero-Day %** = 
DIVIDE(
    CALCULATE([Total Incidents], 'attack_dim'[attack_type] = "Zero-Day Exploit"),
    [Total Incidents]
)

---

## 3. Data Modeling (Star Schema)
We implemented a **Star Schema** to optimize query performance and reporting.
![Alt Text](/images/star_schema.png) 
* **Fact Table**: `cybersecurity_incident_fact` (contains metrics and foreign keys).
![Alt Text](/images/fact_table.png) 

* **Dimension Tables**: `attack_dim`, `target_system_dim`, `security_dim`, `user_role_dim`, `location_dim`, and `industry_dim`.
    - **attack_dim**\
    ![Alt Text](/images/attack_dim.png)
    - **target_system_dim**\
    ![Alt Text](/images/target_system_table.png)
    - **security_dim**\
    ![Alt Text](/images/security_table.png)
    - **user_role_dim**\
    ![Alt Text](/images/user_table.png)
    - **location_dim**\
    ![Alt Text](/images/location_table.png)
    - **industry_dim**\
    ![Alt Text](/images/industry_table.png)

**Descriptive Analysis** 
* This dataset contains 100,000 records of cybersecurity incidents. It is composed of different attacks, target systems, compromised data, attack durations, mitigation methods, etc. These columns will give us an insightful view regarding cybersecurity occurrences. Descriptive analysis is used here to summarize, organize, interpret, and present data. 
The following are some of the findings this dataset has shown:

    - **Success and Failure Rates of Hackings per Year** 

    ![Alt Text](/images/success_failure_rates.png)

    This table presents the success and failure rates of hackers per year. This shows that the percentages are very close, but the highest success rate was in 2023, while the lowest was in 2024. In addition, the failure rate means a win for the system because it was not hacked.

    - **Total Number of Compromised Data in GB** 

    ![Alt Text](/images/compromised_data.png)

    This horizontal bar chart shows the sum of the compromised number of data in GB per industry. It presents that every industry comprises over 600k GB of data in the span of one year (September 2023 - September 2024).

    - **Success Rate of Hacking per Target System**

    ![Alt Text](/images/hacking_sucess.png)

    This table shows the following system that the hacker targets. This also indicates the success rate, wherein data are compromised per target system. In this, the database has the highest success rate at 50.71%, while the web server is the lowest at 49.23%.


---

## 4. Visualization & Dashboard (DASH Framework)
The dashboard was developed in **Power BI** following the **DASH Framework**.

### Wireframe
![Alt Text](/images/wireframe.png)

### The Pyramid Framework (Metrics & KPIs)
* **Total Incidents**: 100K.
* **Success Rate**: 50.03% (Attacker wins).
* **Failure Rate**: 49.97% (System defense wins).
* **Avg Attack Time**: 151.07 Minutes.
* **Avg Response Time**: 90.45 Minutes.
* **Critical Incidents**: 30K.
![Alt Text](/images/kpis.png)
The following KPIs shows the Total Incidents, Critical Incidents, Average of Attack Duration in Min, Average of Response Time in Min, Success Rate, and Failure Rate of Cybersecurity Incidents. 

### Filters
* **Attack Type**
* **Year**
* **Attack Severity**
* **Industry**
* **Location**
![Alt Text](/images/sliders.png)

### Charts
* **Count of User Roles**
![Alt Text](/images/count_user_roles.png)
This donut chart displays the count of user roles involved in assessing cybersecurity incidents.

* **Count of User Roles**
![Alt Text](/images/total_incidents.png)
This line chart shows the total cybersecurity incidents that occurred day by day. This also presents that cybersecurity incidents averagely occur nearly 300 times per day as shown in the chart.

* **Total Incidents by Security Tools used and Mitigation Method**
![Alt Text](/images/incidents_tools.png)
This clustered column chart displays the total incidents on what security tools and method of mitigation were used in that incident.

* **Total Incidents by Attack Type and Target System**
![Alt Text](/images/attack_type_system.png)
This stacked bar chart displays the total incidents by which attack type occurred and what system was targeted by the attack.


---

## 5. Insights & Recommendations

### Insights

* **Insight 1**: In the year 2023, there were approximately 32K total incidents, while in 2024, the total incidents doubled, with more or less 64K incident records. Despite this, the success and failure rates are still ~50%.

* **Insight 2**: Out of all the incidents, the victims are somehow equally distributed at ~25%. However, critical incidents with approximately 8k are among admins and employees. In contrast, approximately 7K critical incidents were recorded among external users and contractors.

* **Insight 3**: Hackers mostly do brute force, DDoS, and zero-day exploits; all three were recorded in approximately 13K attacks. While attackers have the highest failure rate of hacking at 50.42% when the system is using security tools like Web Application Firewall (WAF).

### Recommendations
* **Recommendation 1**:  Use SOAR (Security Orchestration, Automation, and Response) 

* **Recommendation 2**:  Implement Zero Trust Security

* **Recommendation 3**:  Maximize WAF Coverage and Virtual Patching.

### Results in the Real-World Context

In the real world, seeing this many attacks shows that manual protection is no longer enough to stay safe. Since any user, from an admin, employee, external user, or contractor, can be a target, it is important to stop assuming every login is safe and instead strictly limit what each person can see or change. Using automation to handle the thousands of daily threats and a web firewall to block specialized hacking tricks allows the system to defend itself instantly. This creates a safer environment where a single stolen password doesn't lead to a total system shutdown or the loss of private records. 

---
