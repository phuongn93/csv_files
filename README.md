# Introduction
  Take a look at remote data analyst jobs market in Texas specifically fram data in 2023. This project explores top paying jobs, top paying skills, and where high demand meets high salary and predict some trends changing in 2024-2025.
   
   SQL queries? Check them out here [Project_Sql_Folder](/Project_SQL/)

# Data Set Background
  Data hails from Luke Barousse's course [SQL Course](https://lukebarousse.com/sql). It's packed with insights on job titles, salaries, locations, and essential skills.

  ### The questions I wanted to answer through my SQL queries were:

  1. What are the remoted top-paying data analyst jobs at the entry level in Texas?
  2. What are the top-paying data analyst jobs, and what skills are required at the entry level in Texas?
  3. What companies in Texas did they hire remoted data analyst with entry level? Which companies did they post the most jobs?
  4.  What are the most in-demand skills for data analysts in Texas?
  5. What are the most optimal skills to learn (aka it’s in high demand and a high-paying skill) for a remoted data analyst in Texas?
  6.  What are the top skills based on salary for remoted data analyst in Texas?
  
# Tools I Used

  -**Structured Query Language ( SQL)** : domain-specific language used to manage and manipulate data stored in relational databases. It allows me to convey data into some critical insights about what I am looking for.

  -**PostgreSQL** : open-source object-relational database management systsem that extends the SQL language with numerous features to safely store and scale complex data workloads, in this case handling the job posting data.

  -**Visual Studio Code (VS Code)** : an integrated development environment developed by Microsoft to provide a streamlined and efficient coding experience, executing my SQL queries.

  -**Git** : a distributed version control system that allows me to track changes in my codebase over time 

  _**GitHub** : a platform that hosts code repositories and utilizes Git for version control. It acts as a hub where I can collaborate on software projects, share files, and manage different versions of my code.
  
# The Anlysis
  Each query for this project investigates specific aspects of remoted data analyst job market in Texas. Here is how I use SQL to query data to answer each question:
  ### 1.Top Paying Data Analyst Jobs at the Entry Level in Texas:
  ```sql
  Select 
 job_id, company_id, job_title_short, job_location,search_location, job_work_from_home as remoted_job, job_no_degree_mention, salary_year_avg, job_posted_date
From job_postings_fact
Where
 job_title_short = 'Data Analyst' and job_location = 'Anywhere' and search_location = 'Texas, United States' and job_no_degree_mention = 'true' and salary_year_avg is not null
Order by salary_year_avg DESC;

*/ what is the max, min and avg of salary_year_avg of remoted data analyst jobs?

Select
 max(salary_year_avg) as maximum_salary,
 min(salary_year_avg) as minimum_salary,
 avg(salary_year_avg) as Avg_Salary

from (
    Select 
 job_id, company_id, job_title_short, job_location,search_location, job_work_from_home as remoted_job, job_no_degree_mention, salary_year_avg, job_posted_date
from job_postings_fact
where
 job_title_short = 'Data Analyst' and job_location = 'Anywhere' and search_location = 'Texas, United States' and job_no_degree_mention = 'true' and salary_year_avg is not null
order by salary_year_avg DESC
)

*/ which areas in Texas did they hire remoted and non-remoted data analysts at the entry level?/*

Select 
 job_location,
 count (job_id),
 round(avg (salary_year_avg),2) as TX_Avg_Salary
From job_postings_fact
Where
 job_title_short = 'Data Analyst' and job_location like '%TX%' and job_no_degree_mention = 'true' and salary_year_avg is not null
 group by job_location
 order by count DESC;
```
Here's the breakdown of the top data analyst jobs:

-**General Salary Statistics** : as a remoted data analyst at entry level has maximum salary as $650,000, mininum salary as $40,000, average salary as $103,119.43

-**Degree mention** : None of the jobs explicitly mention the need for a degree, indicating companies may prioritize skills and experience over formal education.

-**Top Locations**: The dominance of remote work at the entry level indicates a significant shift in hiring practices, offering competitive pay without requiring relocation. In addition, the prevalence of postings in these areas suggests high demand for professionals at the entry level in major metro areas in United States.


# What I Learned Technically 
# Conclusions From Data