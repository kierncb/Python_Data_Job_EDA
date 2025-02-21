# Project Background
Navigating career choices can be daunting, especially for college students preparing to enter the workforce. As someone pursuing a data-related career, I recognize the importance of developing key skills early to stay competitive in the industry. This realization led me to ask: *Which skills should I focus on now to maximize my future success?*

This project is designed not only as a personal initiative for skill enhancement but also as a resource for other students aiming to build a strong foundation in data-related fields. By identifying essential competencies and gaining hands-on experience, aspiring data professionals can boost their confidence and preparedness for job opportunities. Ultimately, this project seeks to bridge the gap between academic learning and industry expectations, ensuring that students enter the workforce equipped with relevant and in-demand skills.

# Data Overview
The [data_jobs](https://huggingface.co/datasets/lukebarousse/data_jobs) dataset was sourced from Data Analyst Luke Barousse. It contains real job posting data collected through his website in 2023. The dataset consists of 786,000 rows and 17 columns.

| Column Name              | Data Type  | Description |
|--------------------------|-----------|-------------|
| job_title_short         | object    | Shortened version of the job title |
| job_title              | object    | Full job title |
| job_location          | object    | Location of the job posting |
| job_via               | object    | Source or platform where the job is posted |
| job_schedule_type      | object    | Type of work schedule (e.g., full-time, part-time) |
| job_work_from_home    | bool      | Indicates if the job allows remote work (True/False) |
| search_location       | object    | Location used for job search |
| job_posted_date       | object    | Date the job was posted |
| job_no_degree_mention | bool      | Whether a degree requirement is not mentioned (True/False) |
| job_health_insurance  | bool      | Indicates if health insurance is provided (True/False) |
| job_country          | object    | Country where the job is located |
| salary_rate          | object    | Type of salary (e.g., hourly, yearly) |
| salary_year_avg      | float64   | Average yearly salary |
| salary_hour_avg      | float64   | Average hourly salary |
| company_name         | object    | Name of the hiring company |
| job_skills          | object    | Skills required for the job |
| job_type_skills     | object    | Type of skills required (e.g., technical, soft skills) |

# The Questions
Here are the questions I aim to answer:

1. Which job role had the highest percentage of job postings?
2. Which country recorded the highest number of job openings?
3. Which companies were the top recruiters based on job postings?
4. Which job posting platform is used the most by employers?
5. What percentage of job postings offer work-from-home options, require a degree, or provide health insurance?
6. What are the key trends and patterns in job postings for Data Analyst, Data Engineer, and Data Scientist roles over time?
7. What are the most in-demand skills for the top three most popular data roles in the US?

# Data Preparation and Cleanup
I began by importing the necessary libraries and loading the dataset, then performed initial data cleaning tasks to ensure data quality.

```python
# Importing libraries
import ast
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Loading data 
df = pd.read_csv(r"...\data_jobs.csv")

# Data Cleaning
df = df.drop(columns=['salary_rate', 'salary_year_avg', 'salary_hour_avg'])  # Drop columns with more than 60% null values
df = df.dropna()  # Drop rows containing null values
df = df.drop_duplicates()  # Remove duplicate rows
df['job_via'] = df['job_via'].str.replace(r'\s*via\s*', '', regex=True)  # Remove 'via' from values in the column
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x))  # Convert string representations of lists to actual Python lists
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])  # Convert to datetime format

```
# Insight Deep-Dive

### 1. Which job role had the highest percentage of job postings?

#### Data Visualization
```python
# Plot
sns.set_theme(style='ticks')
ax = sns.barplot(data=df_job_titles, x='proportion', y='job_title_short', hue='proportion', palette='dark:b_r', legend=False)
sns.despine()

# Add percentage labels
for container in ax.containers:
    ax.bar_label(container, fmt='%.1f%%', padding=3)

# Set x-axis limit to 30%
plt.xlim(0, 30)

# Set and format lables
plt.title('Job Hiring Distribution by Job Title', weight='bold', fontsize=15, pad=15)
plt.xlabel('Jobs (%)', weight='bold')
plt.ylabel('')
plt.show()
```
#### Result
![job-job_title](data_visualization/job-job_title.png)

*Bar graph displaying the job role with the highest number of job offers.*

#### Insights:
- Data Engineer, Data Analyst, and Data Scientist dominate the job market, reflecting the increasing need for data professionals in various industries.

- Senior Data Engineer, Senior Data Scientist, and Senior Data Analyst have significantly fewer job offers than their junior counterparts. This suggests that entry and mid-level roles are more widely available compared to senior positions.

### 2. Which country recorded the highest number of job openings?

#### Data Visualization

```python
# Plot
sns.set_theme(style='ticks')
ax = sns.barplot(data=df_job_country, x='proportion', y='job_country', hue='proportion', palette='dark:b_r', legend=False)
sns.despine()

# Add percentage labels
for container in ax.containers:
    ax.bar_label(container, fmt='%.1f%%', padding=3)

# Set x-axis limit to 30%
plt.xlim(0, 30)

# Set and format lables
plt.title('Geographic Distribution of Job Openings', weight='bold', pad=15, fontsize=15)
plt.xlabel('Jobs (%)', weight='bold')
plt.ylabel('')
plt.show()
```

#### Results
![job-hiring_location](data_visualization/job-hiring_location.png)

*Bar graph showing the country with the highest number of job offers*

#### Insights:
- The overwhelming number of jobs in the U.S. suggests a strong and diverse job market, possibly driven by a large economy, technological advancements, and a high concentration of multinational companies. This dominance may indicate greater employment opportunities but also higher competition.

### 3. Which companies were the top recruiters based on job postings?

#### Data Visualization

```python
# Plot
sns.set_theme(style='ticks')
ax = sns.barplot(data=df_job_company, x='proportion', y='company_name', hue='proportion', palette='dark:b_r', legend=False)
sns.despine()

# Add percentage labels
for container in ax.containers:
    ax.bar_label(container, fmt='%.1f%%', padding=3)

# Set x-axis limit to 1%
plt.xlim(0, 1)

# Set and format lables
plt.title('Company-wise Hiring Distribution', weight='bold', pad=15, fontsize=15)
plt.xlabel('Jobs (%)', weight='bold')
plt.ylabel('')
plt.show()
```

#### Results
![job-hiring_company](data_visualization\job-hiring_company.png)

*Bar graph showing the company with the highest number of job offers.*

#### Insights:
- Emprego has the highest number of job postings, significantly more than other companies. This suggests that they are aggressively hiring or have a broader range of job openings.

- The companies listed span various industries, including finance (Citi, Capital One), healthcare (UnitedHealth Group), retail (Walmart), and consulting (Booz Allen Hamilton). This suggests that job opportunities exist across multiple sectors.

### 4. Which job posting platform is used the most by employers?

#### Data Visualization
```python
# Plot
sns.set_theme(style='ticks')
ax = sns.barplot(data=df_job_platform, x='proportion', y='job_via', hue='proportion', palette='dark:b_r', legend=False)
sns.despine()

# Add percentage labels
for container in ax.containers:
    ax.bar_label(container, fmt='%.1f%%', padding=3)

# Set x-axis limit to 30%
plt.xlim(0, 30)

# Set and format lables
plt.title('Most Popular Job Posting Platforms Among Employers', weight='bold', fontsize=13, pad=10)
plt.xlabel('Jobs (%)', weight='bold')
plt.ylabel('')
plt.show()
```
#### Results
![job-hiring_platform](data_visualization\job-hiring_platform.png)

*Bar graph showing the job posting platform with the highest number of job offers.*

#### Inisghts:
- LinkedIn has the highest number of job postings, significantly outpacing all other platforms. This suggests that LinkedIn is a primary platform for job seekers and recruiters.

- While LinkedIn has the most jobs, other factors like industry focus, job quality, and user base may impact which platform is best for job seekers.

### 5. What percentage of job postings offer work-from-home options, require a degree, or provide health insurance?

#### Data Visualization
```python
# Dictionary mapping dataset column names to more readable chart titles
dict_column = {
    'job_work_from_home': 'Work from Home Offered',
    'job_no_degree_mention': 'Degree Requirement',
    'job_health_insurance': 'Health Insurance Offered'
}

# Create a figure with 1 row and 3 columns of subplots, set figure size
fig, ax = plt.subplots(1, 3, figsize=(11, 3.5))

# Loop through each column-title pair in the dictionary
# Create multiple pie charts—one for each column in the dataset
for i, (column, title) in enumerate(dict_column.items()):
    ax[i].pie(
        df[column].value_counts(), 
        labels=['False', 'True'], 
        autopct='%1.1f%%', 
        startangle=90, 
    )
    ax[i].set_title(title, weight='bold', fontsize=13)

plt.show()
```
#### Results
![job-venefits](data_visualization\job-benefits.png)

*Pie charts showing the distribution of job postings based on work-from-home availability, degree requirements, and health insurance offerings.*

#### Insights:
- Only 9.5% of job postings offer work-from-home options, while the majority (90.5%) require employees to work on-site. This suggests that remote work opportunities are still relatively scarce in the dataset, which could be a consideration for job seekers looking for flexible work arrangements.

-  About 28.7% of job postings explicitly require a degree, whereas 71.3% do not mention a degree requirement. This indicates that many employers prioritize skills and experience over formal education, making it easier for non-degree holders to find job opportunities.

- Only 12.2% of job postings include health insurance as a benefit, while the majority (87.8%) do not. This highlights a potential gap in employee benefits, which could influence job seekers' decisions when evaluating job offers. 

### 6. What are the key trends and patterns in job postings for Data Analyst, Data Engineer, and Data Scientist roles over time?

#### Visualize Data

```python
# Set theme
sns.set_theme(style='ticks')

# Create the line plot
ax = sns.lineplot(data=job_percentages.iloc[:, :5], dashes=False, palette='tab10')

# Formatting
plt.xlabel('2023', weight='bold')
plt.ylabel('Job Postings (%)',weight='bold')
plt.title('Job Posting Percentage Trends Over Time', fontsize=15, weight='bold', pad=16)

# Set and format lables
plt.legend(loc='upper left', bbox_to_anchor=(1, 1))
sns.despine()
plt.show()
```

#### Results
![Question_6](data_visualization\job_title-trend.png)

*Bar graph visualizing the monthly trendof the top 3 data roles.*

#### Insights:
- SQL is the most in-demand skill for Data Analysts, Data Engineers, and Data Scientists. This confirms that structured data handling, querying databases, and managing relational databases are fundamental skills in data-related careers.

- Python is the most in-demand skill for Data Scientists and a crucial skill for Data Engineers. This suggests that Data Engineers and Data Scientists need strong coding skills, while Data Analysts can focus more on querying and visualization.

### 7. What are the top trending skills for data engineers in the US?

#### Data Visualization

```python
# Create subplots with the number of rows equal to the number of job titles
fig, ax = plt.subplots(len(job_titles), 1)  

# Loop through each job title to generate a bar plot
for i, job_title in enumerate(job_titles):
    # Filter the top 5 most requested skills for the current job title
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)

    # Create a horizontal bar plot of skill percentages
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills',  ax=ax[i],hue='skill_count', palette='dark:b_r'  )

    # Apply common formatting to all subplots
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 100)

    # Remove x-axis tick labels for all subplots except the last one
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

    # Add percentage labels to the bars
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15, weight='bold')
fig.tight_layout(h_pad=.8)

plt.show()
```
### Results
![job_skill-job_title](data_visualization\job_skill-job_title.png)

*Bar graph visualizing the likelihood of skills requested in US job postings in 2023*

### Insights:
- SQL and Python are the most in-demand skills, consistently appearing in over 70% of job postings, making them essential for data engineers.

- AWS, Azure, and Spark are valuable but secondary skills, with AWS maintaining moderate demand (40–50%) and Azure/Spark showing steady but lower demand (~30–35%).

- It suggests that SQL and Python are fundamental skills for data engineers, making them essential for securing job opportunities. Meanwhile, AWS, Azure, and Spark serve as valuable but complementary skills, meaning proficiency in them can enhance job prospects but may not be mandatory for all roles.

# Recommendation
Throughout this project, I gained deeper insights into the data analyst job market while strengthening my technical skills in Python, particularly in data manipulation and visualization. Here are some key takeaways:

- By utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and additional tools for various analytical tasks, I significantly improved my ability to handle, analyze, and interpret complex datasets with greater efficiency and accuracy.

- I learned that thorough data cleaning and preparation are fundamental to any data analysis process, as they help eliminate inconsistencies, handle missing values, and enhance data quality. Ensuring clean and well-structured data is crucial for generating accurate, meaningful, and reliable insights.

- This project underscored the importance of aligning technical skills with industry demand. By analyzing the relationship between skill requirements, and job availability, I gained a clearer understanding of how to strategically position myself in the competitive tech landscape. This knowledge allows for more informed career planning, ensuring that my skill set remains relevant and valuable in the evolving job market.

# Conclusion
This deep dive into the data job market has provided valuable insights into the essential skills and emerging trends shaping the field. The knowledge gained not only enhances my understanding but also offers actionable guidance for professionals aiming to grow in data-related job. As the industry continues to evolve, ongoing analysis and skill development will be vital to staying competitive. This project serves as a strong foundation for further exploration, reinforcing the importance of continuous learning, adaptability, and data-driven decision-making in this ever-changing landscape.