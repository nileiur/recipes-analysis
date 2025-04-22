# What Factors Influence Recipe Ratings?

By Emily Lin, linemily@umich.edu
---
title: What Factors Influence Recipe Ratings?
description: Analysis of a recipes dataset.
---

## Introduction

Provide an introduction to your dataset, and clearly state the one question your project is centered around. Why should readers of your website care about the dataset and your question specifically? Report the number of rows in the dataset, the names of the columns that are relevant to your question, and descriptions of those relevant columns.

------

I analyzed a dataset of recipes from food.com (called "recipes" from here on out) as well as interactions with these recipes (data about ratings/reviews left by readers, called "interactions" from here on out), downloadable here (maybe). I was curious to see if there were any characteristics influenced whether a certain recipe received a higher or lower rating; this would be helpful for anyone who was casually browsing food.com and wanted highly rated recipes. The total number of rows in the original datasets were 83782 in "recipes" and 731927 in "interactions", and the columns I used for my analysis were:

- 'avg_rating': The average rating of a given recipe. This column was not present in the original data but was created in the process of cleaning the data (see next section)

- 'tags': user-generated tags for a given recipe; I extracted data about meal type from this field.

- 'n_steps': Number of steps in the recipe.

- 'nutrition': Contains nutritional facts about each recipe. I specifically used calorie data.

---

## Data Cleaning and Exploratory Data Analysis

Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions).
...
I merged the "recipes" and "interactions" datasets by column, keeping recipes and dropping the columns 'date', 'user_id', and 'reviews' (these columns contained user-specific rather than recipe-specific data and were not relevant to my analysis), and added an additional column called 'avg_rating' that averaged all ratings for a given recipe. I also chose to drop all recipes that had a rating of "NaN" (recipes that had no rating), as well as a fake recipe called "How to Preserve a Husband" (since it isn't actually a makeable recipe, I believed it would adversely influence the integrity of the final analysis to include it). Approximately 2.6k recipes had no rating, which only comprised 3.1% of the dataset, so I believed the impact of dropping such recipes was negligible. The total number of rows remaining in the final dataframe was equal to 81172.

I then hypothesized that the type of meal (breakfast, lunch, dinner, dessert), food group(s) (which I divided into fruits, vegetables, meat, grains, dairy, and sauces), and calorie count that each recipe had would play a role in how the recipe was rated, so for the first two categories, I extracted the corresponding tags from the 'tags' field in the dataset and created new columns for each category that used binary coding depending on if the recipe had the corresponding tag or not (so for example, if a recipe was tagged 'breakfast' then it would have a 1 under the newly-created 'breakfast' column). I also tried to include related tags under a given meal type/food group as well (for example, 'beef' was counted as 'meat'). I also extracted the nutritional information from the tag and created new columns for each field, although I only used the 'calories' field/column in the end. 

(head of cleaned DF goes here)
...

Embed at least one plotly plot you created in your notebook that displays the distribution of a single column (see Part 2: Report for instructions). Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present, and how they answer your initial question. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one univariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

...

After cleaning the data, I performed EDA on the recipes dataset. I analyzed the distributions of the 'avg_ratings" and 'n_steps' columns and made the following observations: 

The vast majority of recipes earned a 5 rating, meaning that there is likely to be less data/less reliable data showing what sort of recipes receive very low ratings (below 3 stars).
(graph goes here)

Below is a graph of the distribution of the 'n_steps' column. We see a strong right skew with most recipes being between 3-20 steps, so this indicated that it might be worth binning the number of steps to make a recipe, with larger bin lengths for higher numbers of steps.
(graph goes here)

...

Embed at least one plotly plot that displays the relationship between two columns. Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present and how they answer your initial question. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one bivariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

....

I then opted to investigate the relationship between the number of steps required to make a recipe and its average rating, as well as the calorie count of a recipe vs its average rating.

When investigating the relationship between steps and average rating, I opted to sort the data into bins loosely based on the distribution of steps as shown in the graph above. I then produced the graph below, which illustrates the aforementioned relationship. We observe that the median does not change between the different bins, but recipes with 5-10 steps and 10-20 steps have a slightly wider spread of values. 

(graph)

I did something similar when investigating the relationship between calories and average rating, binning calories based on the fact that most recipes had around 350-450 calories (the mean number of calories across recipes was 427, while the median was 305). When plotting, I observed that recipes with 250-650 calories had a slightly wider spread than recipes outside this calorie range.

(graph)

...

***pivot table***
I also wanted to investigate if there was a correlation between meal type and number of steps vs. average rating. The results are shown in the pivot table below; we see that very complex recipes tend to receive slightly higher ratings, and desserts tend to receive slightly lower ratings (at least when they require 6-20 steps to make)

(pivot table)

...

I chose not to impute any missing values. A number of recipes had calorie counts of zero, but had no nutritional information besides sodium amounts, so there was not enough information present to estimate and impute a calorie count. I also considered imputing missing values based on meal type/food group, but since calories can differ greatly between dishes in the same food group and/or emal type, I decided that it ultimately was not beneficial enough impute the calorie counts.

---

## Framing a Prediction Problem

Clearly state your prediction problem and type (classification or regression). If you are building a classifier, make sure to state whether you are performing binary classification or multiclass classification. Report the response variable (i.e. the variable you are predicting) and why you chose it, the metric you are using to evaluate your model and why you chose it over other suitable metrics (e.g. accuracy vs. F1-score).

Note: Make sure to justify what information you would know at the “time of prediction” and to only train your model using those features. For instance, if we wanted to predict your Final Exam grade, we couldn’t use your Final Project grade, because we (probably) won’t have the Final Project graded before the Final Exam! Feel free to ask questions if you’re not sure.

....

I chose to investigate what types of characteristics determine whether a recipe earns a higher average rating or not, and chose to frame it as a regression problem. Thus, the response variable I chose is 'avg_rating' or average rating of a given recipe, and I chose to evaluate the quality of my model with test MSE since I want to see how the model generalizes to unseen data, and test MSE is an excellent predictor for this.

---

## Baseline Model

Describe your model and state the features in your model, including how many are quantitative, ordinal, and nominal, and how you performed any necessary encodings. Report the performance of your model and whether or not you believe your current model is “good” and why.

Tip: Make sure to hit all of the points above: many Final Projects in the past have lost points for not doing so.
...

I initially used a linear regression model, and used all of the following features:

1) 'n_steps'

2) 'calories'

3) 'breakfast'

4) 'lunch'

5) 'dinner'

6) 'desserts'

7) 'meat'

8) 'vegetables'

'n_steps' and 'calories' are both quantitative, and the remaining features are nominal. All features except the first two were binarily encoded; this was performed during the initial data cleaning when I manually created columns corresponding with these features. I fit the model with 5-fold cross validation, and ended up with a test MSE of 0.41129.

I don't believe that this model is currently very good at predicting average ratings; our preliminary analysis showed that there was not a huge correlation between number of steps or calories vs average rating, and for the nominal variables, many recipes don't necessarily fall neatly into one category of meal type or food group. Also, since all recipes are user-submitted, some users may have tagged their recipes appropriately while others may not have; I tried to group all tags I could think of (such as counting 'beef' or 'chicken' under 'meat') but some recipes may simply be missing the appropriate tags.

---

## Final Model

State the features you added and why they are good for the data and prediction task. Note that you can’t simply state “these features improved my accuracy”, since you’d need to choose these features and fit a model before noticing that – instead, talk about why you believe these features improved your model’s performance from the perspective of the data generating process.

Describe the modeling algorithm you chose, the hyperparameters that ended up performing the best, and the method you used to select hyperparameters and your overall model. Describe how your Final Model’s performance is an improvement over your Baseline Model’s performance.
...

I decided to stick with linear regression, but engineered the features in several different ways in an attempt to improve model accuracy, as follows:

1) Included polynomial features: I believe this improved the model by allowing it to capture non-linear trends in the data. I also wanted to see if capturing some of the interactions between features would improve the performance of the model or not as well.

2) Scaled variables with StandardScaler: The inclusion of higher degrees can cause predicted y values to blow up for large values for x, so I believe that scaling the variables improved model performance by mitigating this effect.

3) Used target encoding on nominal variables: instead of binarily encoding meal type/food group, I opted to use the mean ratings of each meal type/food group. This allows us to directly correlate mean ratings between meal type/food groups with the predicted average rating, which could have helped capture the actual relationship between ratings and meal type/food groups.

4) Incorporated regularization into the model using ridge regression: Ridge regression shrinks the coefficients of less significant features towards zero to prevent overfitting. Given how high the optimal alpha penalty ended up being, I believed this played the greatest role in improving my model.

I also created a grid with the following hyperparameters:
1) Polynomial Features: I decided to check degrees 1, 2, and 3 (I hypothesized that any higher values would likely lead to overfitting), and had 2 parameters to check whether including interactions would improve model performance or not.

2) Regularization: I originally only tried alpha values 0.01, 0.1, 1, 10, 100. When I initially ran the model, I noticed that GridSearchCV chose 100 as the best alpha value, so I tried an array that included higher alpha values and found that it chose 1000 as the best alpha value (which probably means that many of the features I chose did not actually contribute to the accuracy of the model).

Ultimately I used GridSearchCV to fit the model with 5-fold cross validation, as before. GridSearchCV found that the best hyperparameters were as follows:

1) Polynomial degree: 2

2) Interactions: False (so not considering interactions led to better performance than taking them into account)

3) Alpha for regularization: 1000

I saw a very small improvement in the model; test MSE went from 0.41129 in the baseline model to 0.41114 in the final model. While this is a step in the right direction, there is definitely still room for improvement, and it might be worth reexamining what features I want to include in the model in the future.

---
This was a project for EECS 398: Practical Data Science at the University of Michigan, Winter 2025.