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
| name                                 |   minutes |   n_steps |   rating |   avr_rating | is_kid   |   sodium(PDV) | is_free   | has_vegfruit   |   nutrition_idx |
|:-------------------------------------|----------:|----------:|---------:|-------------:|:---------|--------------:|:----------|:---------------|----------------:|
| 1 brownies in the world    best ever |        40 |        10 |        4 |            4 | False    |             3 | False     | False          |        0.542309 |
| 1 in canada chocolate chip cookies   |        45 |        12 |        5 |            5 | False    |            22 | False     | False          |        1.41645  |
| 412 broccoli casserole               |        40 |         6 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |
| 412 broccoli casserole               |        40 |         6 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |
| 412 broccoli casserole               |        40 |         6 |        5 |            5 | False    |            32 | False     | True           |        1.95611  |

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
|   sodium(PDV) |   minutes |   n_steps |   n_ingredients |   is_free |   rating |
|--------------:|----------:|----------:|----------------:|----------:|---------:|
|       28.6239 |   70.4862 |  10.0143  |         9.15846 | 0.0593935 |  4.68081 |
|       23.7842 |   62.3692 |   9.95724 |         8.41533 | 0.0850192 |  4.67098 |

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
|   accuracy |
|-----------:|
|   0.952333 |
|   0.944421 |
<iframe
  src="assets/fairp.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
---