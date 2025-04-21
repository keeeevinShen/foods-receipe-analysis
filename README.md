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


<iframe
 src="assets/Cooking_Time_distribution.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>


#### Distribution of number of steps:

<iframe
 src="assets/number_of_steps_distribution.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>


### Bivariate Analysis


### Interesting Aggregates
