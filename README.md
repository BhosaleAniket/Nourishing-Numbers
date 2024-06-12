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

## Number of Steps and Review

**Null Hypothesis:** The missingness of reviews does not depend on the number of steps in the recipe.

**Alternative Hypothesis**: The missingness of reviews does depend on the number of steps in the recipe.

**Test Statistic:** Absolute difference of mean number of steps of the distribution of the group without missing reviews and the distribution of the mean number without missing reviews.

**Significance Level:** 0.05

Permutation test shuffling the missingness of reviews 1000 times to collect 1000 simulating mean differences in the two distributions as described in the test statistic.

---
layout: default
title: Permutation Test Plot
---

<iframe
  src="assets/distr_steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

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

### Feature Engineering

State the features you added and why they are good for the data and prediction task.

### Modeling Algorithm and Hyperparameters

Describe the modeling algorithm you chose, the hyperparameters that performed the best, and the method you used to select hyperparameters.

### Model Performance

Describe how your Final Model’s performance is an improvement over your Baseline Model’s performance.

### Visualization (Optional)

Include a visualization that describes your model’s performance, e.g., a confusion matrix, if applicable.

# Step 8: Fairness Analysis

## Fairness Analysis

### Fairness Evaluation

Perform a “fairness analysis” of your Final Model. Clearly state your null and alternative hypotheses, your choice of test statistic, and the results of your permutation test. 

### Conclusion

Interpret the results and explain any implications related to the fairness of your model.



