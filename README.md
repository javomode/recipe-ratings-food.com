Project for DSC 80 at UCSD. Takes a look at recipes and reviews from users from food.com, and performs data cleaning, EDA, and simple linear regression modeling with data visualizations. 

The website for this project is available at: http://javomode.github.io/recipe-ratings-food.com/

## Introduction:
With thousands of online recipes available, choosing the right one can be overwhelming. This project explores what makes a recipe likely to receive a **high average user rating**, helping people cook smarter and avoid disappointing meals.

I focus on a large dataset from food.com, containing over **230,000 recipes**. My goal is to understand how features like **cooking time**, **number of steps**, and **number of ingredients** relate to a recipe’s rating.

Before dropping and selecting specific values, the dataset that will be used contains 234429, with some relevant columns being:
* 'minutes': the number of minutes a recipe calls for to be made
* 'tags': tags that the author of the recipe used to describe their recipe  
* 'n_steps': the number of steps the recipe uses
* 'n_ingredients': the number of ingredients in the recipe
* 'rating': the rating a single user gave a recipe

## Data Cleaning
1. I merged two datasets, one with recipes and one with user interactions to the recipes. 
2. I replaced ratings of 0 (which indicated no rating was given) with `NaN`, 
	* I replace ratings of 0 with `NaN` because some reviews do not have a rating, yet still receive a 0 as a rating. 
	* I also replace ratings of 0 because it is not possible to leave a 0 star review. 
	* This is important because the lack of a rating is different from the meaning of a rating of 0 stars.
		* If ratings of 0 are left in when they are not possible, then it may skew averages and many other metrics if not replaced.
		* A main reason is that 0 has a meaning, especially when this dataset focuses on looking at predicting rating on certain features.
			* A rating *could* range from 0-5, but food.com only allows full star ratings, from 1-5.
3. I calculated the average rating per recipe after replacing zeroes. 
4. I formatted the nutrition, ingredients, and tags columns to make them easier to work with. 
	* For example, nutrition values were converted from raw strings that looked like lists, actual lists of strings:

Here, I provide the first 3 rows of the dataset I work with, not showing the description, tags, and review columns, all of which can be extensive in length and make it difficult to display the data.

| name                                 |     id |   minutes |   contributor_id | submitted   | nutrition                                                                                                                                    |   n_steps |   n_ingredients |   user_id |   recipe_id | date       |   rating |   avg_rating |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------|----------:|----------------:|----------:|------------:|:-----------|---------:|-------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['calories (138.4)', 'total fat (10.0)', 'sugar (50.0)', 'sodium (3.0)', 'protein (3.0)', 'saturated fat (19.0)', 'carbohydrates (6.0)']     |        10 |               9 |    386585 |      333281 | 2008-11-19 |        4 |            4 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['calories (595.1)', 'total fat (46.0)', 'sugar (211.0)', 'sodium (22.0)', 'protein (13.0)', 'saturated fat (51.0)', 'carbohydrates (26.0)'] |        12 |              11 |    424680 |      453467 | 2012-01-26 |        5 |            5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['calories (194.8)', 'total fat (20.0)', 'sugar (6.0)', 'sodium (32.0)', 'protein (22.0)', 'saturated fat (36.0)', 'carbohydrates (3.0)']    |         6 |               9 |     29782 |      306168 | 2008-12-31 |        5 |            5 |

### Univariate Analysis

<iframe
  src="assets/most_ingredients.html"
  width="1000"
  height="800"
  frameborder="0"
></iframe>

This bar plot shows the most frequently used ingredients across all recipes in the dataset. Unsurprisingly, common staples like salt, butter, and sugar appear most often, reflecting their widespread use in a variety of recipes.

<iframe
  src="assets/least_tags.html"
  width="1000"
  height="800"
  frameborder="0"
></iframe>

This bar plot displays the least frequently used tags in the dataset. These rare tags represent niche or highly specific recipe categories and ingredients, a particular one being: 

Throw the ultimate fiesta with this sopaipillas recipe from Food.com

which is no longer even a tag anymore, but an entire description. The specificity and uniqueness of these tags highlight less common culinary themes or specialized dishes.

### Bivariate Analysis

<iframe
  src="assets/cooking_time_vs_rating.html"
  width="1000"
  height="800"
  frameborder="0"
></iframe>

This graph details the average rating by binned amounts of minutes that recipes called for. In the notebook, I also plotted number of ingredients against average rating, as well as number of ingredients against rating.

Across all of these graphs, there is no alarming/interesting trend and they all look very similar, besides the fact that across all of these distributions, the average ratings for all numbers of steps, numbers of ingredients, or minutes a recipe took lied around ~4.6 - 4.7.

### Interesting Aggregate

|   n_ingredients |   1.0 |   2.0 |   3.0 |   4.0 |    5.0 |
|----------------:|------:|------:|------:|------:|-------:|
|               1 | nan   |   5   |   5   |  38.4 |   38.6 |
|               2 | 167.1 | 153.7 |  96.9 | 136.9 | 1617.3 |
|               3 |  64.6 | 113   |  69.2 | 378.5 |  144.6 |
|               4 | 187.2 |  39.7 |  57.9 |  64.8 |  248.9 |
|               5 |  81.4 |  63.4 |  62.3 |  59   |   61.8 |

Here, I used a pivot table whereset the index to the number of ingredients, the columns to the ratings, and the values to the average minutes, and showed only the top 5 rows. This shows the average cooking time for recipes with a given number of ingredients and rating. Under the 5 star rating, there is an obscenely large value of 1617.3 minutes, which suggests there may be outliers of recipes that take large amounts of time to make.

<iframe
  src="assets/pivot_ingredients_by_rating.html"
  width="1000"
  height="800"
  frameborder="0"
></iframe>

This graph visually shows the pivot table, where for the most part, most of the data makes sense, besides the fact that there is a recipe that takes 37 ingredients, and that there are recipes that are skewing the data with extremely large outliers in the minutes column.

## Assessment of Missingnesss

I believe some columns in this dataset may be NMAR:
* description may be NMAR because users who are less confident or disinterested might skip writing one.
* rating could be NMAR if users avoid rating bad recipes rather than leaving low scores.
* reviews might be NMAR since users often leave comments only when they feel strongly-either very positively or negatively.

To assess whether these are truly NMAR or just MAR, there would need to be more data on user behavior, such as whether they viewed or made the recipe without leaving feedback.

### Missingness Dependency
I explored whether the missingness of the rating column depends on the minutes column, which would suggest it is not missing completely at random.

Permutation Test Setup:

Missingness indicator: Created a binary column where 1 indicates a missing rating, and 0 indicates a present value.

### Hypotheses

* Null hypothesis (H₀): The missingness of rating is independent of minutes.
* Alternative hypothesis (H₁): The missingness of rating is dependent on minutes.

### Test statistic

mean(minutes when rating is missing) − mean(minutes when rating is present)

### Procedure

1. Permuted the missingness indicator multiple times.
2. Computed the test statistic for each permutation.
3. Compared the observed difference to the empirical distribution of permuted differences using a two-tailed test at a significance level of 0.05.

### Plot

This plot shows the distribution of permuted differences in minutes, with the red line marking the observed difference. Since the observed value lies outside the permuted range but the p-value is not significant, the relationship between missingness of rating and minutes is inconclusive.

<iframe 
	src="assets/permutation_missing_rating_vs_minutes.html" 
	width="1000" 
	height="800" 
	frameborder="0" 
></iframe>

### Results

The empirical distribution of permuted differences clusters around zero, and the observed difference falls outside the range of permuted values. However, the corresponding p-value is approximately 0.122, which is not statistically significant at the 0.05 level.

Therefore, there is no strong evidence that the missingness of rating depends on the minutes column. This suggests that missingness in rating is not related to minutes and could be either Missing Completely At Random (MCAR) or Not Missing At Random (NMAR) with respect to other variables.

## Hypothesis Testing

Question: Is there a relationship between calories and rating?

### Hypotheses

* Null hypothesis (H₀): There is no linear correlation between recipe calories and average rating (ρ = 0).
* Alternative hypothesis (H₁): There is a linear correlation between recipe calories and average rating (ρ ≠ 0).

These hypotheses are appropriate because we are specifically testing whether a linear relationship exists between two numerical variables-calories and rating.

### Test Statistic

I used Pearson’s r as the test statistic. This measures the strength and direction of a linear relationship between two continuous variables. It is a widely used and interpretable measure of linear association, making it a good choice when a linear relationship is plausible.

### Procedure

1. Computed the observed Pearson’s r between calories and rating.

2. Shuffled the rating column to simulate the null hypothesis of no relationship.

3. Recomputed Pearson’s r for each permutation.

4. Repeated this process many times (e.g., 1000 iterations) to build a null distribution.

5. Calculated the p-value as the proportion of permuted correlations that are as or more extreme than the observed one.

5. Used a significance level of α = 0.05 for decision-making.

### Plot

<iframe
  src="assets/permutation_calorie_vs_rating.html"
  width="1000"
  height="800"
  frameborder="0"
></iframe>

Provided is also a plot, showing the distribution of the permuted values, with the dashed red lines showing the negative and positive observed r-values.

### Results

* Observed Pearson’s r: -0.009
* P-value: 0.0005

Since the p-value is greater than 0.05, we fail to reject the null hypothesis. This means that we do not have strong evidence of a significant linear relationship between calories and average rating in this dataset.

This does not prove that no relationship exists, but rather that the observed linear correlation could plausibly have arisen by chance under the null hypothesis.

## Prediction Problem

### Prediction Problem and Type

I aim to predict a recipe’s rating based on basic properties of the recipe. This is a regression problem, since the response variable-rating, is continuous.

### Response Variable: 
* rating
* Rating is a key indicator of a recipe's success and user satisfaction. Predicting ratings allows us to estimate user preferences for recipes even before they receive reviews.

### Model Type: 
* Linear Regression
*Linear regression is a simple, interpretable baseline model that assumes a linear relationship between the features and the target.

### Features Used for Prediction:

The following features are available at the time a recipe is posted, which makes them valid predictors:

* minutes: The total time required to prepare the recipe.
* n_ingredients: The number of ingredients, as a proxy for complexity.
* n_steps: The number of steps in the recipe instructions.

These are all features that:

1. Are available before any user ratings exist, and
2. Offer interpretable, numeric inputs that may influence a user’s decision to rate highly or not.

### Features not used:

* ID columns (e.g., user_id, recipe_id) are ignored because their numeric values do not have meaningful or predictive relationships with the rating.
* Dates (submitted, review_date) are excluded because they are either not available at the time of recipe creation or offer little predictive value without additional context.

### Time of Prediction Justification:

All selected features are known at the time the recipe is created and posted. This respects the causality principle-no information from future events (like review behavior or user feedback) is used during model training.

### Evaluation Metric
* Metric: RMSE (Root Mean Squared Error)
* RMSE penalizes large errors more heavily than small ones, making it sensitive to significant deviations.
* It provides a clear interpretation in the same units as the response variable (ratings), making it easier to explain model performance.

## Model Description & Results

| Feature | Type | Description |
| ----------- | ----------- |
| minutes | Quantitative | Represents total preparation time. Due to extreme outliers and skew, this feature was log-transformed using log(minutes + 1) |
| n_ingredients | Quantitative | The number of ingredients in the recipe, representing potential complexity |
| n_steps | Quantitative | The number of preparation steps, also used as a measure of complexity |

There are no ordinal or nominal features in this baseline model, so no encoding was required. Only a standard scaler was used on all three features, as this baseline model did not use any other categorical features.

### Model Performance

RMSE = 0.7139

This means that, on average, the model's predicted rating differs from the actual rating by approximately 0.71 stars. Given that ratings are typically measured on a 1 to 5 scale, this error is rather large, suggesting there is a lot of room for improvement.

### Evaluation

The current model provides a basic approximation of ratings using simple, numeric recipe characteristics. While useful as a baseline, the model does not capture more complex patterns or higher-order interactions.

### Ideas for Improvement

* Categorical Time Buckets: The minutes feature may not linearly influence rating. Grouping it into cooking time categories (short, medium, long) could capture user preferences better than the raw or log-transformed numerical values.
	* Short: < 30 minutes
	* Medium: 30 minutes to 3 hours
	* Long: > 3 hours

* Recipe Complexity: Instead of using n_ingredients and n_steps directly, a derived feature representing overall complexity could be created. For example:
	* Easy: score < 20 (where score = n_ingredients + n_steps)
	* Medium: 20–40
	* Hard: > 40

### Nutrition Features

Although detailed nutrition information is present, it is difficult to interpret without serving size information. Future work could involve engineering features like macro ratios (e.g., fat-to-protein, sugar-per-calorie), though this must be done carefully to avoid misinterpretation.

### Planned Next Steps
* Transform minutes into a categorical variable to capture rough distinctions in preparation time.
* Explore feature engineering based on complexity.
* Compare these enhanced models against the baseline using RMSE for consistency.

## Final Model and Feature Enhancements

### New Features and Rationale

To improve the baseline model, I added:

* time_category (short, medium, long): captures non-linear effects of preparation time
* complexity (easy, medium, hard): summarizes recipe difficulty based on steps and ingredients

These features were made with the idea that they would be more reflective of how users likely rate recipes, based on convenience and complexity, rather than raw numerical values alone.

### Model and Evaluation

Model: Random Forest Regressor

Why Random Forest: This model handles non-linear relationships and feature interactions well without requiring explicit transformations. It was a better fit for capturing complex relationships in recipe data than linear regression.

* Before I had decided to use the random forest regressor, I had tried to use a lasso regression, which showed that the coefficients for time_category and complexity were both zero when the model was predictinng rating linearly from the featuers.
* The change to random forest was in response to finding that there was not a strong linear relationship.

### Encoding: 
* Original standard scaler to quantitative features (minutes, n_ingredients, n_steps)
* One-hot encoding was applied to categorical features (time_category, complexity)

### Features Used:
* minutes
* n_ingredients
* n_steps
* time_category
* complexity

### Hyperparameter Tuning

I used randomized search with cross-validation to tune key hyperparameters (n_estimators, max_depth, etc.). This was more computationally efficient than grid search and still allowed for meaningful performance gains.
* Not only that, but grid search finds hyperparameters differently, in that it tries to use all possible combinations while randomized search only searchs a up to a specified number of possible combinations.

### Performance

* Baseline Linear Regression RMSE: 0.7139
* Final Random Forest RMSE: 0.7075

### Results

The Random Forest model, combined with engineered categorical features, improved predictive performance, though not by much, by only ~0.0064. The lower RMSE suggests it better captures the true structure of the data and how users rate recipes based on time and complexity.

## Fairness Analysis

To assess whether the final model (Random Forest Regressor, RMSE = 0.7075) treats all recipe types fairly, I conducted a permutation test comparing model performance on short versus long recipes.

### Group Definitions:

* Group X: Recipes with short cooking times (log-transformed minutes < median)
* Group Y: Recipes with long cooking times (log-transformed minutes ≥ median)

### Evaluation Metric:

RMSE: Root Mean Squared Error remains the evaluation metric, consistent with the rest of the analysis.

### Hypotheses:

* Null Hypothesis (H₀): The model performs equally for both groups. Any difference in RMSE is due to chance.
* Alternative Hypothesis (H₁): The model performs worse (higher RMSE) for long-time recipes than for short-time ones.

### Test Statistic

* RMSE<sub>long</sub> - RMSE<sub>short</sub>

A positive observed test statistic would suggest worse performance on longer recipes.

### Significance Level:

* α = 0.05

Provided is also a plot, showing the distribution of the permuted values, with the dashed red lines showing the negative and positive observed r-values.

<iframe 
	src="assets/fairness_permutation_RMSE_distribution.html" 
	width="1000" 
	height="800" 
	frameborder="0" 
></iframe>

This plot shows the distribution of RMSE differences under the null hypothesis that model performance is equal for short and long recipes. The red dashed line marks the observed RMSE difference, which lies far to the right of the permuted values, indicating significantly worse performance on long recipes.

### Results

The observed RMSE difference between long and short recipe groups was larger than all differences obtained through permutation, resulting in a p-value of 0.00. Since this is below the significance level of 0.05, we reject the null hypothesis.

This provides strong statistical evidence that the model performs worse on long recipes compared to short ones. In other words, there is an issue of fairness, as prediction error is not consistent across groups defined by recipe duration.

## Conclusion

In this project, I explored how recipe characteristics relate to user ratings and developed a regression model to predict recipe ratings using features that are available at the time a recipe is posted. The analysis began with an investigation into missing data, which suggested that missingness in the rating column was likely not missing completely at random, particularly with respect to preparation time.

After performing bivariate and univariate analyses, I framed a prediction problem using a regression approach, with rating as the response variable. Initial features included minutes, n_steps, and n_ingredients, and the baseline model used linear regression evaluated by RMSE. To improve predictive performance, I added engineered features such as categorized cooking time (short, medium, long) and a complexity score derived from steps and ingredients. These features better aligned with how users might perceive and rate recipes in the real world.

To better capture non-linear relationships and interactions between features, I transitioned to a Random Forest Regressor. This model improved performance, reducing the RMSE from 0.7139 to 0.7075. Hyperparameters were selected using randomized search, as grid search proved computationally intensive given the model complexity.

Finally, I conducted a fairness analysis using a permutation test to evaluate whether the model performed equally well across different types of recipes. The results revealed that the model exhibited worse performance on longer recipes, with a statistically significant difference in RMSE between short and long recipe groups. This highlights a potential fairness concern, as prediction error is not evenly distributed.

Future improvements could include incorporating more nuanced nutritional or ingredient-level data, such as serving size level nutritional information per recipe, or using more NLP processes to utilize more data from the dataset, such as performing sentiment analysis on user reviews.
