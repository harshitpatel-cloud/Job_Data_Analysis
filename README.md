# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying) 

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter for Jobs in India

To focus my analysis on the Indian. job market, I apply filters to the dataset, narrowing down to roles based in India.


```python
df_IND = df[df['job_country'] == 'India']

```
Note : The `salary_year_avg` column in the dataset that contains the yearly salary for each role, seems ambiguous regarding the currency unit (USD or INR). Hence only for questions concerning the salary analysis I have filtered the dataset on US based jobs.

# The Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles, I filtered out those positions by which ones werer the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the roles I'm targeting.

View my notebook with detailed steps here:
[2_skills_demand.ipynb](Project\2_skill_demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(top_roles), 1)

sns.set_theme(style='ticks')
for i, job_title in enumerate(top_roles):
    df_plot = df_skills_pct[df_skills_pct['job_title_short'] == job_title].head(5)
    sns.barplot(df_plot, x='skill_pct', y='job_skills', ax=ax[i], hue='skill_pct', palette='dark:b_r', legend=False)
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].set_xlim(0, 80)

    for idx, val in enumerate(df_plot['skill_pct']):
        ax[i].text(val + 1, idx, f"{val:.0f}%", va='center')

    if i != len(top_roles) - 1:
        ax[i].set_xticks([])

fig.suptitle('Likelihood of skills Requested in job Postings', fontsize=15)
fig.tight_layout()
plt.figure(figsize=(15, 5.5))
plt.show()
```

### Results

![Visualization for Top Skills for Data Nerds](Project\Images\skill_likelihood_for_top_roles.png)

### Insights

- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (70%) and Data Engineers (61%).

- SQL is the most requested skill for Data Analysis and Data Scientist, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in the 61% of job psotings.

- Data Engineers require more Specialized technica skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).

## 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3_Skills_Trend](Project\3_Skills_Trend.ipynb).

### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=False))

for column in df_plot.columns:
    plt.text(11.2, df_plot.loc['Dec', column], column)

sns.despine()
plt.title('Trending Top Skills for Data Analysts in India')
plt.ylabel('Likelihood in Job Posting')

plt.xlabel(2023)
plt.legend().remove()
plt.show()
```

### Results

![Trending Top Skills for Data Analysts in India](Project\Images\skill_Trend_DA.png) </br>
*Bar graph visualizing the trending top skills for data analysts in India in 2023.*

### Insights:
- SQL remains the most consistently demanded skill throughtout the year.

- Python and Excel both being the second most demanded skill throughout the year, although python experienced a significant increase in demand starting around mid July appearing on top of Excel at the end of year.

- Tableu shows relatively stable demand throughout the year with some fluctuations while Power BI though less demanded compared to other skills, shows a sligh upward trend towards the year's end.

## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most. 

View my notebook with detailed steps here: [4_Salary_Analysis](Project\4_Salary_Analysis.ipynb).

### Visualize the Data

```python
sns.boxplot(df_US_top6, x = 'salary_year_avg', y = 'job_title_short', order = job_order)
sns.set_theme(style='ticks')

plt.title('Salary distributions in US')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
ticks_x = plt.FuncFormatter(lambda x, pos: f"${int(x/1000)}K")
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.xlim(0, 600_000)
plt.show()
```

### Results
![Salary Distributions of the Data Jobs in the US](Project\Images\Salary_boxplot.png)</br>
*Bar plot visualizingthe salary distributions for the top 6 data job titles.*

### Insights:

- There's a significant variation in the salar ranges across different job titles. Senior Data Scientist positions tend to have the highest Salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.

- Senior Data Engineer and Senior Data Scientist roles shpw a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In constrast, Data analyst roles demonstrate more consistency in Salary, with fewer outliers.

- The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher medium salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsiblities increase.

### Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

#### Visualize Data 

```python
fig, ax = plt.subplots(2, 1)
sns.set_theme(style='ticks')
sns.barplot(data=df_DA_IND_topPay, x='median', y=df_DA_IND_topPay.index, ax=ax[0], hue='median', palette='dark:b_r')
ax[0].set_ylabel(' ')
ax[0].set_title('Top 10 Highest Paid Skills for Data Analysts')
ax[0].set_xlim(0, 200_000)
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"₹{x/10000:.1f}L"))
ax[0].legend().remove()

sns.barplot(data=df_Da_IND_skills, x='median', y=df_Da_IND_skills.index, ax=ax[1], hue='median', palette='dark:b_r')
ax[1].set_ylabel(' ')
ax[1].set_title('Top 10 Most in-demand Skills for Data Analysts')
ax[1].set_xlim(0, 200_000)
ax[1].set_xlabel('Median Salary (INR)')
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"₹{x/10000:.1f}L"))
ax[1].legend().remove()

fig.tight_layout()
plt.show()
```
### Results

Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the US:

![The Highest Paid 7 Most In-Demand Skills for Data Analysts in the US](Project\Images\Highest_paid_and_most_demanded_skills.png)</br>
*Two separate bar graphs visualizing the highest paid skills and most in-demand skils for data analysts in India.*

### Insights:

- The top graph shows that specialized technical skills like `pyspark`, `gitlab` and `gdpr` are associated with higher salaries, some reaching up to ₹16.5 lpa, suggesting that advanced technical proficiency can increase earning potential.

- The bottom grpah highlights that foundational skills like `spark`, `Power BI` and `tableu` are the most in-demand. This demonstrates that importance of these core skills for employability in the data analysis roles. 

- There's a clear distinction between the skills that are highest paid and those that are most in-demand, Data analysts aiming to maximize their career potential should consider developing a diverse skill set that included both high-paying specialized skills and widely demanded foundational skills. 

## 4. What is teh most optimal skill to lear for Data Analysts?


To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn. 

View my notebook with detailed steps here: [5_optimal_skills](Project\5_optimal_skills.ipynb).

#### Visualize Data

```python
from adjustText import adjust_text

sns.scatterplot(df_DA_skills, x='skill_percent', y='median_salary')

#Prepare texts fro adjustText
texts = []
for txt in df_DA_skills.index:
    texts.append(plt.text(df_DA_skills.loc[txt, 'skill_percent'], df_DA_skills.loc[txt, 'median_salary'], txt))

#Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

#Set axis, labels, title and legend
plt.xlabel('Percent of Job Postings')
plt.ylabel('Median Yearly Salary (USD)')
plt.title('Most Optimal Skills for Data Analysts in US')

from matplotlib.ticker import PercentFormatter

ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"${int(y/1000)}K"))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=False))
#Adjust layout and display plot
plt.tight_layout()
plt.show()

```

### Results

![Most Optimal Skills for Data Analysts in the US](Project\Images\Most_optima_skills_to_learn_DA_US.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US.*

### Insights:

- The skill `Oracle` appears to have the highest median salary of nearly $97K, despite being less common in job postings. This suggests a high value placed on specialized database skills within the data analyst profession.

- More commonly required skills like `Excel` and `SQL` have a large presence in job listings but lower median salaries compared to specialized skills like `Python` and `Tableau`, which not only have higher salaries but are also moderately prevalent in job listings.

- Skills such as `Python`, `Tableau`, and `SQL Server` are towards the higher end of the salary spectrum while also being fairly common in job listings, indicating that proficiency in these tools can lead to good opportunities in data analytics.

### Visualizing Different Techonologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

```python
from adjustText import adjust_text

fig = sns.scatterplot(df_plot, 
x='skill_percent', 
y='median_salary',
hue='technology',)

sns.move_legend(fig, 'upper left', bbox_to_anchor=(1.05, 1))
sns.despine()
sns.set_theme(style='ticks')

#Prepare texts fro adjustText
texts = []
for txt in df_DA_skills.index:
    texts.append(plt.text(df_DA_skills.loc[txt, 'skill_percent'], df_DA_skills.loc[txt, 'median_salary'], txt))

#Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

#Set axis, labels, title and legend
plt.xlabel('Percent of Job Postings')
plt.ylabel('Median Yearly Salary (USD)')
plt.title('Most Optimal Skills for Data Analysts in US')

from matplotlib.ticker import PercentFormatter

ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"${int(y/1000)}K"))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=False))

#Adjust layout and display plot
plt.figure(figsize=(12, 12))
plt.tight_layout()
plt.show()
```

### Results

![Most Optimal Skills for Data Analysts in the US with Coloring by Technology](Project\Images\Most_optimal_tech_to_learn_US.png)  
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US with color labels for technology.*


### Insights:

- The scatter plot shows that most of the `programing` skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programing expertise might offer greater salary benefits within the data analytics field.

- Analyst tools (colored orange), indicate `Tableu`, `Excel` and `Power BI`, are prevalent in job postinfs and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This categroy not only has good salaries but is also versatile across different types of data tasks.

- The database skills (colored Brown), such as `SQL Server` is associated with some of the highest salaries among data analyst tools. This indicate a significant demand and valuation for data management and manipulation expertise in the industry.

# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.


# Insights

This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
- **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.


# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.


# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.