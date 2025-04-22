# What Factors Influence Recipe Ratings?

By Emily Lin, linemily@umich.edu
---
title: What Factors Influence Recipe Ratings?
description: Analysis of a recipes dataset.
---

## Introduction

I analyzed a dataset of recipes from food.com (called "recipes" from here on out) as well as interactions with these recipes (data about ratings/reviews left by readers, called "interactions" from here on out), downloadable here (maybe). I was curious to see if there were any characteristics influenced whether a certain recipe received a higher or lower rating; this would be helpful for anyone who was casually browsing food.com and wanted highly rated recipes. The total number of rows in the original datasets were 83782 in "recipes" and 731927 in "interactions", and the columns I used for my analysis were:

- 'avg_rating': The average rating of a given recipe. This column was not present in the original data but was created in the process of cleaning the data (see next section)

- 'tags': user-generated tags for a given recipe; I extracted data about meal type from this field.

- 'n_steps': Number of steps in the recipe.

- 'nutrition': Contains nutritional facts about each recipe. I specifically used calorie data.

---

## Data Cleaning and Exploratory Data Analysis

I merged the "recipes" and "interactions" datasets by column, keeping recipes and dropping the columns 'date', 'user_id', and 'reviews' (these columns contained user-specific rather than recipe-specific data and were not relevant to my analysis), and added an additional column called 'avg_rating' that averaged all ratings for a given recipe. I also chose to drop all recipes that had a rating of "NaN" (recipes that had no rating), as well as a fake recipe called "How to Preserve a Husband" (since it isn't actually a makeable recipe, I believed it would adversely influence the integrity of the final analysis to include it). Approximately 2.6k recipes had no rating, which only comprised 3.1% of the dataset, so I believed the impact of dropping such recipes was negligible. The total number of rows remaining in the final dataframe was equal to 81172.

I then hypothesized that the type of meal (breakfast, lunch, dinner, dessert), food group(s) (which I divided into fruits, vegetables, meat, grains, dairy, and sauces), and calorie count that each recipe had would play a role in how the recipe was rated, so for the first two categories, I extracted the corresponding tags from the 'tags' field in the dataset and created new columns for each category that used binary coding depending on if the recipe had the corresponding tag or not (so for example, if a recipe was tagged 'breakfast' then it would have a 1 under the newly-created 'breakfast' column). I also tried to include related tags under a given meal type/food group as well (for example, 'beef' was counted as 'meat'). I also extracted the nutritional information from the tag and created new columns for each field, although I only used the 'calories' field/column in the end. 

'|  | id | avg_rating | review_count | name | minutes | contributor_id | submitted | tags | nutrition | n_steps | steps | description | ingredients | n_ingredients | recipe_id | calories | fat | sugar | sodium | protein | sat_fat | carbs | desserts | lunch | dinner | breakfast | grains | meat | dairy | fruit | vegetables | sauces | ratings_bin | rate_bin_label | bin_order | scores_bin | bin_label | ingredient_bins | steps_x_calories | breakfast_encoded | lunch_encoded | dinner_encoded | desserts_encoded | meat_encoded | vegetables_encoded | cscores_bin | cbin_label |\n| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |\n| 59451 | 424796 | 1.0 | 1 | gluten free wonton soup | 39 | 946146 | 2010-05-12 | [\'60-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'cuisine\', \'preparation\', \'for-1-or-2\', \'clear-soups\', \'soups-stews\', \'pork\', \'asian\', \'dietary\', \'gluten-free\', \'free-of-something\', \'meat\', \'number-of-servings\'] | [\'485.1\', \'37.0\', \'20.0\', \'53.0\', \'85.0\', \'44.0\', \'7.0\'] | 33 | [\'step 1: crock pot pork stock\', \'wash the meat\', \'place in crock pot\', \'cut the onion in quarters and add to the pot\', \'add the bay leaf and water\', \'cover with a lid and cook on high for 5 hours , or low for 8-10 hours\', \'remove the meat , onion , and bay leaf\', \'remove any excess fat from the meat\', \'save the meat for another meal\', \'skin any fat from the top of the broth and discard\', \'for the clearest broth , drain through cheesecloth\', \'let cool\', \'makes about 5 1 / 4 cups\', \'side\', \'to make chicken stock use 1 small frying chicken\', \'to make beef stock use 1 beef sirloin roast\', \'step 2: asian meatballs\', \'in a medium-sized bowl , combine all the ingredients\', \'shape into meatballs , using approximately 1 rounded teaspoon per meatball\', \'to eat these on their own place in a lightly greased pan over medium heat\', \'cook until no pink remains , 3-5 minutes\', \'for the wonton soup , leave them uncooked\', \'makes 20-25 meatballs\', \'step 3: wonton soup\', \'heat the broth and salt to a boil in the saucepan\', \'add the noodles and meatballs\', \'cook until the noodles are tender and the meatballs are cooked through , between 3 and 5 minutes\', \'garnish with green onion\', \'serve hot\', \'a\', \'mine took closer to 10maybe cause they were a bit bigger than directed\', \'so , about halfway through i added the noodles\', \'end result was perfect\'] | another fabulous roben ryberg recipe.  a 3 stage process that is totally worth the effort.  note that cooking time includes making stock in the crock pot. | [\'pork sirloin roast\', \'onion\', \'bay leaf\', \'water\', \'ground pork\', \'shrimp\', \'sugar\', \'soya sauce\', \'green onions\', \'salt\', \'pork stock\', \'rice noodles\', \'meatballs\', \'green onion\'] | 14 | 424796.0 | 485.1 | 37.0 | 20.0 | 53.0 | 85.0 | 44.0 | 7.0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | [1, 2) | [1, 2) | 1 | [20, 40) | [20, 40) | Complex (13-20) | 16008.300000000001 | 4.626862949190772 | 4.624336682890608 | 4.626862949190772 | 4.633423192812037 | 4.613325174732103 | 4.619304112688417 | [450, 550) | [450, 550) |\n| 64358 | 442044 | 1.0 | 1 | buttermilk mashed sweet potatoes  rachael ray | 20 | 238971 | 2010-11-15 | [\'30-minutes-or-less\', \'time-to-make\', \'course\', \'preparation\', \'occasion\', \'low-protein\', \'healthy\', \'5-ingredients-or-less\', \'side-dishes\', \'easy\', \'holiday-event\', \'kid-friendly\', \'low-fat\', \'dietary\', \'thanksgiving\', \'low-sodium\', \'low-cholesterol\', \'low-saturated-fat\', \'low-calorie\', \'healthy-2\', \'toddler-friendly\', \'low-in-something\', \'3-steps-or-less\'] | [\'321.1\', \'1.0\', \'107.0\', \'9.0\', \'12.0\', \'1.0\', \'24.0\'] | 7 | [\'wash and peel the sweet potatoes , then cut them into 1 1 / 2-2-inch chunks\', \'place them into a large pot and cover them with cold water\', \'place the pot on the stove , add some salt and turn the heat on high to bring to a boil\', \'once boiling , cook the potatoes for about 8 minutes , until they are tender but not falling apart\', \'drain the potatoes thoroughly , then pop them back into the pot and place them over medium heat to dry them out a bit\', \'add the buttermilk , pepper and some salt , to taste\', \'using a potato masher , mash up the sweet potatoes until smooth add syrup and serve them\'] | i am putting together a weight watcher\'s thanksgiving.  i found this on rachael ray\'s yum-o site. i will be using pure maple syrup as it has less sugar content than the traditional sugars used in sweet potatoes. this sounds great! | [\'sweet potatoes\', \'buttermilk\', \'maple syrup\', \'black pepper\'] | 4 | 442044.0 | 321.1 | 1.0 | 107.0 | 9.0 | 12.0 | 1.0 | 24.0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | [1, 2) | [1, 2) | 1 | [5, 10) | [5, 10) | Very Simple (1-5) | 2247.7000000000003 | 4.626862949190772 | 4.624336682890608 | 4.626862949190772 | 4.633423192812037 | 4.630083378875962 | 4.619304112688417 | [250, 350) | [250, 350) |\n| 56487 | 416010 | 1.0 | 1 | best ever slow cooker carnitas | 375 | 1035931 | 2010-03-09 | [\'weeknight\', \'course\', \'main-ingredient\', \'cuisine\', \'preparation\', \'occasion\', \'north-american\', \'main-dish\', \'pork\', \'mexican\', \'dinner-party\', \'dietary\', \'spicy\', \'one-dish-meal\', \'oamc-freezer-make-ahead\', \'low-carb\', \'egg-free\', \'free-of-something\', \'low-in-something\', \'meat\', \'pork-loins\', \'taste-mood\', \'number-of-servings\', \'presentation\', \'served-hot\'] | [\'820.1\', \'96.0\', \'55.0\', \'31.0\', \'93.0\', \'134.0\', \'5.0\'] | 15 | [\'cut the 2 lbs of pork into roughly 2 inch squares and place in the bottom of the slow cooker\', \'pour the stick of melted butter in the slow cooker coating the meat\', \'do the same with the 2 tbs of oil\', \'break the one cinnamon stick in half and place to the sides of the meat\', \'peel 5-6 garlic cloves , cutting the\', \'place the leaves and the garlic in the slow cooker , evenly distributed among the meat\', \'add the 1 tsp oregano , 1 tsp kosher salt , 1 tsp black pepper , 2 tsp cumin , and 1 tsp red pepper flakes , sprinkling evenly across the meat\', \'pour the 1 / 3 cup whole milk in the cooker , being careful not to wash away the spices from the meat\', \'add the coca cola , being careful not to wash away the spices from the meat\', \'add enough to just cover the top of the meat\', \'turn the slow cooker to the "low" heat setting\', \'cook for 6 hours , stirring every few hours\', \'once cooked , remove meat from the slow cooker with a slotted spoon and place in a separate large bowl\', \'shred the meat using two forks , and add spoonfuls of liquid from the cooker to ensure meat is moist\', \'serve on warm tortillas , with pico de gallo , sour cream , and chopped jalapenos\'] | a mouth-watering mexican carnitas recipe with a special sweet addition, slow cooked for hours in a crock-pot and served with warm tortillas. after numerous attempts at perfecting the art of carnitas from scratch, this is my best recipe yet. a taste of mexico in your very own kitchen! | [\'pork loin\', \'butter\', \'vegetable oil\', \'cinnamon stick\', \'garlic cloves\', \'bay leaves\', \'oregano\', \'kosher salt\', \'black pepper\', \'cumin\', \'red pepper flakes\', \'whole milk\', \'coca-cola classic\'] | 13 | 416010.0 | 820.1 | 96.0 | 55.0 | 31.0 | 93.0 | 134.0 | 5.0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | [1, 2) | [1, 2) | 1 | [10, 20) | [10, 20) | Complex (13-20) | 12301.5 | 4.626862949190772 | 4.624336682890608 | 4.626862949190772 | 4.633423192812037 | 4.613325174732103 | 4.619304112688417 | [650, 46000) | [650, 46000) |\n| 79135 | 498739 | 1.6666666666666667 | 3 | oatmeal cookies  sugar free  flour free | 25 | 1815304 | 2013-04-10 | [\'30-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'preparation\', \'drop-cookies\', \'desserts\', \'fruit\', \'easy\', \'cookies-and-brownies\', \'grains\', \'apples\', \'tropical-fruit\', \'bananas\', \'pasta-rice-and-grains\', \'3-steps-or-less\'] | [\'133.6\', \'2.0\', \'18.0\', \'0.0\', \'9.0\', \'1.0\', \'8.0\'] | 1 | [\'mix all ingredients , drop by the tablespoonful onto cookie sheet , bake at 350 degrees for 15-20 minutes\'] | sugar-free, flour-free oatmeal banana cookies, found recipe on facebook, thanks don colbert. | [\'bananas\', \'no-sugar-added applesauce\', \'oats\', \'almond milk\', \'raisins\', \'vanilla\', \'cinnamon\'] | 7 | 498739.0 | 133.6 | 2.0 | 18.0 | 0.0 | 9.0 | 1.0 | 8.0 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 1 | 0 | 0 | [1, 2) | [1, 2) | 1 | [1, 3) | [1, 3) | Simple (6-8) | 133.6 | 4.626862949190772 | 4.624336682890608 | 4.626862949190772 | 4.582551971281785 | 4.630083378875962 | 4.619304112688417 | [0, 150) | [0, 150) |\n| 64390 | 442155 | 1.0 | 1 | simple dark chocolate icing | 8 | 1731414 | 2010-11-16 | [\'lactose\', \'15-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'preparation\', \'occasion\', \'for-1-or-2\', \'5-ingredients-or-less\', \'desserts\', \'easy\', \'beginner-cook\', \'chocolate\', \'dietary\', \'inexpensive\', \'egg-free\', \'free-of-something\', \'number-of-servings\', \'3-steps-or-less\', \'birthday\', \'nut-free\'] | [\'1907.8\', \'153.0\', \'799.0\', \'27.0\', \'33.0\', \'291.0\', \'82.0\'] | 5 | [\'first put the butter and sugar in a bowl , stick it in the microwave untill butter is melted\', \'wisk untill sugar is desolved\', \'then immediatly after add your coco and mix\', \'finally enjoy !\', \'you can also add more butter to get the consistancy you desire\'] | very thick | [\'butter\', \'sugar\', \'cocoa\'] | 3 | 442155.0 | 1907.8 | 153.0 | 799.0 | 27.0 | 33.0 | 291.0 | 82.0 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | [1, 2) | [1, 2) | 1 | [5, 10) | [5, 10) | Very Simple (1-5) | 9539.0 | 4.626862949190772 | 4.624336682890608 | 4.626862949190772 | 4.582551971281785 | 4.630083378875962 | 4.619304112688417 | [650, 46000) | [650, 46000) |\n'


After cleaning the data, I performed EDA on the recipes dataset. I analyzed the distributions of the 'avg_ratings" and 'n_steps' columns and made the following observations: 

The vast majority of recipes earned a 5 rating, meaning that there is likely to be less data/less reliable data showing what sort of recipes receive very low ratings (below 3 stars).
 <iframe
 src="assets/rating-dist.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

Below is a graph of the distribution of the 'n_steps' column. We see a strong right skew with most recipes being between 3-20 steps, so this indicated that it might be worth binning the number of steps to make a recipe, with larger bin lengths for higher numbers of steps.
 <iframe
 src="assets/steps-dist.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

I then opted to investigate the relationship between the number of steps required to make a recipe and its average rating, as well as the calorie count of a recipe vs its average rating.

When investigating the relationship between steps and average rating, I opted to sort the data into bins loosely based on the distribution of steps as shown in the graph above. I then produced the graph below, which illustrates the aforementioned relationship. We observe that the median does not change between the different bins, but recipes with 5-10 steps and 10-20 steps have a slightly wider spread of values. 

 <iframe
 src="assets/steps-vs-rating.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

I did something similar when investigating the relationship between calories and average rating, binning calories based on the fact that most recipes had around 350-450 calories (the mean number of calories across recipes was 427, while the median was 305). When plotting, I observed that recipes with 250-650 calories had a slightly wider spread than recipes outside this calorie range.

 <iframe
 src="assets/cals-vs-rating.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

I also wanted to investigate if there was a correlation between meal type and number of steps vs. average rating. The results are shown in the pivot table below; we see that very complex recipes tend to receive slightly higher ratings, and desserts tend to receive slightly lower ratings (at least when they require 6-20 steps to make)

| ingredient_bins | breakfast | desserts | lunch |
| --- | --- | --- | --- |
| Very Simple (1-5) | 4.57353891996543 | 4.607085853739433 | 4.642696123949743 |
| Simple (6-8) | 4.603726881570355 | 4.577957900339616 | 4.636211606677793 |
| Moderate (9-12) | 4.595942820718766 | 4.573344969264219 | 4.622353757671199 |
| Complex (13-20) | 4.634689709555239 | 4.594243893618267 | 4.661446272708023 |
| Very Complex (21+) | 4.733333333333333 | 4.62984496124031 | 4.6022100706311235 |

I chose not to impute any missing values. A number of recipes had calorie counts of zero, but had no nutritional information besides sodium amounts, so there was not enough information present to estimate and impute a calorie count. I also considered imputing missing values based on meal type/food group, but since calories can differ greatly between dishes in the same food group and/or emal type, I decided that it ultimately was not beneficial enough impute the calorie counts.

---

## Framing a Prediction Problem

I chose to investigate what types of characteristics determine whether a recipe earns a higher average rating or not, and chose to frame it as a regression problem. Thus, the response variable I chose is 'avg_rating' or average rating of a given recipe, and I chose to evaluate the quality of my model with test MSE since I want to see how the model generalizes to unseen data, and test MSE is an excellent predictor for this.

---

## Baseline Model

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