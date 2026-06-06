# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles, I filtered out those positions by which ones werer the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the roles I'm targeting.

View my notebook with detailed steps here:
[2_skills_demand.ipynb](Job_Data_analysis\2_skill_demand.ipynb)

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

![Visualization for Top Skills for Data Nerds](Job_Data_analysis\Images\skill_likelihood_for_top_roles.png)

### Insights

- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (70%) and Data Engineers (61%).

- SQL is the most requested skill for Data Analysis and Data Scientist, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in the 61% of job psotings.

- Data Engineers require more Specialized technica skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).
