# Kid-Centric Recipe Analysis  

By Rita Yujia Wu (yuw172@ucsd.edu) & Yolanda Feng (yuf019@ucsd.edu)  

This is a data science project for the DSC 80 Final Project, The Data Science Lifecycle, focusing on exploring the features of recipes that make them kid-friendly.

---
## Introduction

### Background Information & Project Topic
Good nutrition for kids is essential for their growth, development, and long-term health. A balanced diet ensures that children receive the necessary nutrients to strengthen their immune systems, support cognitive development, and build strong bones and muscles. Proper nutrition also helps prevent childhood obesity, which has tripled in the United States since the 1970s and is linked to serious health issues like type 2 diabetes and heart disease. Additionally, instilling healthy eating habits early on not only promotes immediate well-being but also helps establish routines and habits that contribute to their overall quality of life as they grow.

With this information in mind, our project aims to **investigate the features of recipes that make them kid-friendly**. By doing so, we hope to provide valuable insights for caregivers to better support children's nutritional needs. To achieve this, we are analyzing two datasets consisting of recipes and ratings posted since 2008 on food.com. These datasets were originally compiled for the recommender system research paper, "Generating Personalized Recipes from Historical User Preferences" by Majumder et al.

### Dataset Overview

#### Recipe Dataset
- **Rows**: 83,782
- **Columns**: 10

| Column          | Description                                                                                                         |
|-----------------|---------------------------------------------------------------------------------------------------------------------|
| `name`          | Recipe name                                                                                                         |
| `id`            | Recipe ID                                                                                                           |
| `minutes`       | Minutes to prepare the recipe                                                                                       |
| `contributor_id`| User ID who submitted the recipe                                                                                    |
| `submitted`     | Date the recipe was submitted                                                                                       |
| `tags`          | Food.com tags for the recipe                                                                                        |
| `nutrition`     | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for "percentage of daily value" |
| `n_steps`       | Number of steps in the recipe                                                                                       |
| `steps`         | Text for recipe steps, in order                                                                                     |
| `description`   | User-provided description                                                                                           |
| `ingredients`   | Text for recipe ingredients                                                                                         |
| `n_ingredients` | Number of ingredients in the recipe                                                                                 |

#### Interactions Dataset
- **Rows**: 731,927
- **Columns**: 5

| Column    | Description           |
|-----------|-----------------------|
| `user_id` | User ID               |
| `recipe_id` | Recipe ID            |
| `date`    | Date of interaction   |
| `rating`  | Rating given          |
| `review`  | Review text           |


---

## Data Cleaning and Exploratory Data Analysis
### Cleaned data
### Performed univariate analyses
### Performed bivariate analyses and aggregations
---
## Assessment of Missingness
### Addressed NMAR question
### Performed permutation tests for missingness
### Interpreted missingness test results
---
## Hypothesis Testing
### Selected relevant columns for a hypothesis or permutation test
### Explicitly stated a null hypothesis
### Explicitly stated an alternative hypothesis
### Performed a hypothesis or permutation test
### Used a valid test statistic
### Computed a p-value and made a decision
---
## Framing a Prediction Problem
---
## Baseline Model
---
## Final Model
---
## Fairness Analysis
---