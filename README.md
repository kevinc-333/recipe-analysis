
---
## Introduction

In this project, we aim to answer the question of how recipe difficulty affects nutrition. This is important because many people have very little time to cook for themselves, causing them to fall back on fast food and other non-nutritious foods. We plan to alleviate this issue by identifying whether simple recipes have enough nutrition to be healthy.

To do this, we will be using the Recipes and Ratings dataset, which contains recipes and ratings from food.com that were posted in 2008. The data comes in the form of two CSV files, one containing raw recipe data and one containing the ratings.

---

`recipes.csv` contains information about the recipes themselves. There are 83,782 rows and 12 total columns of this file.

|    | name                                 |     id |   minutes |   contributor_id | ... |
|---:|:-------------------------------------|-------:|----------:|-----------------:|----:|
|  0 | 1 brownies in the world    best ever | 333281 |        40 |           985201 | ... |
|  1 | 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | ... |
|  2 | 412 broccoli casserole               | 306168 |        40 |            50969 | ... |
|  3 | millionaire pound cake               | 286009 |       120 |           461724 | ... |
|  4 | 2000 meatloaf                        | 475785 |        90 |          2202916 | ... |

`ratings.csv` contains the user ratings and reviews. There are 731,927 rows and 5 columns total.

|    |   user_id |   recipe_id | date       |   rating | review                                                                                                                                                                                                        |
|---:|----------:|------------:|:-----------|---------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  0 |   1293707 |       40893 | 2011-12-21 |        5 | So simple, so delicious! Great for chilly fall evening. Should have doubled it ;)<br/><br/>Second time around, forgot the remaining cumin. We usually love cumin, but didn't notice the missing 1/2 teaspoon! |

---

Before we do any data cleaning, we first must combine these two datasets into a single CSV for ease of use. First, we will merge `recipes.csv` with `ratings.csv` via a left merge. This will keep all recipes regardless of if they have reviews, while removing all reviews for recipes outside of our scope. 

Finally, we will add an additional column containing the average rating of each recipe. This is done by first converting all ratings of 0 to NaN values, as ratings of 0 simply represent a review without a rating; converting these to NaN will allow them to be ignored when averaging ratings. Then, the averages are calculated for each recipe individually.


Now, our dataframe has 234,429 rows and 18 columns after the merge. For the purposes of this report, only a few of these columns are of interest. These are:
- `minutes`: The minutes taken to prepare the recipe
- `nutrition`: A list containing various nutritional information, like calorie count and protein count
- `n_steps`: The number of steps in the recipe
- `n_ingredients`: The number of ingredients in the recipe
- `rating`: The rating of the review
- `average_rating`: The average rating for the recipe


## Data Cleaning and Exploratory Data Analysis
## Assessment of Missingness
## Hypothesis Testing
## Framing a Prediction Problem
## Baseline Model
## Final Model
## Fairness Analysis