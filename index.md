# Recipes Analysis
By Emily Lin, linemily@umich.edu
---
title: Under Construction
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
I merged the "recipes" and "interactions" datasets by column, keeping recipes and dropping the columns 'date', 'user_id', and 'reviews' (these columns contained user-specific rather than recipe-specific data and were not relevant to my analysis), and added an additional column called 'avg_rating' that averaged all ratings for a given recipe. I also chose to drop all recipes that had a rating of "NaN" (recipes that had no rating), as well as a fake recipe called "How to Preserve a Husband" (since it isn't actually a makeable recipe, I believed it would adversely influence the integrity of the final analysis to include it). Approximately 2.6k recipes had no rating, which only comprised 3.1% of the dataset, so I believe the impact of dropping such recipes is negligible. The total number of rows remaining in the final dataframe was equal to 81172.

I then hypothesized that the type of meal (breakfast, lunch, dinner, dessert), food group(s) (which I divided into fruits, vegetables, meat, grains, dairy, and sauces), and calorie count that each recipe had would play a role in how the recipe was rated, so for the first two categories, I extracted the corresponding tags from the 'tags' field in the dataset and created new columns for each category that used binary coding depending on if the recipe had the corresponding tag or not (so for example, if a recipe was tagged 'breakfast' then it would have a 1 under the newly-created 'breakfast' column). I also extracted the nutritional information from the tag and created new columns for each field, although I only used the 'calories' field/column in the end. 

(head of cleaned DF goes here)
...

Embed at least one plotly plot you created in your notebook that displays the distribution of a single column (see Part 2: Report for instructions). Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present, and how they answer your initial question. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one univariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

...

The vast majority of recipes earned a 5 rating, meaning that there is likely to be less data/less reliable data showing what sort of recipes receive very low ratings (below 3 stars).
(graph goes here)

...

Embed at least one plotly plot that displays the relationship between two columns. Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present and how they answer your initial question. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one bivariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

....



---

## Framing a Prediction Problem

---

## Baseline Model

---

## Final Model

---
This was a project for EECS 398: Practical Data Science at the University of Michigan, Winter 2025.