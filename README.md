# Kid-Centric Recipe Analysis  

By Rita Yujia Wu (*yuw172@ucsd.edu*) & Yolanda Feng (*yuf019@ucsd.edu*)  

This is a data science project for the DSC 80 Final Project, The Data Science Lifecycle, focusing on exploring the features of recipes that make them kid-friendly.

---
## Introduction  

Good nutrition is essential for children's growth, development, and long-term health, providing necessary nutrients to strengthen their immune systems, support cognitive development, and build strong bones and muscles. Proper nutrition helps prevent childhood obesity, which has tripled in the United States since the 1970s and is linked to serious health issues like type 2 diabetes and heart disease. However, high sodium intake, primarily from processed foods, is a significant concern for children, resulting in 1 in 6 children having high blood pressure, a major risk factor for heart disease and stroke. The CDC reports that 90% of children consume more sodium than recommended, with average intakes around 3,393 milligrams per day, far exceeding age-specific guidelines. Reducing sodium intake and maintaining a balanced diet for children is crucial and can be achieved by cooking at home and choosing low-sodium food products. Encouraging healthy eating habits through grocery shopping, cooking, and eating together helps children develop lifelong healthy dietary patterns.[[1]](https://www.healthdirect.gov.au/healthy-eating-for-children#:~:text=Healthy%20eating%20is%20essential%20for,better%20and%20enjoy%20life%20more.) [[2]](https://www.mayoclinichealthsystem.org/hometown-health/speaking-of-health/kids-and-sodium-serious-risks-and-alarming-realities)

In light of this, our project aims to **investigate the features of recipes that make them kid-friendly** by focusing on the recipe's sodium content, balanced nutrients, preparation process, and safety. By doing so, we hope to provide valuable insights for caregivers to better support children's nutritional needs for a healthier life. To achieve this, we are analyzing two datasets consisting of recipes and ratings posted since 2008 on [food.com](https://www.food.com/). These datasets were originally compiled for the recommender system research paper, "[Generating Personalized Recipes from Historical User Preferences](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf)" by Majumder et al.

To facilitate the investigation of our question, we examined both datasets and chose some of the highly relevant columns. The *Recipe Dataset* contains recipes from the website, an online recipe-sharing platform. This original dataset has 83,782 rows and 10 columns. The *Interactions Dataset* contains reviews and ratings submitted for the recipes in the *Recipe Dataset*. This original dataset has 731,927 rows and 5 columns.

Shown below is the names of the columns that are relevant to our question and the description of the relevant columns. These columns form the foundation of the analysis, enabling the examination of preparation time, nutritional content, recipe complexity, and user ratings to guide the selection of healthy recipes for kids.


| Column      | Description                                                                                                                                                                                |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`      | Recipe name (*Recipe Dataset*), used for identifying the recipe on the website.                                                                                                             |
| `id`        | Recipe ID (*Recipe Dataset*), utilized for merging the Recipe Dataset with the Interactions Dataset.                                                                 |
| `minutes`   | Total time in minutes required to prepare the recipe (*Recipe Dataset*), included for time-based analysis of recipe preparation.                                                             |
| `tags`      | Food.com tags for the recipe (*Recipe Dataset*), applied in the analysis to derive categorical features of a recipe, aiding in classification.                         |
| `nutrition` | Nutritional information presented as [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for "percentage of daily value" (*Recipe Dataset*), essential for nutritional analysis. |
| `n_steps`   | The number of steps involved in the recipe (*Recipe Dataset*), used to assess recipe complexity.                             |
| `recipe_id` | Recipe ID (*Interactions Dataset*), utilized for merging with the Recipe Dataset to link user interactions, such as ratings, with specific recipes.                                     |
| `rating`    | The rating given to a recipe (*Interactions Dataset*), used in analyzing user ratings towards different recipes.                                                        |



---

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
1. Left merge the recipes and interactions datasets together.
  - In this step, we match the recipes in the recipes dataset with their rating and reviews in the interactions dataset.  


2. In the merged dataset, fill all ratings of 0 with np.nan.
  - On our website, we can find that the rating of recipes are represented as filled in stars. With that being said, the rating in our dataset is scale from 1 to 5, where 1 represents the lowest rating and 5 represents the highest. Thus, if the rating is 0, then that means this rating is missing. Therefore, we should replace all the 0 rating with np.nan to indicate missing and avoid further calculating bias.  


3. Find the average rating per recipe, as a Series. Add this Series containing the average rating per recipe to the merged dataset.
  - After we match recipes with their rating and reviews, recipes have different number of ratings and reviews from users. Therefore, to better understand how each recipe is rated, we find the average of ratings of each given recipe.  


4. Clean and change the data in the `'nutrition'` column to dictionary type.
  - Nutrion information was initially in the form of "faked" list, where each of them is a string that looks like the list format. Therefore, to clean the data in the column for conducting numerical calculations, we extract all the useful information (containing nutrient and their PDV) out of the string and form that into dictionary type where keys are the nutrient in string format and the values are the corresponding PDV in percentage in float format.  


5. Clean and isolating the strings in `'tags'`columns into words in list.
  - The information in `'tags'` column was also initially in the form of "faked" list, where each of them is a string that looks like a list format. Therefore, to clean the data in the column for further catogorizing the recipe, we find all the tags as word strings using regular expression and place them in a list for each recipe.  


6. Dealing with outliers of the `'minutes'` column (we consider minutes > 4320 as outliers) and the `'nutrition'` column (we consider calories == 0 as outliers) by dropping the corresponding rows.
  - After exploring the data and search for the exact recipe on the website, we found that some recipes have irrgular cost time, which lead to a very huge number in the `'minutes'` column. Therefore, to perform a fair and general calculation, we look at the distribution of the minutes and conclude that the recipes with minutes more than 4320 are outliers. Also, we found a very small amount of recipes whose calories in the `'nutrition'` column to be 0. Since we are considering the healthiness of a recipe based on calories, we treat these recipes as outliers. We finally deal with the outliers in these two columns by dropping the corresponding rows.  


7. Adding a new boolean column (`'is_kid'`) indicating whether the recipe contains kid-friendly tags
  -  `'is_kid'` is a boolean column checking whether the tags of recipes contain 'kid-friendly'. This step separates the the recipes into two groups, where True group contains recipes that are kid-friendly and False group contains not labelled kid-friendly. This is our main focus in this project.    


8. Adding a new float column(`'sodium(PDV)'`) indicating the sodium(PDV) level of the recipe
  - `'sodium(PDV)'` is a float column containing the sodium PDV level in percentage of each recipe. We add this new column because we believe that recipes with lower sodium(PDV) level typically are more kid-friendly. This column gives us an indicator to compare the kid-friendly recipes with not kid-friendly recipes.    


9. Adding a new boolean column (`'has_vegfruit'`) indicating whether the recipe contains fruit or vegetables
  - `'has_vegfruit'` is a boolean column chekcing whether the tags of recipes contain 'vegetable' or 'fruit'. We add this new column because we believe that recipes with vegetable or fruit tags are typically more kid-friendly. This column gives us another indicator to compare the kid-friendly recipes with not kid-friendly recipes.    


10. Creating and calculating a comprehensive index for the `'nutrition'` column based on [This Reference](https://health.gov/sites/default/files/2020-01/1995%20Dietary%20Guidelines%20for%20Americans.pdf). Adding a new float column (`'nutrition_idx'`) containing the nutrition index calculated.
  - `'nutrition_idx'` is a float column containing a calculated index for each recipe where lower index value means a relatively healthier recipe and vice versa. We perform the calculation by first turning the PDV back into the real consuming value through multiply the suggested daily value, then we divide by the total calories to get a fraction. Then, we calculate the distance between each recipe's fraction and the suggested fraction of each nutrient in the reference. 
  - Here are the values we use for calculation:  
  **Transform each recipe's nutrion into fraction:** total fat(PDV) * 65 * 9; protein(PDV) * 55 * 4; carbohydrates(pdv) * 300 * 4 (since other nutrient does not take into account of calorie calculations, we also ignore them in the index calculation)  
  **The balanced fraction we get from the reference:** total fat: 0.3; protein: 0.3; carbonhydrates: 0.4  
  **The formula for calculating the distance:** (sum(recipe - balanced) ** 2 / 3) ** (1 / 2) * 100
  - We add this new column because we believe that recipes with lower index (more balanced, healthier) are typically more kid-friendly. This column gives us another indicator to compare the kid-friendly recipes with not kid-friendly recipes.    


11. Selecting the final columns we need for the future analysis by dropping redundant or useless columns
  - By dropping other useless and redundant columns, we reduce the size of the dataframe, making our future testing and modeling quicker.    
  
### Result
Our cleaned dataframe has 233,867 rows and 11 columns. Below are the first 5 rows of our final cleaned dataframe.

|    | name                                 |   minutes |   n_steps |   n_ingredients |   rating |   avr_rating | is_kid   |   sodium(PDV) | is_free   | has_vegfruit   |   nutrition_idx |
|---:|:-------------------------------------|----------:|----------:|----------------:|---------:|-------------:|:---------|--------------:|:----------|:---------------|----------------:|
|  0 | 1 brownies in the world    best ever |        40 |        10 |               9 |        4 |            4 | False    |             3 | False     | False          |        0.542309 |
|  1 | 1 in canada chocolate chip cookies   |        45 |        12 |              11 |        5 |            5 | False    |            22 | False     | False          |        1.41645  |
|  2 | 412 broccoli casserole               |        40 |         6 |               9 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |
|  3 | 412 broccoli casserole               |        40 |         6 |               9 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |
|  4 | 412 broccoli casserole               |        40 |         6 |               9 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |

### Univariate Analyses

<iframe src="assets/uni.html" width="650" height="450" frameborder="0"></iframe>

For this analysis, we examined the distribution of sodium(PDV) values across recipes. It shows a heavily right-skewed distribution, with the majority of the data concentrated at lower sodium values and a long tail extending to higher values. This decreasing trend also suggests that as the sodium level increases, there are less recipes on our website.

### Bivariate Analyses 

<iframe src="assets/bi.html" width="650" height="450" frameborder="0"></iframe>

For this analysis, we examined the distribution of sodium(PDV) values conditioned on whether the recipe is labeled for kids (True) or not (False). Both distributions are right-skewed, but recipes labeled for kids generally have lower sodium values compared to those not labeled for kids. This indicates that recipes intended for children tend to contain less sodium. We will further analyze whether this difference is significant in later sections. 

### Interesting Aggregates
In this section, we investigate the relationship between whether the recipe is kid-friendly and other columns we believe are relavent in the dataframe, including `'sodium(PDV)'`, `'minutes'`, `'n_steps'`, `'n_ingredients'`, `'is_free'`, and `'rating'`. We group by the is_kid label and aggregate the mean of each columns.

| is_kid   |   sodium(PDV) |   minutes |   n_steps |   n_ingredients |   is_free |   rating |
|:---------|--------------:|----------:|----------:|----------------:|----------:|---------:|
| False    |       28.6239 |   70.4862 |  10.0143  |         9.15846 | 0.0593935 |  4.68081 |
| True     |       23.7842 |   62.3692 |   9.95724 |         8.41533 | 0.0850192 |  4.67098 |

Above is the summary table comparing various characteristics between recipes labeled is kid-friendly and those not labeled. It shows that items for kids have lower average sodium content (23.78 vs. 28.62), take less time to prepare (62.37 vs. 70.49 minutes), and have fewer ingredients (8.42 vs. 9.16). Interestingly, items for kids are more likely to be free (0.09 vs. 0.06) and have a slightly lower average rating (4.67 vs. 4.68), highlighting significant differences in nutritional content, preparation complexity, and accessibility between the kid-friendly recipes and not kid-friendly recipes. This interesting aggregates offer us a significant insight on which features of a recipe may correlate more with the kid-friendly labeling which inform us on future testing and modeling.

---
## Assessment of Missingness

In our cleaned dataset, only the `'rating'` column has missing values, so we decided to investigate the factors that could be affecting the missingness of users' ratings on certain recipes.

### NMAR Analysis

We believe that the missingness of `'rating'` is NMAR (Not Missing At Random), because, given a one to five grading scale range, if people choose not to fill in the stars, it is highly likely they believe this recipe does not deserve even a single star. That is, the missingness in the `'rating'` column reflects the user's extreme dissatisfaction with the recipe. 

To better understand and capture the users' satisfaction levels, we can introduce an additional feature in the comments section, allowing users to choose from options like "Recommended," "Meh," and "Do Not Recommend." This approach could provide more insight into users' levels of satisfaction with the recipes.

### Missingness Dependency

In this section, we examine the missingness dependency of the `'rating'` column. We are investigating whether the missingness in the `'rating'` column depends on the `'is_kid'` column, which is a boolean column checking whether the tags of recipes contain kid-friendly tags, or the `'sodium(PDV)'` column, which represents the sodium percentage of the daily value level of the recipe.

#### Dependency on `'is_kid'`

The distribution of `'is_kid'` when `'rating'` is missing and the distribution of `'is_kid'` when `'rating'` is not missing.

<iframe src="assets/missingko.html" width="650" height="450" frameborder="0"></iframe>

- **Null Hypothesis:** The missingness of ratings does not depend on whether the recipe is labeled as kid-friendly.
- **Alternate Hypothesis:** The missingness of ratings does depend on whether the recipe is labeled as kid-friendly.
- **Test Statistic:** The Total Variation Distance (TVD) in whether the recipe is labeled as kid-friendly between the group with missing ratings and the group without missing ratings.
- **Significance Level:** 0.05

The empirical distribution of the TVD, along with the observed statistic of 0.0044 indicated by the red vertical line on the graph.

<iframe src="assets/missingk.html" width="850" height="650" frameborder="0"></iframe>

Since the p-value that we found (0.086) is greater than 0.05, which is the significance level we set, we fail to reject the null hypothesis. Therefore, the missingness of the `'rating'` does not depend on the `'is_kid'` column, meaning that whether the recipe is labeled as kid-friendly does not significantly relate to the likelihood of a rating being missing.

#### Dependency on `'sodium(PDV)'`

The distribution of `'sodium(PDV)'` when `'rating'` is missing and the distribution of `'sodium(PDV)'` when `'rating'` is not missing. We specifically rescaled the plot in the report to help readers more easily identify the differences in distribution.

<iframe src="assets/missingso.html" width="650" height="450" frameborder="0"></iframe>

- **Null Hypothesis:** The missingness of ratings does not depend on the sodium (PDV) level of the recipe.
- **Alternate Hypothesis:** The missingness of ratings does depend on the sodium (PDV) level of the recipe.
- **Test Statistic:** The Kolmogorov–Smirnov (K-S) statistic in the sodium (PDV) level distribution between the group with missing ratings and the group without missing ratings.
- **Significance Level:** 0.05

The empirical distribution of the K-S statistic, along with the observed statistic of 0.0344 indicated by the red vertical line on the graph.

<iframe src="assets/missings.html" width="850" height="650" frameborder="0"></iframe>

Since the p-value that we found (0.0) is smaller than 0.05, which is the significance level we set, we reject the null hypothesis. Therefore, the missingness of the `'rating'` depends on the `'sodium(PDV)'` column, meaning that the sodium content of the recipe significantly relates to the likelihood of a rating being missing.


---
## Hypothesis Testing

<iframe src="assets/ht.html" width="850" height="650" frameborder="0"></iframe>

---
## Framing a Prediction Problem

We plan to predict whether a recipe is kid-friendly, which would be a binary classification problem. The response variable we chose is `'is_kid'`, the binary boolean value indicating whether the recipe is labeled as kid-friendly by the contributor. To build this classifier, our features come from our cleaned dataset, including `'minutes'`, `'n_steps'`, `'sodium(PDV)'`, `'is_free'`, `'has_vegfruit'`, and `'nutrition_idx'`.

The metric we are using to evaluate our model is the F-1 score. We believe the F-1 score is the optimal metric for us for the following reasons:

1. **Unbalanced Data:** Our dataset is unbalanced. Out of all 233,867 rows, only 26,100 rows, around 10%, evaluate to `True` for `is_kid`. This imbalance makes accuracy an unsuitable evaluation metric for our model, as it could be misleading.

2. **Precision and Recall:** Both variety and healthiness are important in building good eating habits for children. Therefore, we care about both the precision and recall levels of our model. The F-1 score, which is the harmonic mean of precision and recall, provides a balanced measure of these two metrics, ensuring our model gives the best prediction of a large variety of actually healthy recipes that are suitable for preparing for kids.

---
## Baseline Model

---
## Final Model

---
## Fairness Analysis

<iframe src="assets/fairp.html" width="850" height="650" frameborder="0"></iframe>
---