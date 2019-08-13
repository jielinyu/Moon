---
layout: post
title:  "Kabam Challenge"
date:   2019-06-02
excerpt: ""
feature: https://media.licdn.com/dms/image/C560BAQF_bIjmex_qFQ/company-logo_200_200/0?e=2159024400&v=beta&t=6yhP3lzY2Hmf5n5shPPJgmt5dN7XWRAT-cRlkuRR63U
tag:
- Machine Learning
- Data Science
comments: true
---
## [Presentation](https://docs.google.com/presentation/d/1zrBBpXi_k79XopsfD2BIsnSTYmGdVBasNpyMvok8VZU/edit#slide=id.p)

In this challenge, the objective is to construct a model that predicts the probability of a user spending money in the game. I treated this problem as a binary classification on whether the user will spend or not. Then, I inferred the probabilities by the classification of the user groups given the features of a user’s gaming profile.

Three separate data files were given. I first joined these three tables by the unique user ID. Then, I converted the continuous variable - total_spending- to a binary variable that equals to 1 when the purchase is greater than 0, and equal to 0 otherwise. The binary variable ‘whether_spend’ was my responding variable. Since this target variable was indicated by the variable ‘total_spend’, I dropped the feature ‘total_spend’ to avoid confounding effects.

Next, I checked the proportion of these two classes, the proportion of users who had spent money was around 1.14% while 98.86% of users did not spend money. Therefore, this was an imbalanced data set. This was concerning because, in an imbalanced dataset, it may falsely give high accuracies by always blindly predicting the majority class. Thus, accuracy should not be used as a measure of performance as I was interested in being able to predict the minority class. Therefore, I chose to use recall and f-1 score as the evaluation metric here as recall would emphasize on correctly identifying the money-spending users, while f-1 score would present a more general measure of recall and precision.

Next, I counted missing values existing in each feature. Almost half of the observations in the features ‘game_stats_tutorial_complete’ and ‘game_stats_tutorial_complete_time’ were missing. Instead of removing or imputing these missing values, I dropped these two features because there was no intuitive way to replace these missing values. Moreover, there were features about experience points and redeemer actions that had a significant number of missing values. Since all of their distribution was right-skewed, I imputed the missing values with their median. For other features with small number of missing values, I remove the observations as it did not affect the data significantly

Since the ultimate goal was to predict the probability, I compared four classifiers which had the ‘predict_proba’ feature, which are Random Forest, Logistic Regression, K-nearest Neighbors, and LightGBM. This feature could output the probability that the classification predicted by the model. I standardize all numerical variables and I label encoded multiple categorical columns.

Next, I split the data into training, validation, and testing data set with a proportion as 6:2:2 using stratified sampling method to ensure that each set has approximately the same proportion of the binary classes as the complete data set.

The LightGBM classifier performed the best as both its recall score (0.028) and F-1 score (0.053) was the highest.  Next, I used Randomized Search Cross Validation to tune the hyperparameters of LightGBM classifier. The tuning was customized such that it maximized recall. In comparison to the untuned classifier, the tuned LightGBM classifier performed better on test data.
