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

| name                                 |     id |   minutes |   contributor_id | submitted   | nutrition                                                                                                                                    |   n_steps |   n_ingredients |   user_id |   recipe_id | date       |   rating |   avg_rating |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------|----------:|----------------:|----------:|------------:|:-----------|---------:|-------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['calories (138.4)', 'total fat (10.0)', 'sugar (50.0)', 'sodium (3.0)', 'protein (3.0)', 'saturated fat (19.0)', 'carbohydrates (6.0)']     |        10 |               9 |    386585 |      333281 | 2008-11-19 |        4 |            4 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['calories (595.1)', 'total fat (46.0)', 'sugar (211.0)', 'sodium (22.0)', 'protein (13.0)', 'saturated fat (51.0)', 'carbohydrates (26.0)'] |        12 |              11 |    424680 |      453467 | 2012-01-26 |        5 |            5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['calories (194.8)', 'total fat (20.0)', 'sugar (6.0)', 'sodium (32.0)', 'protein (22.0)', 'saturated fat (36.0)', 'carbohydrates (3.0)']    |         6 |               9 |     29782 |      306168 | 2008-12-31 |        5 |            5 |


