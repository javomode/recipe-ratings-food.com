Project for DSC 80 at UCSD. Takes a look at recipes and reviews from users from food.com, and performs data cleaning, EDA, and simple linear regression modeling with data visualizations. 

The website for this project is available at: http://javomode.github.io/recipe-ratings-food.com/

## Introduction:
With thousands of online recipes available, choosing the right one can be overwhelming. This project explores what makes a recipe likely to receive a **high average user rating**, helping people cook smarter and avoid disappointing meals.

I focus on a large dataset from food.com, containing over **230,000 recipes**. My goal is to understand how features like **cooking time**, **number of steps**, and **number of ingredients** relate to a recipe’s rating.

Before dropping and selecting specific values, the dataset that will be used contains ==234429==, with some relevant columns being:
* 'minutes': the number of minutes a recipe calls for to be made
* 'tags': tags that the author of the recipe used to describe their recipe  
* 'n_steps': the number of steps the recipe uses
* 'n_ingredients': the number of ingredients in the recipe
* 'rating': the rating a single user gave a recipe
