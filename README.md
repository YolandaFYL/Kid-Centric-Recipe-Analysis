# Kid-Centric Recipe Analysis  

By Rita Yujia Wu (*yuw172@ucsd.edu*) & Yolanda Feng (*yuf019@ucsd.edu*)  

This is a data science project for the DSC 80 Final Project, The Data Science Lifecycle, focusing on exploring the features of recipes that make them kid-friendly.

---
## Introduction  

Good nutrition is essential for children's growth, development, and long-term health, providing necessary nutrients to strengthen their immune systems, support cognitive development, and build strong bones and muscles. Proper nutrition helps prevent childhood obesity, which has tripled in the United States since the 1970s and is linked to serious health issues like type 2 diabetes and heart disease. However, high sodium intake, primarily from processed foods, is a significant concern for children, resulting in 1 in 6 children having high blood pressure, a major risk factor for heart disease and stroke. The CDC reports that 90% of children consume more sodium than recommended, with average intakes around 3,393 milligrams per day, far exceeding age-specific guidelines. Reducing sodium intake and maintaining a balanced diet for children is crucial and can be achieved by cooking at home and choosing low-sodium food products. Encouraging healthy eating habits through grocery shopping, cooking, and eating together helps children develop lifelong healthy dietary patterns.[[1]](https://www.healthdirect.gov.au/healthy-eating-for-children#:~:text=Healthy%20eating%20is%20essential%20for,better%20and%20enjoy%20life%20more.) [[2]](https://www.mayoclinichealthsystem.org/hometown-health/speaking-of-health/kids-and-sodium-serious-risks-and-alarming-realities)

In light of this, our project aims to **investigate the features of recipes that make them kid-friendly** by focusing on the recipe's sodium content, balanced nutrients, preparation process, and safety. By doing so, we hope to provide valuable insights for caregivers to better support children's nutritional needs for a healthier life. To achieve this, we are analyzing two datasets consisting of recipes and ratings posted since 2008 on [food.com](https://www.food.com/). These datasets were originally compiled for the recommender system research paper, "[Generating Personalized Recipes from Historical User Preferences](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf)" by Majumder et al.

To facilitate the investigation of our question, we examined both datasets and chose some of the highly relevant columns. The *Recipe Dataset* contains recipes from the website, an online recipe-sharing platform. This dataset has 83,782 rows and 10 columns. The *Interactions Dataset* contains reviews and ratings submitted for the recipes in the *Recipe Dataset*. This dataset has 731,927 rows and 5 columns.


| Column      | Description                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------|
| `name`      | Recipe name (*Recipe Dataset*, *Interactions Dataset*)                                                                                                         |
| `id` | Recipe ID (*Recipe Dataset*)                                                                                                           |
| `minutes`   | Minutes to prepare the recipe (*Recipe Dataset*)                                                                                        |
| `tags`      | Food.com tags for the recipe (*Recipe Dataset*)                                                                                          |
| `nutrition` | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for "percentage of daily value" (*Recipe Dataset*) |
| `n_steps`   | Number of steps in the recipe (*Recipe Dataset*)                                                                                        |
| `recipe_id` | Recipe ID (*Interactions Dataset*)                                                                                                           |
| `rating`    | Rating given (*Interactions Dataset*)                                                                                                         |

---

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
We conducted the following data cleaning steps for our data analysis process.
1. Left merge the recipes and interactions datasets together.

2. In the merged dataset, fill all ratings of 0 with np.nan.

3. Find the average rating per recipe, as a Series. Add this Series containing the average rating per recipe back to the recipes dataset.

4.
5.
6.
7.
8.
| name                                 |   minutes |   contributor_id | tags                                                                                                                                                                                                                        | nutrition                                                                                                                             |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                        | ingredients                                                                                                                                                                    |   n_ingredients |   recipe_id |   rating |   avr_rating | is_kid   |   sodium(PDV) | is_free   | has_vegfruit   |   nutrition_idx |
|:-------------------------------------|----------:|-----------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------|----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|------------:|---------:|-------------:|:---------|--------------:|:----------|:---------------|----------------:|
| 1 brownies in the world    best ever |        40 |           985201 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] | {'calories': 138.4, 'total_fat': 10.0, 'sugar': 50.0, 'sodium': 3.0, 'protein': 3.0, 'saturated_fat': 19.0, 'carbohydrates': 6.0}     |        10 | ['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'remove from heat and let cool to room temperature', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'remove from the oven and cool completely before cutting'] | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |               9 |      333281 |        4 |            4 | False    |             3 | False     | False          |        0.542309 |
| 1 in canada chocolate chip cookies   |        45 |          1848091 | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               | {'calories': 595.1, 'total_fat': 46.0, 'sugar': 211.0, 'sodium': 22.0, 'protein': 13.0, 'saturated_fat': 51.0, 'carbohydrates': 26.0} |        12 | ['pre-heat oven the 350 degrees f', 'set aside', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center', 'serve hot and enjoy !']      | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |              11 |      453467 |        5 |            5 | False    |            22 | False     | False          |        1.41645  |
| 412 broccoli casserole               |        40 |            50969 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | {'calories': 194.8, 'total_fat': 20.0, 'sugar': 6.0, 'sodium': 32.0, 'protein': 22.0, 'saturated_fat': 36.0, 'carbohydrates': 3.0}    |         6 | ['preheat oven to 350 degrees', 'bake for 25 minutes or until cheese is lightly browned']                                                                                                                                                                                                                                                                                                    | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      306168 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |
| 412 broccoli casserole               |        40 |            50969 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | {'calories': 194.8, 'total_fat': 20.0, 'sugar': 6.0, 'sodium': 32.0, 'protein': 22.0, 'saturated_fat': 36.0, 'carbohydrates': 3.0}    |         6 | ['preheat oven to 350 degrees', 'bake for 25 minutes or until cheese is lightly browned']                                                                                                                                                                                                                                                                                                    | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      306168 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |
| 412 broccoli casserole               |        40 |            50969 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | {'calories': 194.8, 'total_fat': 20.0, 'sugar': 6.0, 'sodium': 32.0, 'protein': 22.0, 'saturated_fat': 36.0, 'carbohydrates': 3.0}    |         6 | ['preheat oven to 350 degrees', 'bake for 25 minutes or until cheese is lightly browned']                                                                                                                                                                                                                                                                                                    | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      306168 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |

### Univariate Analyses

### Bivariate Analyses 

### Interesting Aggregates
|   sodium(PDV) |   minutes |   n_steps |   n_ingredients |   is_free |   rating |
|--------------:|----------:|----------:|----------------:|----------:|---------:|
|       28.6239 |   70.4862 |  10.0143  |         9.15846 | 0.0593935 |  4.68081 |
|       23.7842 |   62.3692 |   9.95724 |         8.41533 | 0.0850192 |  4.67098 |

---
## Assessment of Missingness

### NMAR Analysis

### Missingness Dependency

---
## Hypothesis Testing

---
## Framing a Prediction Problem

---
## Baseline Model

---
## Final Model

---
## Fairness Analysis

---