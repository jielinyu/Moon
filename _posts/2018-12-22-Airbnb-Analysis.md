---
layout: post
title:  "Airbnb in Amsterdam"
date:   2018-12-22
excerpt: ""
feature: https://media.wired.com/photos/5926c14ef3e2356fd800a56a/master/pass/airbnb_horizontal_lockup_white_print.jpg
tag:
- Data analysis
- Data Visualization
comments: true
---

Airbnb in Amsterdam!
--------------------
![GitHub](https://github.com/UBC-MDS/522_Airbnb_Listings_Amsterdam)

Airbnb renting has always been interesting. Some spots can be rented in higher prices, whereas other spots can only be rented in very low prices. Suppose you would like to be a host in Amsterdam - in order to be a successful host, it is a good idea to investigate the host listing data from airbnb and see what are the major predictors of the room prices. In this project, we are going to do this job for those who want to be a successful host! Therefore, in our project, a question that is reasonable to ask is: What are the two strongest predictors of price for airbnb listings in Amsterdam? We are going to analyse the host listing data in Amsterdam and find out the answer!

Analyzing and Prediction
------------------------

From the [Website](http://insideairbnb.com/get-the-data.html), we found the host listing data of Airbnb in Amsterdam. Before starting analyze the data, let's take a look at the data.

``` r
data=read.csv("data/amsterdam_listings.csv")
knitr::kable(head(data))
```

|     id| name                                              |  host\_id| host\_name | neighbourhood\_group | neighbourhood                          |  latitude|  longitude| room\_type      |  price|  minimum\_nights|  number\_of\_reviews| last\_review |  reviews\_per\_month|  calculated\_host\_listings\_count|  availability\_365|
|------:|:--------------------------------------------------|---------:|:-----------|:---------------------|:---------------------------------------|---------:|----------:|:----------------|------:|----------------:|--------------------:|:-------------|--------------------:|----------------------------------:|------------------:|
|   2818| Quiet Garden View Room & Super Fast WiFi          |      3159| Daniel     | NA                   | Oostelijk Havengebied - Indische Buurt |  52.36575|   4.941419| Private room    |     69|                3|                  243| 2018-10-01   |                 2.10|                                  1|                 90|
|   3209| Quiet apt near center, great view                 |      3806| Maartje    | NA                   | Westerpark                             |  52.39023|   4.873924| Entire home/apt |    160|                4|                   42| 2018-08-29   |                 1.08|                                  1|                 96|
|  20168| 100%Centre-Private Floor/Bathroom-Lockable Studio |     59484| Alex       | NA                   | Centrum-Oost                           |  52.36509|   4.893541| Private room    |     80|                1|                  221| 2018-09-20   |                 2.11|                                  2|                132|
|  25428| Lovely apt in City Centre (Jordaan)               |     56142| Joan       | NA                   | Centrum-West                           |  52.37311|   4.883668| Entire home/apt |    125|               14|                    1| 2018-01-21   |                 0.12|                                  2|                 65|
|  27886| Romantic, stylish B&B houseboat in canal district |     97647| Flip       | NA                   | Centrum-West                           |  52.38673|   4.892078| Private room    |    150|                2|                  164| 2018-10-04   |                 2.00|                                  1|                209|
|  28658| Cosy guest room near city centre -1               |    123414| Michele    | NA                   | Bos en Lommer                          |  52.37534|   4.857289| Private room    |     65|                3|                  426| 2018-10-02   |                 4.17|                                  2|                297|

From the data, we can see that there are several features inside:

<table>
<colgroup>
<col width="25%" />
<col width="74%" />
</colgroup>
<thead>
<tr class="header">
<th>Column name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>id</td>
<td>Unique identification code for the listing.</td>
</tr>
<tr class="even">
<td>name</td>
<td>Descriptive name of the listing.</td>
</tr>
<tr class="odd">
<td>host_id</td>
<td>Unique identification code for the host.</td>
</tr>
<tr class="even">
<td>host_name</td>
<td>First name of the host (privacy is respected. The data is open.)</td>
</tr>
<tr class="odd">
<td>neighbourhood</td>
<td>A categorical character variable that specifies the name of neighbourhood of the listing.</td>
</tr>
<tr class="even">
<td>latitude</td>
<td>A numeric variable that combining the <strong>longitude</strong> to represent the location of the listing.</td>
</tr>
<tr class="odd">
<td>longtitude</td>
<td>A numeric variable that combining the <strong>latitude</strong> to represent the location of the listing.</td>
</tr>
<tr class="even">
<td>room_type</td>
<td>A categorical variable including <strong>Shared Room</strong>, <strong>Private Room</strong> or <strong>Entire Room/Apt</strong>.</td>
</tr>
<tr class="odd">
<td>price</td>
<td>The price of the listing.</td>
</tr>
<tr class="even">
<td>minimum_nights</td>
<td>The minimum number of nights the host requires to book their property.</td>
</tr>
<tr class="odd">
<td>number_of_reviews</td>
<td>Number of customer reviews regarding the listing.</td>
</tr>
<tr class="even">
<td>last_review</td>
<td>Date of the last review.</td>
</tr>
<tr class="odd">
<td>reviews_per_month</td>
<td>Number of customer reviews per month.</td>
</tr>
<tr class="even">
<td>calculated_host_listings_count</td>
<td>Number of listings each host has simultaneously.</td>
</tr>
<tr class="odd">
<td>availability_365</td>
<td>The number of days that the listing is available in a 365 days, which is pre-defined by the host.</td>
</tr>
</tbody>
</table>

After looking at the data, we predict that `neighbourhood`, `location(latitude and longtitude)`, `room_type`, `minimum_nights`, `number_of_reviews`, `reviews_per_month`, `calculated_host_listings_count`, and `availability_365` can possibly be the factors affecting the price. In order to find out the two strongest predictors, after cleaning the data, we are going to perform some exploratory data analysis to tray and have a grasp of these factors visually, then use a decision tree algorithm to find out the two strongest predictors.

Data cleaning
-------------

After taking a look of these features, we realized that `neighbourhood` and `location(latitude and longtitude)` could act similarly in affecting the room price, as well as `reviews_per_month` and `number_of_reviews`. `Latitude and longitude` pairs are a more complicated feature than `Neighbourhood`, since it contains two continuous variables that must be combined to carry any meaning. At the moment, we haven't learned how to combine two features into one and implement it in the decision tree. Therefore, at this time, we decide not to use these two features. For `reviews_per_month` and `number_of_reviews`, when we first looked at the data, we believed that `reviews_per_month` is a better feature to use than `number_of_reviews` because it is possible that some listings have more reviews simply due to the fact that they're on airbnb longer than others. However, when we took a deep look, we found out that there are more than 2000 missing values in the `reviews_per_month`, therefore, we decided to use `number_of_reviews` instead. At the end, we used `room_type`, `minimum_nights`, `number_of_reviews`, `calculated_host_listings_count`, and `availability_365` for our analysis.

Data Visualization
------------------

In this section, we are going to explore if each feature affects the price by visualizing it.

The first one we are going to look at is the relationship between number of reviews of a listing and its price.

![Figure 1: Price vs Number of Reviews](../results/exploratory-amsterdam_price-reviews.png) Figure 1: Relationship between the Number of Reviews per Listing and Price.

Based on our experience, AirBnb reviews are unbalanced toward positive appraisals. Negative reviews are actually not that common, maybe because after a few negative reviews, no one else will rent that place. Therefore, we would think that a higher number of reviews would make the listing more reliable, allowing the host to charge a higher price for that. However, lower priced places can had more guests in the past, so they may have higher reviews either. From the graph above, there is a negative trend between price and number of reviews. Therefore, it can be predictor of the price, but we are not sure if it will be a strong one.

The second one we are going to look at is the relationship between minimum nights the host requires for a listing and its price.

![Figure 2: Price vs Minimum number of nights](../results/exploratory-amsterdam_price-minNights.png) Figure 2: Relationship between Minimum number of nights host requires and price.

From this graph , listings of median price level are having more required minimum nights than others, which is weird because we cannot see a clear pattern on it. Therefore, from this graph, we cannot be sure whether the minimum number of nights is a predictor of the price.

The third one we are going to look at is the relationship between the room type of a listing and its price.

![Figure 3: Price vs Room Type](../results/exploratory-amsterdam_price-roomType.png) Figure 3: Comparision of the maximum, minimum, 25% percentile, median, and 75% percentile of prices for different room types.

From this graph, we can see the counts of shared room are always high regardless of the price level. When we look at private room, there are more listings of high level price than other price. However, for the entire home/apt, low price level has most counts. From the graph, it is hard to see some corelation between the room type and the price.

The fourth one we are going to look at is the relationship between the number of listings that listing host has and the price of that listing.

![Figure 4: Price vs Number of listings per host](../results/exploratory-amsterdam_price-listingsCount.png) Figure 4: Relationship between the number of listings the host has and the price

From the graph, we can see there is a weak and unproportional corelation, but there is no strong correlation between the number of listings and the price. Although there are more highly priced listings when the number of listings a host has is small, that could be because there are more listings that have the host having low number of listings.

The last one we are going to look at is the relationship between the number of availability per 365 days of a listing and the its price.

![Figure 5: Price vs Availability per 365 days](../results/exploratory-amsterdam_price-availability.png) Figure 5: Relationship between the number of availability per 365 days and the price

From the graph, we can see that there is a week positive correlation between the availability per 365 days and the price, however we are not sure if it is a strong predictor or not.

After the exploration, as a result, we found out usually there are always a weak or a weird corelation between the price and the feature, however from the graphs, `number of reviews` and `availability per 365` have the strongest corelation among these features. We expect `number of reviews` and `availability per 365` to be the strongest predictors of price. We are not sure about rest of the features.

Decision Tree
-------------

In order to find out the two strongest predictors of price, we are going to make a decision tree and test it out. Thes features that will be tested out are: `room_type`, `minimum_nights`, `number of reviews`, `availability_365`, and `calculated_host_listings_count`. The `neighborhood` variable will be left to be tested in a future time. `room_type` as a categorical variable, will be converted into numeric for modeling.

We are going to label the price as three levels: `low`,`median`, and `high`, and set it as our response variable. To train the model, we are going to use 80% of the data as training data, and 20% of the data as testing data.

First, in order to get the higher accuracy, we decide to test the accuracy on the hyperparameter `max_depth`. We choose to expriment the value of `max_depth` between 2 and 13 with our training data. After the experiment, we found out that when `max_depth` is 4, the test accuracy is the highest. Therefore, we decide to use `max_depth=4` in this model.

Next, we use the training data again for fitting the model that has the hyperparameter of `max_depth=4`. After fitting the model, we are going to use this model and our testing data to predict the outputs. This is the decision tree we get:

![Figure 7: Decision Tree](../results/tree_graph.png) Figure 7: The decision tree we made for analyzing the strong predictors of the price

After getting the decision tree and outputs from the model, we are going to alculate the accuracy of the outputs by comparing the outputs with the actual outputs we already had in the testing data. The accuracy we came out is *0.482*, which is not high. This could be because all features we analyzed are not really strong predictors of price, which is actually in accordance with the findings in our data visualization.

Still, as the goal of our research, we are going to use this model to find out the most important factors affecting the price. After testing, we found out that `room_type` and `availability_365` are the two strongest predictors of the price. However, due to the low accuracy of this model, we are not very confident about the results. Since the accuracy of the model is low, it is possible that the results from the decision trees are just less reliable than our intuition and exploratory data analysis.

Conclusion and Limitation
-------------------------

Overall, from the data visualzation, we expected `number of reviews` and `availability_365` to be the two strongest predictors. However, from the decision tree, we got `room_type` and `availability_365` as the "winners". Regardless, we are confident that `availability_365` is a strong predictor of the price, but we are less sure about the other two.

We do know that our analysis has limitations. For example, we found out that `neighborhood` could be a strong predictor on pricing, however, due to the complexity of this categorical feature, we decided not to use it. Also, as mentioned before, we used `number of reviews` instead or `reviews per month`, which means that some listings could simply have more reviews because they are on Airbnb for a longer time.

For future work on this matter, we expect to explore the geographical attributes of the listings such as neighborhoods and/or distance to downtown to check whether or not they're the strongest predictor of the price of listings.

References
----------

Scikit-learn.org. (2018). sklearn.tree.DecisionTreeClassifier â scikit-learn 0.20.1 documentation. \[online\] Available at: <https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html> \[Accessed 24 Nov. 2018\].

Brownlee, J. (2018). Save and Load Machine Learning Models in Python with scikit-learn. \[online\] Machine Learning Mastery. Available at: <https://machinelearningmastery.com/save-load-machine-learning-models-python-scikit-learn/> \[Accessed 24 Nov. 2018\].

Inside Airbnb. (2018). Inside Airbnb. Adding data to the debate.. \[online\] Available at: <http://insideairbnb.com/get-the-data.html> \[Accessed 24 Nov. 2018\].
