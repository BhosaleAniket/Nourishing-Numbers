# Nourishing Numbers: The Impact of Nutritional Value and Recipe Steps on Food Ratings and Reviews
Authors: Aniket Bhosale and Selina Wu

# Introduction
In today's fast-paced lifestyle food has found less of an importance in people's lives. With a growing supply of fast-food, people have began to substitute their home-cooked meals for a quick and often nutritionally "dirty" substitute. In fact, acccording to the National Center for Health Statistics, about 45% of people report that they have consumed fast-food on a given day. This begs us to ask the question : **Has an increase in fast-food consumption contined people into liking quickly accessible unhealthy meals?** 

In this project we explore a rich dataset of recipes and their corresponding ratings posted on Food.com since 2008 and scraped by by Majumder et al. **Our primary goal is to investigate how the nutritional content and number of steps to complete a recipe influences its rating**. By analyzing the relationships between nutritional factors (such as calories, fat, protein, sugar, etc.), number of steps involved, and the ratings given by users, we aim to predict the overall rating a recipe is likely to receive based on its nutritional profile.

We developed this approach because we are interested in whether the rise of fast food in America has conditioned people to stray from the appreciation of a healthy or home cooked meal made with time and dedication, and instead favoring foods that are quick and easy to prepare with low nutritional value. We hope that this information may also be helpful for recipe developers and nutritionists who aim to create healthier recipes that still receive high ratings, balancing taste and nutrition more effectively.

### Dataset Introduction

In our analysis, we will use two datasets to complete our analysis. The first dataset is the `'recipes'` dataset. This dataset contains 83782 rows and 12 columns. Below is a description of the dataset.

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe                                                                                                                                                                   |

The second dataset is the `'ratings'` dataset. This dataset contains 731927 rows and 5 columns. The information about the dataset is given below:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |


Here, since we are analyzing the relationships between health, number of steps, and ratings, the relevant columns are:
`'rating'`
`'review'`
`'nutrition'`
`'n_steps'`
`'minutes'`

The description of all these columns is provided above.

# Data Cleaning and Exploratory Data Analysis

## Data Cleaning

### Data Cleaning Steps

In order to make our dataset compatible with our analysis, we took the following steps to clean and wrangle the data:

1. We left_join the two datasets on `'id'` and `'recipe_id'`
2. We remove the entries where the `'recipe_did'` was `'Nan'` as a precaution.
3. We then set all the values in the `'rating'` column that were `'0'` to `'np.nan'`. This step was taken because the data for the rating column is an ordinal column that ranges from `'1 - 5'`. A value of `'0'` indicates invalid data which is why it is marked as `'np.nan'` or missing.
4. An `'average rating per recipe'` column is created to indicate the average rating for each recipe.
5. We drop the `'id'` column since the `'recipe_id'` contains the same data and has a more informative name.
6. We convert the `'submitted'` and the `'date'` columns from string to `'pd.DateTime'` using the `'to_datetime'` function so that these columns are compatible with temporal analysis.
7. We split the `'nutriton'` column into `'calories (#)'`, `'total fat (PDV)'`, `'sugar (PDV)'`, `'sodium (PDV)'`, `'protein (PDV)'`, `'saturated fat (PDV)'`, `'carbohydrates (PDV)'` columns and also convert it to floats for further analysis. This is an important step since we will be using these columns a lot thoughout our analysis.

### Cleaned DataFrame

| name                              |   minutes |   contributor_id | submitted   |   sodium (PDV) |   protein (PDV) |   saturated fat (PDV) |   carbohydrates (PDV) |
|:--------------------------------- |----------:|-----------------:|:------------|---------------:|----------------:|----------------------:|----------------------:|
| 1 brownies in the world best ever |        40 |           985201 | 2008-10-27  |              3 |               3 |                    19 |                     6 |
| 1 in canada chocolate chip cookies|        45 |          1848091 | 2011-04-11  |             22 |              13 |                    51 |                    26 |
| 412 broccoli casserole            |        40 |            50969 | 2008-05-30  |             32 |              22 |                    36 |                     3 |
| 412 broccoli casserole            |        40 |            50969 | 2008-05-30  |             32 |              22 |                    36 |                     3 |
| 412 broccoli casserole            |        40 |            50969 | 2008-05-30  |             32 |              22 |                    36 |                     3 |

## Univariate Analysis

<iframe
  src="assets/univariate.html"
  width="650"
  height="450"
  frameborder="0"
></iframe>

| Statistic | Value    |
|-----------|----------|
| count     | 83781.00 |
| mean      | 10.11    |
| std       | 6.39     |
| min       | 1.00     |
| 25%       | 6.00     |
| 50%       | 9.00     |
| 75%       | 13.00    |
| max       | 100.00   |


This plot plots the distribution of the `'n_steps'` in the merged dataframe. From the distribution plot, we can see that the number of steps in the dataframe is generally based around 6-7 steps. We can see that the minimum number of steps is 0 and the max number of steps is 100. In general, the number of steps lie between 6 and 13. The distribution of steps presents a normal distribution.

## Bivariate Analysis

<iframe
  src="assets/bivariate.html"
  width="650"
  height="450"
  frameborder="0"
></iframe>

Here, we have plotted a scatterplot where we show the relationship between the `'n_steps'` and `'average_review_length'`. From the plot, we can see that there is a clear linear trend between `'n_steps'` and `'average_review_length'`. This linear relationship lies between 0 and 40 steps. Beyond 40 steps, the relationship doesn't exhibit the linear relationship anymore and we need to introduce new parameters to summarize the relationship.

## Interesting Aggregates

### Grouped/Pivot Table

|   rating |   calories (#) |   carbohydrates (PDV) |   protein (PDV) |   saturated fat (PDV) |   sodium (PDV) |   sugar (PDV) |   total fat (PDV) |
|---------:|----------------:|----------------------:|----------------:|----------------------:|---------------:|--------------:|------------------:|
|      1.0 |          457.16 |                  15.6 |           31.19 |                 43.69 |          43.39 |         79.5  |             34.51 |
|      2.0 |          438.04 |                  15.63|           31.44 |                 40.81 |          28.94 |         79.11 |             31.14 |
|      3.0 |          424.03 |                  13.85|           33.81 |                 39.43 |          26.12 |         68.33 |             31.37 |
|      4.0 |          417.67 |                  13.3 |           34.76 |                 37.49 |          27.95 |         59.63 |             30.88 |
|      5.0 |          427.79 |                  13.64|           32.64 |                 40.33 |          29.02 |         69.09 |             32.73 |


This is a pivot_table that depicts the mean nutrients per rating. From this pivot_table, we can see that certain nutrients quatities drop or rise along with rating. This is noticable in nutrients in `'carbohydrates (PDV)` or `'calories (#)'`.

# Assessment of Missingness

The columns 'description', 'review', and 'rating' had missing values in the original merged dataset.

## NMAR Analysis

Of the three columns with missing values, the 'rating' column appears to potentially be NMAR. Missingness of the rating column would depend on whether users actually tried the recipe and have something to rate. If the users who viewed the recipe did not even try the recipe, there is no rating to file. Thus, additional data we could obtain to explain missingness would be information on engagement of the recipe, including total number of views, interactions, and time spent viewing the recipe, to make missingness MAR because we would be able to see whether many users saw the recipe and how many tried it.

## Missingness Dependency

We assessed the missingness dependency of the 'review' column on the columns 'n_steps' and 'minutes'.

### Number of Steps and Review

**Null Hypothesis:** The missingness of reviews does not depend on the number of steps in the recipe.

**Alternative Hypothesis**: The missingness of reviews does depend on the number of steps in the recipe.

**Test Statistic:** Absolute difference of mean number of steps of the distribution of the group without missing reviews and the distribution of the mean number of steps with missing reviews.

**Significance Level:** 0.05

Permutation test shuffling the missingness of reviews 1000 times to collect 1000 simulating mean differences in the two distributions as described in the test statistic.

<iframe
  src="assets/distr_n_steps_histogram.html"
  width="650"
  height="450"
  frameborder="0"
></iframe>
With an observed p-value of **0.0** < 0.05, we reject the null hypothesis in favor of the alternative. We suggest that the missingness of reviews does depend on number of steps. 

<iframe
  src="assets/rev_steps_plot.html"
  width="650"
  height="450"
  frameborder="0"
></iframe>
x: number of steps y: probability, with 1 corresponding to True and 0 corresponding to False.

### Minutes and Review

**Null Hypothesis:** The missingness of reviews does not depend on the number of minutes the recipe takes.

**Alternative Hypothesis**: The missingness of reviews does depend on the number of minutes the recipe takes.

**Test Statistic:** Absolute difference of mean number of minutes of the distribution of the group without missing reviews and the distribution of the mean minutes with missing reviews.

**Significance Level:** 0.05

Permutation test shuffling the missingness of reviews 1000 times to collect 1000 simulating mean differences in the two distributions as described in the test statistic.

<iframe
  src="assets/distr_minutes_histogram.html"
  width="650"
  height="450"
  frameborder="0"
></iframe>
With an observed p-value of **0.624** > 0.05, we fail to reject the null, there is not enough evidence that missingness of reviews depends on minutes. Although minutes and number of steps would appear to be correlated, this is not the case because many recipes involve waiting processes such as marinating, refrigerating, proofing, baking etc., so minutes has high variance and does not directly impact complexity of the recipe.

<iframe
  src="assets/rev_mins_plot.html"
  width="650"
  height="450"
  frameborder="0"
></iframe>
Zoomed in due to outliers in minutes, included only recipes that take 5000 minutes or less. 
x: minutes y: probability, with 1 corresponding to True and 0 corresponding to False.

# Hypothesis Testing

## Hypothesis Testing

### Hypotheses and Test

Clearly state your null and alternative hypotheses, your choice of test statistic and significance level, the resulting p-value, and your conclusion.

### Visualization (Optional)

Embed a visualization related to your hypothesis test if applicable.


# Framing a Prediction Problem

## Problem Identification

### Prediction Problem

** Prediction Problem**: Can we predict the rating of a recipe based on the nutritional content of the recipe. 

** Input Parameters**: `'total fat (PDV)'`, `'sugar (PDV)'`, `'sodium (PDV)'`, `'protein (PDV)'`, `'saturated fat (PDV)'`, `'carbohydrates (PDV)'`

** Output Parameters**: `'Rating'`

This problem is a **multi-class classification** problem. Here, the classes are the different ratings **1-5**. We chose this variable in order to answer the question we posed in the introduction. By predicting rating, we can asses if the people like healthy or unhealthy food.

In order to determine how good our model is, we will be using the following metric ** Accuracy **. The reason why this was chosen over the other metrics like precision, recall, and F1  score is because in this classifier, the false negative scores or false positive score individually are not crucial. Since this is a low stakes classifier where any individual metric doesn't need to have prevelance over other, the **accuracy** is the best overall measure here.


# Baseline Model

## Baseline Model

### Model Description

Since our goal is to create a multi-class classifier, we decided to use a Decision Tree Classifier. The parameters for the Decision Tree Classifier were set to the defualt parameters offered by scikit-learn. In order to predict the label for the `'rating'` column, we decided to use the following columns as the input parameters:

| Column                  | Description               |
| :---------------------- | :------------------------ |
| `'total fat (PDV)'`     | Continuous (Numerical)    |
| `'sugar (PDV)'`         | Continuous (Numerical)    |
| `sodium (PDV)`          | Continuous (Numerical)    |
| `'protein (PDV)'`       | Continuous (Numerical)    |
| `'saturated fat (PDV)'` | Continuous (Numerical)    |
| `'carbohydrates (PDV)'` | Continuous (Numerical)    |

There are no encodings or pre-processing that was performed. The data was split into training, testing, and validation data with the training data being 20% of all the data and the validation data being 25% of the training data.
The output of the baseling model were as follow:

| Type                    | Accuracy                  |
| :---------------------- | :------------------------ |
| `Training Accuracy`     | 87.37%                    |
| `Validation Accuracy`   | 68.23%                    |
| `Testing Accuracy`      | 68.36%                    |

In our opinion, this model is not "good" since there is a large difference between the training and testing accuracy. Currently, the model is overfit on the training data and doesn't have good variance. For this reason, the model has room for improvement and can't be classified as "good" right now.

|   | 1   | 2   | 3   | 4    | 5    |
|---|-----|-----|-----|------|------|
| 1 | 42  | 17  | 24  | 103  | 413  |
| 2 | 10  | 9   | 23  | 84   | 315  |
| 3 | 35  | 26  | 74  | 314  | 1032 |
| 4 | 95  | 99  | 305 | 1686 | 5221 |
| 5 | 345 | 297 | 864 | 4300 | 28146|

Above, we can see the confusion matrix generated for the model. We can note that there is low precision for the 'low' rating classes.

# Final Model

## Final Model

For our final model, we used all the features used in the baseline model and added two additional features that we engineered ourselves. These features were `'sodium_prop'` and `'protein_to_fat_ratio'`. These features were generated in the pre-processing stage of the pipeline. A description of the two new features is provided below.

`'sodium_prop'`

This feature is computed by calculating the proportion of sodium relative to all the other nutrients used in the baseline model. The addition of this feature was derived from the interesting aggregates plot provided in our analysis. In the interesting aggregates, we noticed that in general, there was a negative trend between `'rating'` and mean `'sodium (PDV)'`. This trend is very helful since our EDA show that in general, there are a lot more data points for 4 and 5 star ratings. This feature allows us to differentiate between the low and the high ratings, which makes it a very useful feature to consider.

`'protein_to_fat_ratio'`

This feature is computed by calculating the ratio between the ``'protein (PDV)'` content and `'total fat (PDV)'` content in a recipe. This feature was generated due to two reasons. The first reason is that we noticed a trend in our interesting aggregates plot. We noticed that as the star rating increases, the average protein in the recipe increases and the average total fat deacreses. This is a trend that can again be useful in differentiating between the low rating and high rating recipes. Additionally, we also noticed during the EDA that in general, healthier recipies (those with less sugar or higher protein) tended to have higher ratings. For this reason, we determined this to be an important feature to add.

**Note**: We have added a small epsilon to the denominator to prevent any zero division errors

Our final model architecture uses a `'RandomForestClassifier'`. This model was chosen as opposed to the `'DecisionTreeClassifier`' since we saw an improved performance increase after trying out the two models on the same data. Additionally, the due to the architecture of the `'RandomForestClassifier'`, it was determined that if we have mutliple `'DecisionTreeClassifiers'` trained on the data would allow the model to better generalize and capture the trends in our data. The pipeline for the model contains the transformer to introduce the two new features mentioned above and the `'RandomForestClassifier'`.

The hyperparameters for the model were chosen to be `'n_estimators'` and `'max_depth'`. The reasoning for each is provided below:

`'n_estimators'`

In order to better capture the trends of the data and make the model have higher variance, we decided to test different n_estimators to verify if the model can be made more generalizable and have higher testing and training accuracy.

`'max_depth'`

By making the decisionTrees more deep, we can again make the model more rich by having it analyze deeper trends in the data and again be more robust.

In order to search the best parameters, we used `'GridSearchCV'` along with a K-fold value of **3**. The inputs used were:

`'n_estimators'` : [50, 100, 150, 200]

`'max_depth'` : [None, 10, 20, 30, 40, 50]

The best hyperparameters were determined to be:

`'n_estimators'` : **150**

`'max_depth'` : **10**

After running the model with the aforementioned hyperparamters and pre-processing we found the new metrics to be:

| Type                    | Accuracy                  |
| :---------------------- | :------------------------ |
| `Training Accuracy`     | 77.37%                    |
| `Validation Accuracy`   | 77.22%                    |
| `Testing Accuracy`      | 77.37%                    |

From the results, we can see that the training accuracy of the model dropped. However, the validation and the testing accuracy of the model increased. We consider this an improvement over the baseline model since prediction based on unseen data is the most important aspect of a model. Here, we can see that the model is not overfitting the data like before, and the variance of the model is much higher and it is generalizing better to unseen data. This is why we consider this an improvement over the baseline model.

|   | 1   | 2   | 3   | 4    | 5    |
|---|-----|-----|-----|------|------|
| 1 | 28  | 10  | 12  | 38   | 511  |
| 2 | 4   | 2   | 9   | 35   | 391  |
| 3 | 15  | 12  | 34  | 160  | 1260 |
| 4 | 32  | 33  | 100 | 868  | 6373 |
| 5 | 112 | 94  | 254 | 1844 | 31648|

Above, we can see the confusion matrix for the final model. We can note that the model is more tuned toward the data spread of all the data in our dataset.

# Fairness Analysis

To perform our fairness analysis, we split the data into **Group X:** Low total fat content and **Group Y:** High total fat content, and used the median total fat content of **20.0** PDV as the threshold for splitting our data. We used median due to a couple of recipes with extremely high nutritional content that skew the mean too much. We used **precision** as our parity measure because we want the model to correctly identify ratings and because we used a classification model.

**Null Hypothesis:** Our model is fair. Its precision for recipes with low total fat and high total fat content are roughly the same, and any differences are due to random chance.

**Alternative Hypothesis:** Our model is unfair. Its precision for low sodium is lower than its precision for high sodium.

**Evaluation Metric:** Precision

**Test Statistic:** low_group precision - high_group precision

**Significance Level:** 0.05

<iframe
  src="assets/precision_difference_histogram1.html"
  width="700"
  height="450"
  frameborder="0"
></iframe>
With a an actual observed difference of -0.0049 and a p-value of **0.229**>0.05, we fail to reject the null hypothesis, and there is not enough evidence to claim that our model is unfair. The models precision for recipes with low total fat is not lower its precision for recipes with high sodium, and the situation where it did seem to be different was due to random chance.


