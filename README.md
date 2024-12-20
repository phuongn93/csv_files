﻿# Introduction
  Take a look at data analyst jobs at the entry-level market in 2023. This project explores top paying jobs, top paying skills, and where high demand meets high salary and predict some trends changing in 2024-2025.
   
   SQL queries? Check them out here [Project_Sql_Folder](/Project_SQL/)

# Dataset Background
  Data hails from Luke Barousse's course [SQL Course](https://lukebarousse.com/sql). It's packed with insights on job titles, salaries, locations, and essential skills.

  ### The questions I wanted to answer through my SQL queries were:

  1. What are the top-paying data analyst jobs at the entry-level?
  2. What companies did they hire data analyst at the entry level? Which companies did they post the most jobs?
  3. What are the most in-demand skills for data analysts?
  4. What are the top skills based on salary for remoted data analyst?
  5. What are the most optimal skills to learn (aka it’s in high demand and a high-paying skill) for a remoted data analyst?
  
# Tools I Used

  -**Structured Query Language ( SQL)** : domain-specific language used to manage and manipulate data stored in relational databases. It allows me to convey data into some critical insights about what I am looking for.

  -**PostgreSQL** : open-source object-relational database management systsem that extends the SQL language with numerous features to safely store and scale complex data workloads, in this case handling the job posting data.

  -**Visual Studio Code (VS Code)** : an integrated development environment developed by Microsoft to provide a streamlined and efficient coding experience, executing my SQL queries.

  -**Git** : a distributed version control system that allows me to track changes in my codebase over time 

  _**GitHub** : a platform that hosts code repositories and utilizes Git for version control. It acts as a hub where I can collaborate on software projects, share files, and manage different versions of my code.
  
# The Anlysis
  Each query for this project investigates specific aspects of remoted data analyst job market in Texas. Here is how I use SQL to query data to answer each question:
  ### 1.Top Paying Data Analyst Jobs at the Entry Level

Question: What are the top-paying data analyst jobs?
- Identify the data analyst roles that are available with no degree needed along with the maximum, minimum and average salary.
- Top locations have companies hiring data analyst at the entry-level.

```sql

Select 
 job_id, company_id, job_title_short, job_location,search_location, job_work_from_home as remoted_job, job_no_degree_mention, salary_year_avg, job_posted_date
from job_postings_fact
where
 job_title_short = 'Data Analyst' and job_no_degree_mention = 'true' and salary_year_avg is not null
order by salary_year_avg DESC;

What is the max, min, and avg of salary_year_avg of data analyst jobs?

Select
 max(salary_year_avg) as maximum_salary,
 min(salary_year_avg) as minimum_salary,
 round(avg(salary_year_avg),2) as Avg_Salary
From (
select
 job_id, company_id, job_title_short, job_location,search_location, job_work_from_home as remoted_job, job_no_degree_mention, salary_year_avg, job_posted_date
from job_postings_fact
where
 job_title_short = 'Data Analyst' and job_no_degree_mention = 'true' and salary_year_avg is not null
order by salary_year_avg DESC)

Which job location did they hire remoted and non-remoted data analysts at the entry-level?/*

Select 
 job_location,
 count (job_id),
 round(avg (salary_year_avg),2) as Avg_Salary
From job_postings_fact
Where
 job_title_short = 'Data Analyst' and job_no_degree_mention = 'true' and salary_year_avg is not null
 group by job_location
 order by count DESC
 limit 10;

 ```

Here's the breakdown of the top data analyst jobs:

-**General Salary Statistics** : as a data analyst at entry level has maximum salary as $650,000, mininum salary as $26,444 average salary as $92,650.92

-**Degree mention** : None of the jobs explicitly mention the need for a degree, indicating companies may prioritize skills and experience over formal education.

-**Top Locations**: The prevalence of postings offer more opportunites for the entry level data analyst not only in  major metro areas such as New York, California and Texas in United States but also working remotely.

### 2. Companies hire data analyst at entry-level?
Question: what companies did they hire data analyst at the entry-level?
- Identify the companies's core culture, missions whether they align with job seeker's.

```sql
What companies did they hire data analyst at entry-level?

select
  job_postings_fact.job_id,
  company_dim.company_id,
  job_postings_fact.job_title_short as job_title,
  job_postings_fact.job_location as location,
  job_postings_fact.search_location as State_USA, 
  job_postings_fact.job_work_from_home as remoted_job, 
  job_postings_fact.job_no_degree_mention as no_degree, 
  job_postings_fact.salary_year_avg as salary_year_avg, 
  job_postings_fact.job_posted_date, company_dim.name,company_dim.link

from job_postings_fact

full outer join company_dim on job_postings_fact.company_id = company_dim.company_id

where
 job_title_short = 'Data Analyst' and job_no_degree_mention = 'true' and salary_year_avg is not null

order by salary_year_avg;

How many entry-level jobs are they remoted and onsite?
select
 case 
  when job_location = 'Anywhere' then 'remoted_job'
  Else 'Onsite'
 End as location_category,
 count (job_id)
 from job_postings_fact
 where
 job_title_short = 'Data Analyst' and job_no_degree_mention = 'true' and salary_year_avg is not null
group by location_category;

| Location Category | Count |
|--------------------|-------|
| Onsite            | 1122  |
| Remote            | 114   |


How many not entry-level jobs are they remoted and onsite?

select
 case 
  when job_location = 'Anywhere' then 'remoted_job'
  Else 'Onsite'
 End as location_category,
 count (job_id)
 from job_postings_fact
 where
 job_title_short = 'Data Analyst' and job_no_degree_mention = 'false' and salary_year_avg is not null
group by location_category;

| Location Category | Count |
|--------------------|-------|
| Onsite            | 3737 |
| Remote            | 490   |


Which companies did they post the most jobs in 2023?

select
  count(company_dim.company_id) as company_count, 
  company_dim.name

from job_postings_fact

full outer join company_dim on job_postings_fact.company_id = company_dim.company_id
where
job_title_short = 'Data Analyst' and job_no_degree_mention = 'true' and salary_year_avg is not null
group by company_dim.name
order by company_count DESC
Limit 10;

| Rank | Company Count | Company Name               |
|------|---------------|----------------------------|
| 1    | 67            | Robert Half               |
| 2    | 54            | Acadia Technologies, Inc. |
| 3    | 33            | Insight Global            |
| 4    | 17            | CyberCoders               |
| 5    | 16            | Publicis Groupe           |
| 6    | 14            | Jobot                     |
| 7    | 11            | Upen Group Inc            |
| 8    | 10            | Worldgate LLC             |
| 9    | 8             | Motion Recruitment        |
| 10   | 8             | Bosch Group               |

```
Here's the breakdown of the top data analyst jobs:

-**Location_category** : The result indicate an emphasis on in-office collboration for data analys jobs ( more than 90% of the job requires on-site)

-**Companies** : Staffing companies like Robert Half and Insight Global are known for recruiting across experience levels, including entry-level roles, for various clients. Technology such as Bosch Group and Publicis Groupe may have structured graduate or internship programs that include entry-level analyst roles.

### 3. Top in-demand skills for Entry-Level Data Analyst

Question: What are the most in-demand skills for data analysts?
- Identify the top in-demand skills for a data analyst.
- Focus on all job postings.
- Why? Retrieves the top skills with the highest demand in the job market, providing insights into the most valuable skills for job seekers

```sql

select
  skills_dim.skills,
  COUNT(skills_job_dim.job_id) AS demand_count

from job_postings_fact

full outer join skills_job_dim on job_postings_fact.job_id= skills_job_dim.job_id
full outer join skills_dim on skills_job_dim.skill_id = skills_dim.skill_id

where
 job_postings_fact.job_title_short = 'Data Analyst'
 and job_no_degree_mention = 'true'
 GROUP BY
  skills_dim.skills
Order by demand_count DESC
Limit 25;

```

| Rank | Skill        | Demand Count |
|------|--------------|--------------|
| 1    | SQL          | 31,980       |
| 2    | Excel        | 20,549       |
| 3    | Python       | 17,327       |
| 4    | Tableau      | 14,103       |
| 5    | Power BI     | 13,450       |
| 6    | R            | 7,348        |
| 7    | SAS          | 6,146        |
| 8    | Azure        | 4,143        |
| 9    | SAP          | 3,643        |
| 10   | AWS          | 3,235        |
| 11   | Oracle       | 3,197        |
| 12   | Word         | 3,174        |
| 13   | PowerPoint   | 3,070        |
| 14   | Go           | 2,938        |
| 15   | SQL Server   | 2,865        |
| 16   | Looker       | 2,572        |
| 17   | Snowflake    | 2,350        |
| 18   | Flow         | 2,211        |
| 19   | VBA          | 1,909        |
| 20   | Qlik         | 1,855        |
| 21   | Jira         | 1,783        |
| 22   | Spark        | 1,626        |
| 23   | BigQuery     | 1,540        |
| 24   | DAX          | 1,345        |
| 25   | Java         | 1,324        |

Here's the breakdown of the top data analyst jobs:

- **Most Commonly Required Skills**: SQL found in almost every job description, indicating it's a foundational requirement for Data Analyst roles. Python: Frequently paired with SQL, showcasing its importance in advanced data analysis.Tableau and Power BI: Visualization tools like these are in high demand across multiple jobs.

### 4. Top Skills Based On Salary
What are the top skills based on salary?
- Look at the average salary associated with each skill for Data Analyst positions.
- Focuses on roles with specified salaries, regardless of location.
- Why? It reveals how different skills impact salary levels for Data Analysts and helps identify the most financially rewarding skills to acquire or improve

```sql
select
  skills_dim.skills,
  COUNT(skills_job_dim.job_id) AS demand_count,
  Round(AVG (job_postings_Fact.salary_year_avg),2) as Avg_salary

from job_postings_fact

full outer join skills_job_dim on job_postings_fact.job_id= skills_job_dim.job_id
full outer join skills_dim on skills_job_dim.skill_id = skills_dim.skill_id

where
 job_postings_fact.job_title_short = 'Data Analyst'
 AND job_no_degree_mention = 'true'
 And salary_year_avg is not null

GROUP BY
  skills_dim.skills

Order by avg_salary DESC

Limit 25;
```

| Rank | Skill        | Demand Count | Average Salary ($) |
|------|--------------|--------------|---------------------|
| 1    | svn          | 1            | 400,000.00         |
| 2    | golang       | 1            | 165,000.00         |
| 3    | unify        | 1            | 163,782.00         |
| 4    | cassandra    | 2            | 163,042.25         |
| 5    | kafka        | 14           | 154,297.61         |
| 6    | mongo        | 3            | 140,000.00         |
| 7    | twilio       | 2            | 138,500.00         |
| 8    | pytorch      | 1            | 137,500.00         |
| 9    | tensorflow   | 1            | 137,500.00         |
| 10   | mongodb      | 14           | 137,060.79         |
| 11   | terraform    | 2            | 136,891.00         |
| 12   | scala        | 8            | 135,972.75         |
| 13   | linux        | 18           | 131,666.67         |
| 14   | dynamodb     | 3            | 130,000.00         |
| 15   | no-sql       | 2            | 129,391.00         |
| 16   | postgresql   | 8            | 127,010.56         |
| 17   | gitlab       | 2            | 126,441.00         |
| 18   | jenkins      | 3            | 126,260.67         |
| 19   | hadoop       | 21           | 126,024.79         |
| 20   | aurora       | 3            | 124,423.17         |
| 21   | git          | 15           | 124,262.66         |
| 22   | gcp          | 21           | 124,254.76         |
| 23   | angular      | 6            | 123,177.33         |
| 24   | c++          | 8            | 121,875.00         |
| 25   | databricks   | 25           | 120,918.70         |

Here's the breakdown of the top data analyst jobs:

-**Top Skills Base on Salary** : The skills represent a mix of cutting-edge technologies in Big Data, AI/ML, DevOps, and Cloud Computing. The trends highlight growing reliance on distributed systems, automation, and AI-driven frameworks. These skills cater to the evolving needs of industries transitioning to modern, data-centric ecosystems.

### 5. Most Optimal Skills

What are the most optimal skills to learn (aka it’s in high demand and a high-paying skill) for a data analyst?

- Identify skills in high demand and associated with high average salaries for Data Analyst roles
- Why? Targets skills that offer job security (high demand) and financial benefits (high salaries), offering strategic insights for career development in data analysis

```sql 
select
  skills_dim.skills,
  COUNT(skills_job_dim.job_id) AS demand_count,
  Round(AVG (job_postings_Fact.salary_year_avg),2) as Avg_salary

from job_postings_fact

full outer join skills_job_dim on job_postings_fact.job_id= skills_job_dim.job_id
full outer join skills_dim on skills_job_dim.skill_id = skills_dim.skill_id

where
 job_postings_fact.job_title_short = 'Data Analyst'
 and job_no_degree_mention = 'true'
 And salary_year_avg is not null

GROUP BY
  skills_dim.skills

Order by demand_count DESC

Limit 25;
```

| Rank | Skill        | Demand Count | Average Salary ($) |
|------|--------------|--------------|---------------------|
| 1    | SQL          | 715          | 96,627.78          |
| 2    | Excel        | 375          | 83,841.73          |
| 3    | Python       | 355          | 100,595.56         |
| 4    | Tableau      | 305          | 98,590.23          |
| 5    | Power BI     | 223          | 91,114.45          |
| 6    | R            | 163          | 99,120.07          |
| 7    | SAS          | 102          | 93,999.93          |
| 8    | Word         | 87           | 83,101.03          |
| 9    | SQL Server   | 87           | 96,533.41          |
| 10   | Azure        | 83           | 105,950.24         |
| 11   | Looker       | 74           | 108,169.55         |
| 12   | PowerPoint   | 73           | 85,877.85          |
| 13   | Go           | 70           | 92,171.26          |
| 14   | AWS          | 68           | 106,739.08         |
| 15   | Flow         | 65           | 95,852.81          |
| 16   | Oracle       | 60           | 104,402.53         |
| 17   | Snowflake    | 55           | 113,361.91         |
| 18   | Sheets       | 49           | 86,846.51          |
| 19   | Spark        | 44           | 115,421.72         |
| 20   | Outlook      | 39           | 87,097.50          |
| 21   | MATLAB       | 31           | 80,059.68          |
| 22   | Jira         | 30           | 105,415.13         |
| 23   | Redshift     | 29           | 108,787.35         |
| 24   | NoSQL        | 28           | 101,951.91         |
| 25   | C            | 25           | 109,154.38         |

Here's the breakdown of the top data analyst jobs:

-**Most Optimal Skills** : SQL (715), Excel (375), Python (355): Core tools for data analysis and reporting. SQL and Python command strong salaries ($96,000–$100,000), highlighting their importance in analytics workflows. Tableau (305), Power BI (223): Visualization tools, both in high demand with competitive salaries ($91,000–$98,000).

-**Trend** : The data highlights SQL, Python, and Tableau as foundational skills in data roles, while advanced tools like big data and cloud computing (Snowflake, Spark), and cloud technologies (e.g., AWS, Azure) offer premium salaries. The trends underscore the growing value of cloud-based and big data technologies, alongside steady demand for data visualization and analysis tools.

# What I Learned Technically 

-**SQL**: Proficient in extracting data with queries using conditions (WHERE), sorting (ORDER BY), and aggregating grouped data (GROUP BY) to identify trends (AVG, MIN, MAX, Count). Skilled in table manipulation with joins, and breaking down complex tasks using subqueries.

-**PostgreSQL**: Experienced with pgAdmin for interacting with PostgreSQL databases, including uploading CSV files for analysis.

-**VS Code**: Basic proficiency in using IDE features like Git integration for version control and extensions (e.g., PostgreSQL, SQL Tools, CSV Rainbow) for customization.

-**Git & GitHub**: Familiar with setup, integration with VS Code, and managing source control, including resolving configuration issues for Git username and email.

# Conclusions From Data

-**Salary**: Entry-level data analysts earn between $26,444 and $650,000, with an average salary of $92,650.92.

-**Education**: Entry-level data analyst job postings do not require a degree, highlighting a focus on skills and experience over formal education.

-**Locations**: Opportunities are abundant in metro areas like New York, California, and Texas, as well as for remote roles. However, over 90% of positions emphasize on-site work.

-**Employers**: Staffing firms like Robert Half and Insight Global actively recruit entry-level analysts, while companies like Bosch Group and Publicis Groupe offer structured programs for early-career roles.

-**Key Skills**: SQL is essential for nearly all roles, followed by Python for advanced analysis, and Tableau/Power BI for visualization. These tools offer competitive salaries ranging from $91,000 to $100,000.

-**Emerging Trends**: Skills in big data (Snowflake, Spark) and cloud technologies (AWS, Azure) are increasingly valuable, with higher salaries reflecting their demand.

**Conclusion**: SQL, Python, and Tableau form the core skill set for entry-level analysts, while expertise in cloud and big data technologies provides opportunities for higher compensation in evolving data-driven industries.
