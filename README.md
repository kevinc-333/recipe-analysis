# Recipe Analysis

## Introduction

In this project, we will examine the relationship between recipe complexity and overall nutrition. This is important because many people have very little time to cook for themselves, causing them to fall back on fast food and other non-nutritious foods. We plan to alleviate this problem by identifying recipes that are both easy and quick to make and still nutritious.

To do this, we will be using the Recipes and Ratings dataset, which contains recipes and ratings from food.com that were posted in 2008. The data comes in the form of two CSV files, one containing raw recipe data and one containing the ratings. 

---

`recipes.csv` contains information about the recipes themselves. There are 83,782 recipes and 12 total columns of this file.

<iframe
  src="assets/table1.html"
  width="800"
  height="600"
  frameborder="0"
  scrolling="yes"
></iframe>

`ratings.csv` contains the user ratings and reviews. There are 731,927 ratings and 5 columns total.

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