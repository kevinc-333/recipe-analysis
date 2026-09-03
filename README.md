
# Introduction

In this project, we will examine the relationship between recipe complexity and overall nutrition. This is important because many people have very little time to cook for themselves, causing them to fall back on fast food and other non-nutritious foods. We plan to alleviate this problem by identifying recipes that are both easy and quick to make and still nutritious.

To do this, we will be using the Recipes and Ratings dataset, which contains recipes and ratings from food.com that were posted in 2008. The data comes in the form of two CSV files, one containing raw recipe data and one containing the ratings. 

`recipes.csv` contains information about the recipes themselves, including the name, recipe ID, nutritional information, minutes taken to prepare, and more. There are 83,782 recipes and 12 total columns of this file.

|    | name | id | minutes | contributor_id | submitted | tags | nutrition | n_steps | steps | description | ingredients | n_ingredients |
|---:|:-------------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------|----------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|
|  0 | 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make'... | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0] |        10 | ['heat the oven to 350f and arrange the rack in the middle', 'line... | these are the most; chocolatey... | ['bittersweet chocolate', 'unsalted butter', 'eggs'... |               9 |

`ratings.csv` contains the user ratings and reviews, and its columns include the recipe ID the review was left on, the rating given, and the text of the review. There are 731,927 ratings and 5 columns total.

Before we do any data cleaning, we first must combine these two datasets into a single CSV for ease of use. First, we will merge `recipes.csv` with `ratings.csv` via a left merge. This will keep all recipes regardless of if they have reviews, while removing all reviews for recipes outside of our scope. 





# Data Cleaning and Exploratory Data Analysis
# Assessment of Missingness
# Hypothesis Testing
# Framing a Prediction Problem
# Baseline Model
# Final Model
# Fairness Analysis