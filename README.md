# Introduction
📊 Step into the world of data careers! Centered on data analyst positions, this project highlights 💰 the highest-paying opportunities, 🔥 the most sought-after skills, and 📈 the sweet spot where strong demand aligns with strong compensation in data analytics.

# Background
Motivated by the need to better understand the data analyst job landscape, this project was created to identify the best-paying roles and the skills employers value most—making it easier for others to find the right opportunities.

The dataset comes from Luke Barousse's SQL for Data Analytics course, offering a rich collection of information on job titles, salaries, locations, and key skills.

### The questions I wanted to answer through my SQL queries were:

1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

# Tools I Used
For this in-depth exploration of the data analyst job market, I relied on several essential tools:

- **SQL:** The core of my analysis, enabling me to query the data and extract meaningful insights.
- **PostgreSQL:** The database system of choice, well-suited for managing and processing the job posting dataset.
- **Visual Studio Code:** MMy primary workspace for organizing the database and running SQL queries.
- **Git & GitHub:** Crucial for version control and sharing my SQL work, supporting collaboration and transparent project development.

# The Analysis
Every query in this project was designed to explore a particular dimension of the data analyst job landscape. Below is the approach I took for each question:

### 1. Top Paying Data Analyst Jobs
To pinpoint the top-paying positions, I filtered data analyst roles by their average annual salary and location, concentrating specifically on remote opportunities. This query showcases the most lucrative options available in the field.

```sql
SELECT	
	job_id,
	job_title,
	job_location,
	job_schedule_type,
	salary_year_avg,
	job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst' AND 
    job_location = 'Anywhere' AND 
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10;
```
Here's the breakdown of the top data analyst jobs in 2023:
- **Wide Salary Range:** The top 10 highest-paying data analyst positions range from $184,000 to $650,000, demonstrating the strong earning potential within the field.
- **Diverse Employers:** Organizations such as SmartAsset, Meta, and AT&T appear among the top salary providers, reflecting strong demand across a variety of industries.
- **Job Title Variety:** Job titles range widely-from Data Analyst to Director of Analytics-highlighting the diverse roles and specializations that exist within the data analytics field.

![Top Paying Roles](/img/1_top_paying_roles.png)
*Bar graph visualizing the salary for the top 10 salaries for data analysts; ChatGPT generated this graph from my SQL query results*

### 2. Skills for Top Paying Jobs
To uncover which skills are essential for the highest-paying positions, I connected the job postings with the skills dataset, offering a clearer view of what employers prioritize for top-compensation roles.
```sql
WITH top_paying_jobs AS (
    SELECT	
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst' AND 
        job_location = 'Anywhere' AND 
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```
Here's the breakdown of the most demanded skills for the top 10 highest paying data analyst jobs in 2023:
- **SQL** is leading with a bold count of 8.
- **Python** follows closely with a bold count of 7.
- **Tableau** is also highly sought after, with a bold count of 6.
Other skills like **R**, **Snowflake**, **Pandas**, and **Excel** show varying degrees of demand.

![Top Paying Skills](/img/2_top_paying_roles_skills.png)
*Bar graph visualizing the count of skills for the top 10 paying jobs for data analysts; ChatGPT generated this graph from my SQL query results*

### 3. In-Demand Skills for Data Analysts

This query revealed the skills most commonly sought in job postings, helping spotlight the areas with the highest demand.

```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```
Here’s an overview of the most in-demand skills for data analysts in 2023:
- **SQL** and **Excel** continue to be core requirements, underscoring the importance of solid fundamentals in data handling and spreadsheet-based analysis.
- **Programming** and **Visualization** Tools-such as **Python**, **Tableau**, and **Power BI**-are crucial, reflecting the growing need for technical capabilities in data storytelling and supporting informed decision-making.

| Skills   | Demand Count |
|----------|--------------|
| SQL      | 7291         |
| Excel    | 4611         |
| Python   | 4330         |
| Tableau  | 3745         |
| Power BI | 2609         |

*Table of the demand for the top 5 skills in data analyst job postings*

### 4. Skills Based on Salary
Analyzing the average salaries linked to various skills highlighted which competencies command the highest pay.
```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
```
Below is a summary of the highest-paying skills for Data Analysts:
- **High Demand for Big Data & ML Skills:** The highest salaries are earned by analysts proficient in big data technologies (PySpark, Couchbase), machine learning platforms (DataRobot, Jupyter), and Python libraries (Pandas, NumPy), highlighting the industry’s strong demand for expertise in data processing and predictive modeling.
- **Software Development & Deployment Proficiency:** Expertise in development and deployment tools (GitLab, Kubernetes, Airflow) demonstrates a profitable intersection between data analysis and engineering, with high value placed on skills that enable automation and streamline data pipeline management.
- **Cloud Computing Expertise:** Proficiency with cloud and data engineering tools (Elasticsearch, Databricks, GCP) highlights the increasing significance of cloud-based analytics, indicating that expertise in these environments can substantially enhance earning potential in data analytics.

| Skills        | Average Salary ($) |
|---------------|-------------------:|
| pyspark       |            208,172 |
| bitbucket     |            189,155 |
| couchbase     |            160,515 |
| watson        |            160,515 |
| datarobot     |            155,486 |
| gitlab        |            154,500 |
| swift         |            153,750 |
| jupyter       |            152,777 |
| pandas        |            151,821 |
| elasticsearch |            145,000 |

*Table of the average salary for the top 10 paying skills for data analysts*

### 5. Most Optimal Skills to Learn

By merging insights from both demand and salary data, this query sought to identify skills that are not only highly sought after but also well-compensated, providing a strategic guide for skill development.

```sql
SELECT 
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```

| Skill ID | Skills     | Demand Count | Average Salary ($) |
|----------|------------|--------------|-------------------:|
| 8        | go         | 27           |            115,320 |
| 234      | confluence | 11           |            114,210 |
| 97       | hadoop     | 22           |            113,193 |
| 80       | snowflake  | 37           |            112,948 |
| 74       | azure      | 34           |            111,225 |
| 77       | bigquery   | 13           |            109,654 |
| 76       | aws        | 32           |            108,317 |
| 4        | java       | 17           |            106,906 |
| 194      | ssis       | 12           |            106,683 |
| 233      | jira       | 20           |            104,918 |

*Table of the most optimal skills for data analyst sorted by salary*

Below is a summary of the most valuable skills for Data Analysts in 2023:
- **High-Demand Programming Languages:** Python and R are particularly notable for their strong demand, with 236 and 148 job postings respectively. Although highly sought after, their average salaries-approximately $101,397 for Python and $100,499 for R-suggest that while these skills are valued, they are also relatively common in the job market.
- **Cloud Tools and Technologies:** Expertise in specialized technologies like Snowflake, Azure, AWS, and BigQuery demonstrates strong demand paired with relatively high average salaries, highlighting the increasing relevance of cloud platforms and big data tools in data analysis.
- **Business Intelligence and Visualization Tools:** Tableau and Looker, with demand counts of 230 and 49 respectively and average salaries of approximately $99,288 and $103,795, underscore the essential role of data visualization and business intelligence in transforming data into actionable insights.
- **Database Technologies:** The demand for expertise in both traditional and NoSQL databases (Oracle, SQL Server, NoSQL), with average salaries between $97,786 and $104,534, highlights the ongoing importance of skills in data storage, retrieval, and management.

# What I Learned

Throughout this journey, I’ve supercharged my SQL toolkit with some powerful enhancements:

- **🧩 Complex Query Crafting:** Honed advanced SQL skills, expertly joining tables and using WITH clauses to create temporary tables like a pro.
- **📊 Data Aggregation:** Became proficient with GROUP BY and leveraged aggregate functions like COUNT() and AVG() as powerful allies for summarizing data.
- **💡 Analytical Wizardry:** Enhanced my practical problem-solving abilities, transforming complex questions into actionable and insightful SQL queries.

# Conclusions

### Insights
The analysis revealed several key takeaways:

1. **Top-Paying Data Analyst Jobs**: The top-paying remote data analyst roles feature a broad salary range, with the highest reaching $650,000!
2. **Skills for Top-Paying Jobs**: Data analyst roles with the highest salaries demand advanced SQL skills, highlighting it as a key competency for top earnings.
3. **Most In-Demand Skills**: SQL is the most sought-after skill in the data analyst job market, making it indispensable for job seekers.
4. **Skills with Higher Salaries**: Specialized skills like SVN and Solidity correspond to the highest average salaries, reflecting a strong premium on niche expertise.
5. **Optimal Skills for Job Market Value**: SQL tops both demand and average salary metrics, making it one of the most strategic skills for data analysts to master in order to maximize their market value.

### Closing Thoughts

This project strengthened my SQL expertise and offered meaningful insights into the data analyst job market. The analysis serves as a roadmap for prioritizing skill development and targeting job opportunities. By concentrating on high-demand, well-compensated skills, aspiring data analysts can better position themselves in a competitive landscape. Overall, this exploration underscores the importance of continuous learning and staying attuned to emerging trends in data analytics.