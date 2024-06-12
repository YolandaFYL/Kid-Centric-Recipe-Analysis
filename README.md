# Kid-Centric Recipe Analysis  

By Rita Yujia Wu (*yuw172@ucsd.edu*) & Yolanda Feng (*yuf019@ucsd.edu*)  

This is a data science project for the DSC 80 Final Project, The Data Science Lifecycle, focusing on exploring the features of recipes that make them kid-friendly.

---
## Introduction  

Good nutrition is essential for children's growth, development, and long-term health, providing necessary nutrients to strengthen their immune systems, support cognitive development, and build strong bones and muscles. Proper nutrition helps prevent childhood obesity, which has tripled in the United States since the 1970s and is linked to serious health issues like type 2 diabetes and heart disease. However, high sodium intake, primarily from processed foods, is a significant concern for children, resulting in 1 in 6 children having high blood pressure, a major risk factor for heart disease and stroke. The CDC reports that 90% of children consume more sodium than recommended, with average intakes around 3,393 milligrams per day, far exceeding age-specific guidelines. Reducing sodium intake and maintaining a balanced diet for children is crucial and can be achieved by cooking at home and choosing low-sodium food products. Encouraging healthy eating habits through grocery shopping, cooking, and eating together helps children develop lifelong healthy dietary patterns.[[1]](https://www.healthdirect.gov.au/healthy-eating-for-children#:~:text=Healthy%20eating%20is%20essential%20for,better%20and%20enjoy%20life%20more.) [[2]](https://www.mayoclinichealthsystem.org/hometown-health/speaking-of-health/kids-and-sodium-serious-risks-and-alarming-realities)

In light of this, our project aims to **investigate the features of recipes that make them kid-friendly** by focusing on the recipe's sodium content, balanced nutrients, preparation process, and safety. By doing so, we hope to provide valuable insights for caregivers to better support children's nutritional needs for a healthier life. To achieve this, we are analyzing two datasets consisting of recipes and ratings posted since 2008 on [food.com](https://www.food.com/). These datasets were originally compiled for the recommender system research paper, "[Generating Personalized Recipes from Historical User Preferences](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf)" by Majumder et al.

To facilitate the investigation of our question, we examined both datasets and chose some of the highly relevant columns. The *Recipe Dataset* contains recipes from the website, an online recipe-sharing platform. This original dataset has 83,782 rows and 10 columns. The *Interactions Dataset* contains reviews and ratings submitted for the recipes in the *Recipe Dataset*. This original dataset has 731,927 rows and 5 columns.

Shown below is the names of the columns that are relevant to our question and the description of the relevant columns.


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
1. Left merge the recipes and interactions datasets together.
  - In this step, we match the recipes in the recipes dataset with their rating and reviews in the interactions dataset.

2. In the merged dataset, fill all ratings of 0 with np.nan.
  - On our website, we can find that the rating of recipes are represented as filled in stars. With that being said, the rating in our dataset is scale from 1 to 5, where 1 represents the lowest rating and 5 represents the highest. Thus, if the rating is 0, then that means this rating is missing. Therefore, we should replace all the 0 rating with np.nan to indicate missing and avoid further calculating bias.

3. Find the average rating per recipe, as a Series. Add this Series containing the average rating per recipe to the merged dataset.
  - After we match recipes with their rating and reviews, recipes have different number of ratings and reviews from users. Therefore, to better understand how each recipe is rated, we find the average of ratings of each given recipe.

4. Clean and change the data in the 'nutrition' column to dictionary type.
  - Nutrion information was initially in the form of 'faked' list, where each of them is a string that looks like the list format. Therefore, to clean the data in the column for conducting numerical calculations, we extract all the useful information (containing nutrient and their PDV) out of the string and form that into dictionary type where keys are the nutrient in string format and the values are the corresponding PDV in percentage in float format.

5. Clean and isolating the strings in 'tags' columns into words in list.
  - The information in 'tags' column was also initially in the form of 'faked' list, where each of them is a string that looks like a list format. Therefore, to clean the data in the column for further catogorizing the recipe, we find all the tags as word strings using regular expression and place them in a list for each recipe.

6. Dealing with outliers of the 'minutes' column (we consider minutes > 4320 as outliers) and the 'nutrition' column (we consider calories == 0 as outliers) by dropping the corresponding rows.
  - After exploring the data and search for the exact recipe on the website, we found that some recipes have irrgular cost time, which lead to a very huge number in the 'minutes' column. Therefore, to perform a fair and general calculation, we look at the distribution of the minutes and conclude that the recipes with minutes more than 4320 are outliers. Also, we found a very small amount of recipes whose calories in the 'nutrition' column to be 0. Since we are considering the healthiness of a recipe based on calories, we treat these recipes as outliers. We finally deal with the outliers in these two columns by dropping the corresponding rows.

7. Adding a new boolean column (`is_kid`) indicating whether the recipe contains kid-friendly tags
  -  `is_kid` is a boolean column checking whether the tags of recipes contain 'kid-friendly'. This step separates the the recipes into two groups, where True group contains recipes that are kid-friendly and False group contains not labelled kid-friendly. This is our main focus in this project.

8. Adding a new float column(`sodium(PDV)`) indicating the sodium(PDV) level of the recipe
  - `sodium(PDV)` is a float column containing the sodium PDV level in percentage of each recipe. We add this new column because we believe that recipes with lower sodium(PDV) level typically are more kid-friendly. This column gives us an indicator to compare the kid-friendly recipes with not kid-friendly recipes.

9. Adding a new boolean column (`has_vegfruit`) indicating whether the recipe contains fruit or vegetables
  - `has_vegfruit` is a boolean column chekcing whether the tags of recipes contain 'vegetable' or 'fruit'. We add this new column because we believe that recipes with vegetable or fruit tags are typically more kid-friendly. This column gives us another indicator to compare the kid-friendly recipes with not kid-friendly recipes.

10. Creating and calculating a comprehensive index for the 'nutrition' column based on [This Reference](https://health.gov/sites/default/files/2020-01/1995%20Dietary%20Guidelines%20for%20Americans.pdf). Adding a new float column (`nutrition_idx`) containing the nutrition index calculated.
  - `nutrition_idx` is a float column containing a calculated index for each recipe where lower index value means a relatively healthier recipe and vice versa. We perform the calculation by first turning the PDV back into the real consuming value through multiply the suggested daily value, then we divide by the total calories to get a fraction. Then, we calculate the distance between each recipe's fraction and the suggested fraction of each nutrient in the reference. 
  - Here is the value we use for calculation: 
  **Transform each recipe's nutrion into fraction:** total fat(pdv) * 65 * 9; protein(pdv) * 55 * 4; carbohydrates(pdv) * 300 * 4 (since other nutrient does not take into account of calorie calculations, we also ignore them in the index calculation)
  **The balanced fraction we get from the reference:** total fat: 0.3; protein: 0.3; carbonhydrates: 0.4
  **The formula for calculating the distance:** (sum(recipe - balanced) ** 2 / 3) ** (1 / 2) * 100
  - We add this new column because we believe that recipes with lower index (more healthier) are typically more kid-friendly. This column gives us another indicator to compare the kid-friendly recipes with not kid-friendly recipes.

11. Selecting the final columns we need for the future analysis by dropping redundant or useless columns
  - By dropping other useless and redundant columns, we reduce the size of the dataframe, making our future testing and modeling quicker.

Our cleaned dataframe has 233867 rows and 11 columns. Below are the first 5 rows of our final cleaned dataframe.

|    | name                                 |   minutes |   n_steps |   n_ingredients |   rating |   avr_rating | is_kid   |   sodium(PDV) | is_free   | has_vegfruit   |   nutrition_idx |
|---:|:-------------------------------------|----------:|----------:|----------------:|---------:|-------------:|:---------|--------------:|:----------|:---------------|----------------:|
|  0 | 1 brownies in the world    best ever |        40 |        10 |               9 |        4 |            4 | False    |             3 | False     | False          |        0.542309 |
|  1 | 1 in canada chocolate chip cookies   |        45 |        12 |              11 |        5 |            5 | False    |            22 | False     | False          |        1.41645  |
|  2 | 412 broccoli casserole               |        40 |         6 |               9 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |
|  3 | 412 broccoli casserole               |        40 |         6 |               9 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |
|  4 | 412 broccoli casserole               |        40 |         6 |               9 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |

### Univariate Analyses

<iframe
  src="assets/uni.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analyses 

<iframe
  src="assets/bi.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates

| is_kid   |   sodium(PDV) |   minutes |   n_steps |   n_ingredients |   is_free |   rating |
|:---------|--------------:|----------:|----------:|----------------:|----------:|---------:|
| False    |       28.6239 |   70.4862 |  10.0143  |         9.15846 | 0.0593935 |  4.68081 |
| True     |       23.7842 |   62.3692 |   9.95724 |         8.41533 | 0.0850192 |  4.67098 |

---
## Assessment of Missingness

### NMAR Analysis

### Missingness Dependency

<iframe
  src="assets/missingk.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/missings.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---
## Hypothesis Testing

<iframe
  src="assets/ht.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---
## Framing a Prediction Problem

---
## Baseline Model

---
## Final Model

---
## Fairness Analysis

<iframe
  src="assets/fairp.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
---