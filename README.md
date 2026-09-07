
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

### Data Cleaning

First, some columns look like lists but are actually strings. We converted these to lists by splitting up the strings. Additionally, the `nutrition` column contained a list of numbers without any indication for what it meant.

| nutrition |
|:----------|
|[138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]|

These were converted into separate columns, one for each nutritional value.

|    |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbs |
|---:|-----------:|------------:|--------:|---------:|----------:|----------------:|--------:|
|  0 |      138.4 |          10 |      50 |        3 |         3 |              19 |       6 |
|  1 |      595.1 |          46 |     211 |       22 |        13 |              51 |      26 |
|  2 |      194.8 |          20 |       6 |       32 |        22 |              36 |       3 |
|  3 |      194.8 |          20 |       6 |       32 |        22 |              36 |       3 |
|  4 |      194.8 |          20 |       6 |       32 |        22 |              36 |       3 |

Finally, we removed rows with extreme outliers. This was done by cutting off all rows that had either `minutes` over the 90th percentile or a nutrient over the 90th percentile. Below is a table of the maximum values and the 90th percentile before cleaning.

| Column        | Max Value      |   90th Percentile |
|:--------------|---------------:|------:|
| minutes       |     1.0512e+06 |   125 |
| calories      | 45609          |   779 |
| total_fat     |  3464          |    68 |
| sugar         | 30260          |   137 |
| sodium        | 29338          |    60 |
| protein       |  4356          |    80 |
| saturated_fat |  6875          |    89 |
| carbs         |  3007          |    25 |

The cleaned dataframe ended up with 151729 rows and 24 columns. A subset of the dataframe is shown below.

|    | name                                    |   minutes |   n_steps |   protein |   calories |
|---:|:----------------------------------------|----------:|----------:|----------:|-----------:|
|  0 | 1 brownies in the world    best ever    |        40 |        10 |         3 |      138.4 |
|  2 | 412 broccoli casserole                  |        40 |         6 |        22 |      194.8 |
|  7 | 2000 meatloaf                           |        90 |        17 |        29 |      267   |
|  9 | 5 tacos                                 |        20 |         5 |        39 |      249.4 |
| 12 | blepandekager   danish   apple pancakes |        50 |        10 |        19 |      358.2 |

### Univariate Analysis

In order to explore the data, we should see the distribution of `minutes`. 

<iframe
  src="assets/2-minutes-hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that the plot is centered at roughly 30 minutes. Most recipes are under an hour to make, though some recipes take upwards of 2 hours. This makes sense, longer recipes are harder to make and thus less people would be willing to create those recipes.

Next, since we know that our median recipe length is 30 minutes, we can see if longer recipes have any differences in nutrition compared to shorter recipes. An indicator variable for long (over 30 minutes) recipes is added to the dataframe.

|    |   minutes | long   |
|---:|----------:|:-------|
|  0 |        40 | True   |
|  2 |        40 | True   |
|  7 |        90 | True   |
|  9 |        20 | False  |
| 12 |        50 | True   |

<iframe
  src="assets/2-calories-hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This graph shows how calories differs between longer and shorter recipes. It seems that shorter recipes have less calories on average compared to longer recipes; long recipes are centered at roughly 200, while short recipes are centered at about 100. Both types of recipes thin out as the calorie count increases. Generally, it is unhealthy to consume too many calories, meaning that looking just at calories, both types of recipe seem healthy.

 However, calories alone tell us very little information about the actual nutrition of the recipe. Let's look at protein next.

<iframe
  src="assets/2-protein-hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Just like with calories, shorter recipes seem to have lower protein as well on average. Short recipes are centered at the 0-4 bin, while long recipes are centered at the 5-9 bin. However, there are still a comparable amount of short recipes with high protein. This means that while long recipes can be seen as more healthy on average, there are still healthy short recipes.

<iframe
  src="assets/2-sodium-hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Finally, we can look at the sodium content of the recipes. While both short and long recipes are centered at the 0-4 bin, it seems that a much larger amount of short recipes have low sodium compared to long recipes. Sodium is generally regarded as unhealthy in high quantities, giving some evidence that shorter recipes are healthier.

Overall, short recipes seem to have less nutrients compared to long recipes, however the healthiness of this depends on what nutrient is being examined. We would need to test if this difference is significant.

### Bivariate Analysis

For our bivariate analysis, we can see the rating of a recipe has any relationship with how long it takes to prepare.

<iframe
  src="assets/2-minutes-hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

There is a noticeable decrease in time taken as the rating increases. The median time decreases from 35 minutes to 30 minutes from 2 stars to 3 stars. The 1st and 3rd quartiles seem to also show this change.

### Interesting Aggregates

Below is a table showing the average of each column for each rating. There is no consistent pattern as the ratings increase, though the most beneficial value, such as the minimum for minutes or maximum for protein, happen at higher ratings. For example, the maximum average protein is for 4 star ratings.

|   rating |   minutes |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbs |
|---------:|----------:|-----------:|------------:|--------:|---------:|----------:|----------------:|--------:|
|        1 |     36.95 |     228.91 |       16.02 |   34.68 |    13.43 |     16.6  |           20.36 |    7.94 |
|        2 |     37.29 |     247.29 |       16.98 |   33.12 |    14.38 |     19.77 |           21.24 |    8.48 |
|        3 |     36.14 |     244.15 |       17.16 |   29.79 |    14.78 |     20.81 |           20.76 |    7.84 |
|        4 |     34.97 |     250.65 |       17.64 |   28.57 |    15.47 |     22.06 |           20.96 |    7.9  |
|        5 |     34.38 |     242.2  |       17.74 |   29.88 |    14.79 |     19.85 |           21.44 |    7.49 |


## Assessment of Missingness

There are only 4 columns in the dataset with missing values.

|                | Missing Count |
|:---------------|-----:|
| description    |   94 |
| rating         | 8647 |
| review         |   27 |
| average_rating | 1520 |

### MNAR Analysis
The `rating` column is most likely MNAR. In the introduction, all of the ratings of 0 were replaced with `np.nan`, since a value of 0 meant the user did not leave a review. However, people tend to not review/rate something if they didn't have an extremely bad or extremely good experience with it. Thus, it could be that ratings are more likely to be missing if they are closer to the 3-4 star range, as opposed to 1 star or 5 stars.

This column could also be MAR and dependent on other columns like `minutes`, as extremely long recipes would likely cause a bad review. This could push it out of the middle range of ratings and thus make it more likely for someone to leave a review in the first place.

### Missingness Dependecy

Let's run a permutation test to find a column that `rating`'s missingness is dependent on. I will choose the `protein` column, as protein is quite important to healthy nutrition.
* Null hypothesis: The mean protein value is the same for when `rating` is missing and when `rating` is not missing
* Alternative hypothesis: The mean protein value is different for when `rating` is missing and when `rating` is not missing
* Test statistic: Absolute difference in mean protein count in grams

Below is the graph comparing `protein` values for both missing ratings and non-missing ratings.

<iframe
  src="assets/3-protein-missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

There appears to be some differences, which the permutation test captured. The test returned a p-value of $0.0$, which is lower than any reasonable significance level. This means that we reject the null hypothesis; we have convincing evidence that the mean protein value is different between missing `rating` and non-missing `rating`. Thus, the missingness of `rating` is dependent on `protein`.

---

We can also look at the `calories` column. We need to run another permutation test to see if the missingness of `rating` is dependent on `calories` as well.
* Null hypothesis: The mean calorie count is the same for when `rating` is missing and when `rating` is not missing
* Alternative hypothesis: The mean calorie count is different for when `rating` is missing and when `rating` is not missing
* Test statistic: Absolute difference in mean calorie count

Below is the graph comparing the values of `calories` for both missing ratings and non-missing ratings.

<iframe
  src="assets/3-calories-missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Once again, there seems to be some differences, but the permutation test failed to reject the null hypothesis. The p-value of $0.56$ was higher than the significance level of $0.05$, so we found no convincing evidence that the mean calorie count is different for when `rating` is missing versus not missing. This means that the missingness of `rating` is not dependent on `calories`.

## Hypothesis Testing

The aim of this project was to identify a relationship between preparation difficulty and overall recipe healthiness. Thus, it would be optimal to see if easier, healthier recipes also had better ratings. This is very important in this context because many healthy dishes are characterized as bland or unlikeable.

These easier, healthier recipes will be defined as "good" if the recipe takes less than 30 minutes to prepare and has at least 30 grams of protein. The protein cutoff is determined based my personal body weight and level of exercise; this may vary for different people.

Thus, we will be running a permutation test with the following hypotheses:
* Null hypothesis: There is no difference in mean ratings between good recipes and not good recipes
* Alternative hypothesis: Good recipes have higher mean ratings than not good recipes
* Test statistic: Difference in means, $\text{good}-\text{not good}$

Below is a graph of the results of the permutation test.

<iframe
  src="assets/4-perm-test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since our p-value of $0.1216$ is higher than our significance level of $0.05$, we fail to reject the null hypothesis. We don't have convincing evidence that the mean rating of good recipes is higher than the mean rating of not good recipes.

## Framing a Prediction Problem

Our prediction problem will be involving attempting to predict the minutes taken to prepare a recipe, which is a regression problem. Most of the information in our dataframe is available at this time, as the ingredients, steps, etc are known beforehand. The only information that is unavailable would be the review and rating, as it is not reasonable for someone to review or rate a recipe that they haven't made yet.

The response variable is `minutes`, as this is the biggest factor that affects the difficulty of a recipe. Thus, it would be helpful for a user to know how long a recipe takes before they make it, while still ensuring they can get the proper nutrition that they need.

We will be evaluating our model based on the $R^2$ values, as it gives a decent sense of model performance since it represents the amount of variability that our model is capturing. It is also easy to understand since it operates on a 0 to 1 scale, with higehr values being "better".

## Baseline Model

Our baseline model is a linear regression model with only three features:
* `n_steps`, the number of steps to prepare the recipe. This is quantitative and is left unencoded, as it is numerical and has no other features to need to be directly compared to.
* `protein`, the amount of protein in grams. This is quantitative and has been standardized, as different people have different protein needs. The standardization also allows for direct comparison with the other nutrition values.
* `sugar`, the amount of sugar in grams. This is also quantitative and has also been standardized, though mostly for comparison purposes. Since the amount of each nutrient can vary depending on what the nutrient is, standardization allows for direct comparison.

The $R^2$ value of this model was roughly 0.19 for the training data, meaning that our model captures roughly 19% of the variability in `minutes`. On the test data, the $R^2$ is now about 0.18. We believe that this is not a good model, as a vast majority of the variability is unaccounted for. We would want an $R^2$ of at least 0.5.

## Final Model

For our final model, we switched from a lienar regression model to a random forest regression model, as decision trees and forests are more resilient to unhelpful features and train faster. This is useful because our dataframe has over 100,000 rows.

Next, we added multiple features:
* The remaining nutrition information values, as they are essentially the deconstructed version of the ingredients. Having more ingredients and having more complex ingredients would both affect the recipe preparation time, so they are both helpful for prediction.
* `len_steps`, the total length of the steps. This is helpful because the steps can be arbitrarily assigned by the recipe creator, so they aren't necessarily representative of the total amount of tasks that must be done to make the recipe.

---

We used `GridSearchCV` to identify the best maximum depth to use for our random forest. Since random forests are prone to overfitting if left uncapped, this hyperparameter is very important. The other major hyperparameters, `n_estimators`, was set to 10 to avoid excessively long training times. 





## Fairness Analysis

Finally, we will analyze the fairness of the model between well reviewed recipes, which have 5 stars, and poorly reviewed recipes, which have 4 stars or less. We will be evaluating fairness by comparing the Root Mean Squared Error between these groups. 

This will be done via permutation test:
* Null hypothesis: The model has equal RMSE between well reviewed recipes and poorly reviewed recipes.
* Alternative hypothesis: The model has better RMSE for well reviewed recipes versus poorly reviewed recipes.
* Test statistic: Difference in RMSE, well reviewed - poorly reviewed
