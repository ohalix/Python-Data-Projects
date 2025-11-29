# The Analysis

## 1. What are the most demanded skills for the top 3 most popular Data Roles?

To find the most demanded skills for the top 3 roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role i'm targeting.

VIew my notebook with detailed steps here:
[skills_count.ipynb](Advanced\Projects\skills_count.ipynb)

### Visualize Data
```python
fig, ax = plt.subplots(len(job_titles), 1)
sns.set_theme(style= 'ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_percentage[df_skills_percentage['job_title_short'] == job_title].head()
    
    sns.barplot(data= df_plot, x= 'skill_percentage', y= 'job_skills', ax= ax[i], hue= 'skill_percentage', palette= 'dark:g_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].set_xlim(0, 78)
    ax[i].legend().set_visible(False)
    
    for n, v in enumerate(df_plot['skill_percentage']):
        ax[i].text(v +1, n, f'{v:.0f}%', va= 'center')
    
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])


fig.suptitle('Likelyhood of skills requested in US Job Postings', fontsize = 15)
fig.tight_layout(h_pad= 0.5)
plt.show()
```


### Results
![Visualization of Top demanded skills](Advanced\Projects\images\skill_demand.png)


### Insights
- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 68% of job postings.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).


## 2. How are in-demand skills trending for Data Analysts?

### Visualization Data

```python
sns.set_theme(style= 'ticks')
sns.lineplot(data= df_plot, dashes= False, palette='tab10')
sns.despine()

plt.title('Trending Top SKills for Data Analysts in the US')
plt.ylabel('Likelyhood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()
# plt.tight_layout()

ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals= 0))

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i])
```

### Results 

![Visualization of Top demanded skills](Advanced\Projects\images\Top_skill_trends.png)
*Bar Graph for the trending top skills for data analysts in the US in 2023.*

### Insights




# 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis for data nerds
```python
sns.boxplot(data= df_us_top6, x='salary_year_avg', y='job_title_short', color='lightgreen', order= job_order)
sns.set_theme(style='ticks')


plt.title('Salary Comparison by Job Title in the US')
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
plt.xlim(0, 600000)
ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"${int(x/1000)}k"))
plt.show()
```

### Results

![Median Salaries for data roles in the US](Advanced\Projects\images\salary_boxplots.png)
*Box Plot visualizig the salary distribution for the top 6 data job titles*

### Insights
- There's a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.

- Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.

- The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

## Highest Paid & Most Demanded Skills for Data Analysts

### Visualize Data

```python
# fig, ax for 2 by 1 subplot, horizontal plots for generated dataframes, anf formating
fig,ax = plt.subplots(2, 1)

sns.set_theme(style= 'ticks')
sns.barplot(data= df_da_top_pay, x= 'median', y= df_da_top_pay.index, ax= ax[0], hue= 'median', palette= 'dark:g_r')
sns.barplot(data= df_da_skills, x= 'median', y= df_da_skills.index, ax= ax[1], hue= 'median', palette= 'light:g')
# df_da_top_pay[::-1].plot(kind= 'barh',y= 'median', ax= ax[0], legend= False)
# df_da_skills[::-1].plot(kind= 'barh', y= 'median', ax= ax[1], legend= False)


ax[0].set_title('Top Paying Skills for Data Analyst Jobs in the US')
ax[0].set_xlabel('')
ax[0].set_ylabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"${int(x/1000)}k"))
ax[0].legend_.remove()

ax[1].set_title('Most In-Demand Skills for Data Analyst Jobs in the US')
ax[1].set_xlabel('Median Salary ($USD)')
ax[1].set_ylabel('')
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"${int(x/1000)}k"))
ax[1].legend_.remove()


fig.tight_layout()
plt.show()
```
### Results

![The HighestPaid & Most In-Demand skills for Data Analysts in the US](Advanced\Projects\images\Highest_paid_and_most_in_demand_skills.png)


### Insights

- The top graph shows specialized technical skills like dplyr, Bitbucket, and Gitlab are associated with higher salaries, some reaching up to $200K, suggesting that advanced technical proficiency can increase earning potential.

- The bottom graph highlights that foundational skills like Excel, PowerPoint, and SQL are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.

- There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.


# 4. What is the most Optimal Skill to learn for Data Analysts?

### visualize Data
```python
sns.scatterplot(
    data= df_plot,
    x= 'skill_percent',
    y= 'median_salary',
    hue= 'technology'
)
sns.despine()
sns.set_theme(style= 'ticks')


texts = []
for i, txt in enumerate(df_skills_demanded.index):
    texts.append(plt.text(df_skills_demanded['skill_percent'].iloc[i], df_skills_demanded['median_salary'].iloc[i], txt))
adjust_text(texts, arrowprops=dict(arrowstyle= '->', color= 'gray'))

ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"${int(y/1000)}k"))
ax.xaxis.set_major_formatter(PercentFormatter(decimals= 0))


plt.title('Most Optimal skills for Data Analysts in the US')
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary (Yearly)')
plt.tight_layout()
plt.show()
```

### The Results
![Most Optimal skill for a Data Analyst](Advanced\Projects\most_optimal_skill_for_data_analysts.png)


### Insights
- The skill Oracle appears to have the highest median salary of nearly $97K, despite being less common in job postings. This suggests a high value placed on specialized database skills within the data analyst profession.

- More commonly required skills like Excel and SQL have a large presence in job listings but lower median salaries compared to specialized skills like Python and Tableau, which not only have higher salaries but are also moderately prevalent in job listings.

- Skills such as Python, Tableau, and SQL Server are towards the higher end of the salary spectrum while also being fairly common in job listings, indicating that proficiency in these tools can lead to good opportunities in data analytics.