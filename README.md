# Nourishing Numbers: The Impact of Nutritional Value and Recipe Steps on Food Ratings and Reviews
Authors: Aniket Bhosale and Selina Wu

# Introduction

## Introduction and Question Identification

### Dataset Introduction

Provide an introduction to your dataset, including its source and general description.

### Question Identification

Clearly state the one question your project is centered around. Explain why readers should care about this question.

### Dataset Details

Report the number of rows in the dataset, the names of the columns that are relevant to your question, and descriptions of those relevant columns.


# Step 2: Data Cleaning and Exploratory Data Analysis

## Data Cleaning

### Data Cleaning Steps

Describe, in detail, the data cleaning steps you took and how they affected your analyses. 

### Cleaned DataFrame

Show the head of your cleaned DataFrame.

## Univariate Analysis

### Distribution Plots

Embed at least one plotly plot displaying the distribution of a single column. Include a 1-2 sentence explanation about your plot.

## Bivariate Analysis

### Relationship Plots

Embed at least one plotly plot displaying the relationship between two columns. Include a 1-2 sentence explanation about your plot.

## Interesting Aggregates

### Grouped/Pivot Table

Embed at least one grouped table or pivot table and explain its significance.

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
With an observed p-value of **0.0** < 0.05, we reject the null hypothesis in favor of the alternative. The missingness of reviews does depend on number of steps. 

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

# Step 4: Hypothesis Testing

## Hypothesis Testing

### Hypotheses and Test

Clearly state your null and alternative hypotheses, your choice of test statistic and significance level, the resulting p-value, and your conclusion.

### Visualization (Optional)

Embed a visualization related to your hypothesis test if applicable.


# Step 5: Framing a Prediction Problem

## Problem Identification

### Prediction Problem

Clearly state your prediction problem and type (classification or regression). Report the response variable and why you chose it, the metric you are using to evaluate your model and why you chose it over other suitable metrics.


# Step 6: Baseline Model

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

# Step 7: Final Model

## Final Model

For our final model, we used all the features used in the baseline model and added two additional features that we engineered ourselves. These features were `'sodium_prop'` and `'protein_to_fat_ratio'`. These features were generated in the pre-processing stage of the pipeline. A description of the two new features is provided below.

`'sodium_prop'`

This feature is computed by calculating the proportion of sodium relative to all the other nutrients used in the baseline model. The addition of this feature was derived from the interesting aggregates plot provided in our analysis. In the interesting aggregates, we noticed that in general, there was a negative trend between `'rating'` and mean `'sodium (PDV)'`. This trend is very helful since our EDA show that in general, there are a lot more data points for 4 and 5 star ratings. This feature allows us to differentiate between the low and the high ratings, which makes it a very useful feature to consider.

`'protein_to_fat_ratio'`

This feature is computed by calculating the ratio between the ``'protein (PDV)'` content and `'total fat (PDV)'` content in a recipe. This feature was generated due to two reasons. The first reason is that we noticed a trend in our interesting aggregates plot. We noticed that as the star rating increases, the average protein in the recipe increases and the average total fat deacreses. This is a trend that can again be useful in differentiating between the low rating and high rating recipes. Additionally, we also noticed during the EDA that in general, healthier recipies (those with less sugar or higher protein) tended to have higher ratings. For this reason, we determined this to be an important feature to add.

**Note**: We have added a small epsilon to the denominator to prevent any zero division errors

Our final model architecture uses a `'RandomForestClassifier'`. This model was chosen as opposed to the `'DecisionTreeClassifier`' since we saw an improved performance increase after trying out the two models on the same data. Additionally, the due to the architecture of the `'RandomForestClassifier'`, it was determined that have mutliple `'DecisionTreeClassifiers'` trained on the data would allow the model to better generalize and capture the trends in our data. The pipeline for the model contains the transformer to introduce the two new features mentioned above and the `'RandomForestClassifier'`.

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


