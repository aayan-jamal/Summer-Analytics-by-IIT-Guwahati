# Summer Analytics By IIT Guwahati 

## Project Overview

**Project Title**: Cars sale Analysis  
**Level**: Beginner  
**Database**: `Cars`

Welcome to your first assignment of Summer Analytics 2025!
We hope you're excited to implement and test everything you've learned so far.

This project is designed to demonstrate basic data analysis and visualization skills using Python libraries such as Pandas, NumPy, Seaborn, and Matplotlib. You'll be working with a dataset that includes information about cars, and will use it to answer a series of interesting questions.

## Objectives

📊 What you'll do:
Import and explore a real-world dataset about cars

Practice basic data wrangling and transformation with pandas

Use seaborn and matplotlib to create insightful visualizations

Strengthen your analytical thinking with hands-on coding


### 1. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. Start by importing all important libraries 
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

2. ** Read the csv file and assign it to a variable .
```
cars_df = pd.read_csv('cars.csv')
cars_df.head(2)
```

3.*** Display shape of dataframe
Expected Output - (398, 9)***
```
cars_df.shape
```

4. **Print all columns of dataframe.**:
```
cars_df.columns
```

5. ** Set the 'name' column as the index of dataframe**:
```
cars_df.set_index('name', inplace=True)
cars_df.head()
```

6. **Print a list of all the unique mpg values.**:
```
cars_df['mpg'].nunique()
```

7. **Create a column which contains the horsepower divided by weightas its metric and make this new column the index.**:
```
cars_df['hp_weight'] = cars_df['horsepower'] / cars_df['weight']

cars_df.head(2)
```

8. **What is name of car that has the highest horsepower? **:
```
cars_df.reset_index(inplace=True)
max_hp = cars_df.loc[cars_df['horsepower'].idxmax()]
print("Car with the highest horsepower:", max_hp['name'])
```

9. **How many cars have mpg ≥ 35?.**:
```
len(cars_df[cars_df['mpg'] >= 35])
```

10. ** What is the most common origin for cars with horsepower > 100 and weight < 3000?**:
```
common_origin =  cars_df[(cars_df['horsepower'] > 100) & (cars_df['weight'] < 3000)]
most_common = common_origin['origin'].mode()[0]
print("Most common origin for cars with horsepower", most_common)
```

11. ** What is the mean acceleration of cars from Japan? (rounded to 2 decimals) **:
```
  japan_cars = cars_df[cars_df['origin'] == 'Japan']
mean_acceleration = round(japan_cars['acceleration'].mean(), 2)
print("Mean acceleration of cars from Japan:", mean_acceleration)
```
 12. **Which year had the highest average mpg? **   
```
   avg_mpg = cars_df.groupby('model_year')['mpg'].mean()
year_avg  = avg_mpg.idxmax()
high_avg_mpg = round(avg_mpg.max(), 2)
print(f"Year with the highest average MPG: {year_avg} (Average MPG: {high_avg_mpg})")
```
13. ** Find the car (or cars) with the best ratio of horsepower to weight among all cars that also have above-median mpg. **:
```
    median_mpg = cars_df['mpg'].median()
above_median = cars_df[cars_df['mpg'] > median_mpg].copy()
above_median['hp_weight'] = above_median['horsepower'] / above_median['weight']
max_ratio = above_median['hp_weight'].max()
best_car = above_median[above_median['hp_weight'] == max_ratio]
print(best_car['name'].values)

````
14. *** Design a multi-line plot using Matplotlib or Seaborn that shows the evolution of average mpg over the years, separately for each origin **
```
avg_mpg_by_year_origin = cars_df.groupby(['model_year', 'origin'])['mpg'].mean().reset_index()

sns.set(style="whitegrid")

plt.figure(figsize=(12, 6))
sns.lineplot(data=avg_mpg_by_year_origin, x='model_year', y='mpg', hue='origin', marker='o')

plt.title('Evolution of Average MPG Over the Years by Origin', fontsize=14)
plt.xlabel('Model Year')
plt.ylabel('Average MPG')
plt.legend(title='Origin')
plt.xticks(rotation=45)

plt.tight_layout()
plt.show()
```
15. ** Create a Seaborn scatterplot (or PairGrid) where:
X = horsepower

Y = weight

Color by: origin

Size by: mpg

Hue order = ['japan', 'europe', 'usa']

Add meaningful plot titles and axis titles. **:
```
plt.figure(figsize=(12, 6))
scatter = sns.scatterplot(
    data=cars_df,
    x='horsepower',
    y='weight',
    hue='origin',
    size='mpg',
    sizes=(40, 300),
    hue_order=['japan', 'europe', 'usa'],
    alpha=0.7,
    palette='Set2'
)

plt.title('Horsepower vs. Weight of Cars Colored by Origin and Sized by MPG', fontsize=14)
plt.xlabel('Horsepower')
plt.ylabel('Weight')
plt.legend(title='Origin', bbox_to_anchor=(1.05, 1), loc='upper left')
plt.tight_layout()

plt.show()
```
16. ** We define a “consistent” car model as one that was produced over multiple years and had very low variation in mpg across those years (standard deviation < 1.0).
    Tasks:

Identify car names that appear in more than one model_year.

For each such name, compute the standard deviation of mpg across years.

Return the car(s) with the lowest variation in mpg, among those with at least 2 appearances and std(mpg) < 1.0.

Report the model name(s), number of appearances, and the average mpg.

Bonus: Sort the result by number of appearances (descending), then mpg (descending).Tasks **:
```
grouped = cars_df.groupby('name').agg(
    appearances=('model_year', 'nunique'),
    mpg_std=('mpg', 'std'),                   
    avg_mpg=('mpg', 'mean')                   
).reset_index()

consistent_cars = grouped[(grouped['appearances'] >= 2) & (grouped['mpg_std'] < 1.0)]


min_std = consistent_cars['mpg_std'].min()


lowest_variation_cars = consistent_cars[consistent_cars['mpg_std'] == min_std]


sorted_cars = lowest_variation_cars.sort_values(by=['appearances', 'avg_mpg'], ascending=[False, False])


result = sorted_cars[['name', 'appearances', 'avg_mpg']]

print("Consistent car models with very low MPG variation (<1.0 std) over multiple years:")
print(result)
```



## Conclusion

This beginner-level project provided an insightful journey into data analysis using Python. By working with a real-world dataset of car specifications, we explored and answered a series of analytical questions that demonstrated practical applications of libraries like Pandas, NumPy, Matplotlib, and Seaborn.

Throughout the analysis, we:

Explored key attributes of the dataset, such as horsepower, weight, mpg, and origin.

Conducted filtering, grouping, and transformation operations to uncover meaningful insights.

Identified trends over time, such as the increase in average MPG across different car origins.

Visualized patterns using scatter plots and line plots for better interpretation.

Applied logical thinking to derive advanced insights like identifying consistent car models or evaluating the power-to-weight efficiency.

This project not only strengthened fundamental data wrangling and visualization skills but also encouraged structured analytical thinking—a core aspect of data science.

The Cars dataset served as a great example of how raw data can be transformed into actionable insights, preparing us for more complex datasets and real-world business problems ahead.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the csv file provided in the `Cars.csv` file to create and populate the database.
3. **Run the Queries**: You can run the Python script or notebook that contains the analysis:
For .py files: `python analysis.py`
For Jupyter Notebooks: `jupyter notebook cars_analysis.ipynb`


5. **Explore the Results**
Check printed outputs in the terminal or notebook cells for answers to the analytical questions.

View visualizations to gain insights into trends like MPG over time, power-to-weight ratios, etc.

## Author - Jauwad Jamal

This project is part of my portfolio, showcasing the Data Analysis skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!


