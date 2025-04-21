# foods-receipe-rating-analysis

by hao shen (haoshen@umich.edu)

## Introduction 
This analysis is based on the Recipes and Rating dataset, which contains a large amount of recipes information as well as it's reviews from many different users. In this analysis, we are going to see if we can predict the recipe rating based on the number of steps the recipe takes and the minutes it take to prepare. the original recipe dataset the following features: 


|**Column**    | **Description** |
| --- | --- | --- |
|`'name'`      |Recipe name |
|`'id'`        |Recipe ID |
|`'minutes'`   |Minutes to prepare recipe |
|`'tags'`      |Food.com tags for recipe |
|`'nutrition'` |Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
|`'n_steps'`   |Number of steps in recipe |
|`'steps'`     |Text for steps in recipe, in order |
|`'n_ingredients'`   |Number of recipe ingredients |
|`'ingredients'`|List of recipe ingredients |
|`'rating'`    |Rating given |

---

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
first, since the recipe dataset and review dataset are given as two different dataset, but since they have the same recipe id, we can first merge them together to connect the rating with recipe. And since there are more than one review for a certain recipe, we will take the mean rating for a certain recipe, and add a new column to the recipe dataframe called "avg_rating".

after this, we will replace the rating with NaN value with 0. 

The result dataframe looks like this:


| name                                       |     id |   minutes |   n_steps |   avg_rating |
|:-------------------------------------------|-------:|----------:|----------:|-------------:|
| arriba   baked winter squash mexican style | 137739 |        55 |        11 |          5   |
| a bit different  breakfast pizza           |  31490 |        30 |         9 |          3.5 |
| all in the kitchen  chili                  | 112140 |       130 |         6 |          4   |
| alouette  potatoes                         |  59389 |        45 |        11 |          4.5 |
| amish  tomato ketchup  for canning         |  44061 |       190 |         5 |          5   |
| apple a day  milk shake                    |   5289 |         0 |         4 |          5   |
| aww  marinated olives                      |  25274 |        15 |         4 |          2   |



### Univariate Analysis

#### Distribution of minutes to prepare

first, let's look into the 'minutes' column to see how it is distributed:

<iframe
 src="assets/Cooking_Time_distribution.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>


from the distribution we can see that most recipe's preparation time lies on the range 0-25, and range 25-50


#### Distribution of number of steps:

<iframe
 src="assets/number_of_steps_distribution.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>


### Bivariate Analysis

#### minutes vs average rating:

<iframe
 src="assets/cooking_time_vs_avg_rating.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

we can see rating 5 have outliers that having extremly large minutes, this might be useful in out future analysis
#### number of steps vs average rating:

<iframe
 src="assets/number_of_steps_vs_avg_rating.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>


### Interesting Aggregates

from the original merged dataset, by grouping by the rating, we can investigate what mean number of steps and mean minutes looks like for different class rating. (since rating is integer and is from 0-5)

the chart looks like the following:

|   rating |   n_steps |    minutes |
|---------:|----------:|-----------:|
|        0 |  10.4182  | 35499.7    |
|        1 |   9.92347 |   119.812  |
|        2 |   9.70268 |   104.012  |
|        3 |   9.46229 |   100.119  |
|        4 |   9.26096 |    95.1594 |
|        5 |   9.65566 | 47462.3    |

from this chart we can see as the rating increase, the number of steps is tend to decreasae. As the the rating increase, the minutes is also tend to decrease. but the reason why rating 5 have a abnormal large minutes is because there are many outliers, some recipe have rating of 5 but have extremly larger minutes (as shown in the minutes vs average rating graph)

### Imputation
I replaced all the missing value (NaN) in rating into 0. 


## Framing a Prediction Problem

"how preparation minutes and number of steps affect the avg_rating?" this is a regression problem, i am using root meaning square error to evaluate my model performance.

at the time of prediction, we know the number of steps and minutes it take to prepare the recipe. becuase this is what the input values are, providede by the recipe creator. 

I am choosing root mean square error as my method to evaluate my model performance becasue it measures the average magnitude of prediction errors, and it is easy to interpret, for example we can interpret out RMSE as: "On average, our predictions are off by X stars"

## Baseline Model

for the baseline model, I am using RandomForestRegressor, my features are "minutes" and "n_steps", and both "minutes" and "n_steps" are quantitative features. 

my baseline models' RMSE is 1.0135.

I believe my baseline model did a good job, because our model predicting recipe ratings with an RMSE of 1.0. This means our predictions are typically within one star of what users actually rated, which is good enough to tell which recipes people will like versus dislike. The rating of a certain recipe can be very personal means its very different from person to person. So having a 1 point off is a indeed doing a good job. 

## Final Model

for my final model, I first standardize the minutes and n_steps, becuase from the previous distribution of minutes, we can see there are many outliers having extremly large value of minutes, to weaken the impact of these outliers, using standardization is a good choice. Also becasue I will try train our data on neural network, and having a standardized minutes and n_steps will help the performance of neural network, because neural network uses gradient descent algorithms, and gradient descent work best when all features are on the same scale. 

RandomForestRegression is an ensemble machine learning algorithm that builds multiple decision trees on random subsets of the data , and then averages their predictions to improve accuracy and reduce overfitting. And it is robust for regression tasks, it can handl non-linear relationships and feature interactions effectively.

Best Performing Hyperparameters: 
Best Hyperparameters: {'model__max_depth': 10, 'model__min_samples_leaf': 4}

Method for Hyperparameter Selection and Model Choice:
I choose RandomForestRegression because there are likely a non-linear relationship between features(minutes and n_steps) and and the target predict variable(rating)

I used GridSearchCV approach for hyperparameter tuning. 

The hyperparameter grid included:
max_depth: [None, 10, 20, 30], this controlls the maximum depth of each tree to balance model complexity and overfitting.
min_samples_leaf: [1, 2, 4], this specifies the minimum number of samples required at a leaf node to prevent overly specific splits.

The grid search was conducted with 5-fold cross-validation to ensure robust evaluation across different subsets of the training data. 

Finally, the combination has the lowest negative root mean squared error on the validation folds will be selected as the optimal model.

how my Final Model’s performance is an improvement over my Baseline Model’s performance: 
we can conduct a RMSE Comparison to compare the performance. The baseline model achieve a RMSE of 1.0135, and our final model achieve a RMSE of 0.9960, this means our performance improves around 1.73%.

