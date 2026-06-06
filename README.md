# Recipe Cook Time Analysis
DSC80 Final Project

This project captures various steps of analysis, starting from exploratory data analysis to hypothesis testing, creation of baseline models, and concluding with fairness analysis. The primary focus of this project involves the impact of different features of a recipe, such as ingredients, steps, nutritional value, and its effect on recipe cook time.

Authors: David Oh, Nathan Wong

## Introduction
### General Introduction 
Every step of this project was conducted on a dataset accessed from food.com, a website containing various recipes with their respective ratings, ingredients, and directions.

This project uses a merged version of two dataframes merged on 'recipe_id': one containing logistical information on each recipe, and the other containing reviews and ratings for each recipe. Reviews and ratings are uploaded by individual users of website. 

The dataset spans a wide variety of foods, breakfast, lunch, dinner, desserts, ethnic cuisines, healthy alternatives, and everything in between. With over 230,000 rows, it offers a comprehensive snapshot of home cooking in the real world.

As college students with limited time and resources, we found ourselves asking: what actually makes a recipe quick to prepare? Specifically, our project centers around the question: hhow do a recipe's features relate to its cook time? Understanding the relationship between a recipe's features and its cook time has practical implications for meal planning and dietary decision-making, particularly for individuals with limited time.

### Introduction of Columns
The merged dataset initially contains 234429 rows, and 16 columns. Each columns contains valueable information on each recipe. Here is a breakdown of each column. 

|Column                |Description|
|---                |---        |
|`'name'`                |Name of the recipe|
|`'recipe_id'`                |Unique ID corresponding to the respective recipe|
|`'minutes'`                |Time in minutes it takes to prepare the recipe|
|`'contributor_id'`                |Unique ID corresponding to the user who submitted the recipe|
|`'submitted'`                |Date of recipe submission|
|`'tags'`                |Identifying tags that are associated with recipe (ex. #healthy, #breakfast, #easyprep)|
|`'nutrition'`                |List of nutritional in the form of [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”|
|`'n_steps'`                |Number of steps required to make recipe|
|`'steps'`                |Ordered set of directions to prepare recipe|
|`'description'`                |Brief user provided description discussing the recipe|
|`'ingredients'`                |List of ingredients used to make the recipe|
|`'n_ingredients'`                |Number of ingredients required to make recipe|
|`'user_id'`                |Unique ID corresponding to the user who submitted the recipe|
|`'date'`                |Date of review submission|
|`'rating'`                |Numerical rating assigned to a recipe by a user, on a scale of 1 to 5.|
|`'review'`                |Anecdotal review left by user submitting the rating|


## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
The first step in our cleaning process was to drop columns deemed either irrelevant to our analysis or redundant given other columns present in the dataset. Columns such as date, submitted, description, steps, and review were removed as they do not bear on our central question regarding a recipe's features and cook time. Additionally, columns containing personally identifiable information, namely user_id and contributor_id, were dropped both for privacy reasons and due to their irrelevance to the analysis.

Ratings of 0 were replaced with np.nan, as a rating of 0 is not a valid value on the 1–5 scale and likely reflects missing data rather than a true user submission. A new column was then added representing the average rating across all reviews for each recipe, providing a more stable aggregate measure of user sentiment than any single rating alone.

The nutrition column, originally stored as a list of the form [calories, total fat, sugar, sodium, protein, saturated fat, carbohydrates], was expanded into seven individual numeric columns, one per nutritional attribute. The original nutrition column was subsequently dropped as it became redundant.

Finally, to address the presence of extreme outliers in numerical columns such as minutes, calories, and other nutritional values, we applied an IQR-based outlier removal procedure. For each relevant column, rows falling below Q1 - 1.5 * IQR or above Q3 + 1.5 * IQR were removed, as were any rows with non-positive values. This step was necessary given that the data generating process, user-submitted recipes on a public platform, is prone to erroneous or unrealistic entries (e.g., recipes listed as taking tens of thousands of minutes). Removing these outliers produces a dataset more reflective of realistic home cooking conditions and prevents them from distorting our analysis.

The first few rows of the cleaned dataframe are shown below.

| name                                 |     id |   minutes | tags                              | nutrition                                     |   n_steps | ingredients                       |   n_ingredients |   rating |   avg rating |   calories |   total fat |   sugar |   sodium |   protein |   saturated fat |   carbohydrates |
|:-------------------------------------|-------:|----------:|:----------------------------------|:----------------------------------------------|----------:|:----------------------------------|----------------:|---------:|-------------:|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|
| 1 brownies in the world    best ever | 333281 |        40 | ['60-minutes-or-less', 'time-t... ]| [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]      |        10 | ['bittersweet chocolate', 'uns... ]|               9 |        4 |            4 |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 |
| 1 in canada chocolate chip cookies   | 453467 |        45 | ['60-minutes-or-less', 'time-t... ]| [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]  |        12 | ['white sugar', 'brown sugar',... ]|              11 |        5 |            5 |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 |
| 412 broccoli casserole               | 306168 |        40 | ['60-minutes-or-less', 'time-t... ]| [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]     |         6 | ['frozen broccoli cuts', 'crea... ]|               9 |        5 |            5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |
| millionaire pound cake               | 286009 |       120 | ['time-to-make', 'course', 'cu... ]| [878.3, 63.0, 326.0, 13.0, 20.0, 123.0, 39.0] |         7 | ['butter', 'sugar', 'eggs', 'a... ]|               7 |        5 |            5 |      878.3 |          63 |     326 |       13 |        20 |             123 |              39 |
| 2000 meatloaf                        | 475785 |        90 | ['time-to-make', 'course', 'ma... ]| [267.0, 30.0, 12.0, 12.0, 29.0, 48.0, 2.0]    |        17 | ['meatloaf mixture', 'unsmoked... ]|              13 |        5 |            5 |      267   |          30 |      12 |       12 |        29 |              48 |               2 |

## Exploratory Data Analysis
### Univariate Analysis
We graphed the distribution of cooking times among all recipes. 
<iframe src="assets/plots/Distribution_of_Recipe_Cooking_Times.html" 
        width="800" 
        height="500" 
        frameborder="0">
</iframe>

We found that the average cooking time centered around roughly 30 - 34 minutes. The histogram is nearly normal and slightly right-skewed, meaning longer cooking times were less frequent. The variance within the graph suggests that the cooking time data behaves well, and centers itself around a general range

We then plotted the distribution of the number of ingredients within a recipe.

<iframe src="assets/plots/Frequency of Ingredient Counts per Recipe.html" 
        width="800" 
        height="500" 
        frameborder="0">
</iframe>

We found that the distribution of the number of ingredients also takes on a normal shape, with the average amount of ingredients being 8-9. The histogram takes on a tighter shape, suggesting the uncommon nature of using fewer/more ingredients.

### Bivariate Analysis
We performed bivariate analysis on the number of steps vs. cooking time.  

<iframe src="assets/plots/Relationship between Number of Steps and Cooking Time.html" 
        width="800" 
        height="500" 
        frameborder="0">
</iframe>

The plot shows a general, positive trend between the number of steps vs. cooking time. The scatterplot suggests that as the number of steps increases, cooking time increases. This relationship makes sense because, on average, increasing the number of steps would naturally result in a greater amount of time taken.

### Interesting Aggregates

By defining recipe complexity by binning each by the number of steps (ex. [0, 5, 10, 15, 100]), we can see an interesting trend between complexity and calories.
<iframe src="assets/plots/Average Cooking Time and Calories by Recipe Complexity.html" 
        width="800" 
        height="500" 
        frameborder="0">
</iframe>

After grouping by recipe complexity, we can see that the more complex a recipe is, the greater its average calorie count is. 
## Assessment of Missingness
### NMAR Analysis
We believe the rating column in our dataset is likely NMAR — that is, Not Missing At Random. The missingness in this column is plausibly dependent on the value of the rating itself, which is unobserved. Specifically, users who have a neutral or mildly negative experience with a recipe may be less motivated to leave a rating than users who feel strongly, either positively or negatively. This means the absence of a rating is systematically related to the rating's value: middling recipes are disproportionately likely to go unrated, making the missingness non-random and dependent on the unobserved data itself.

To potentially make this missingness MAR, we would want to obtain additional data about user behavior on food.com — for example, the number of times a recipe page was visited versus the number of ratings submitted, or whether users who viewed a recipe went on to make it. If the probability of a missing rating could be explained by an observable variable such as page traffic or recipe popularity, the missingness would then be attributable to those observed factors rather than the rating value itself, satisfying the conditions for MAR.

### Missingness Dependency
To explore the missingness of avg_rating, we conducted permutation tests to determine whether its missingness depends on other columns in the dataset. Of the columns tested, we selected n_steps and name_length to present in detail — one representing a column we believed the missingness of avg_rating would depend on, and one representing a column we believed it would not.

First, we perform the permutation test on n_steps and rating_na, and the missingness of n_steps does depend on rating_na.

**Null Hypothesis**: The distribution of n_steps when rating is missing is the same as the distribution of n_steps when rating is not missing — i.e., the missingness of avg_rating does not depend on n_steps.

**Alternative Hypothesis** : The distribution of n_steps when rating is missing is different from the distribution of n_steps when rating is not missing — i.e., the missingness of avg_rating does depend on n_steps.

After performing the permutation test, the observed absolute mean difference in n_steps was 0.63, and the p-value was 0.0. The plot above shows the empirical null distribution of the test statistic across 500 permutations, with the observed statistic marked by the dashed red line. The observed value falls entirely outside the null distribution.

Since the p-value of 0.0 is less than our significance level of 0.05, we reject the null hypothesis. The missingness of avg_rating is dependent on n_steps, suggesting that recipes with missing ratings tend to systematically differ in their number of steps from recipes that have ratings.

<iframe src="assets/plots/perm_n_steps.html" 
        width="800" 
        height="500" 
        frameborder="0">
</iframe>
## Hypothesis Testing

## Framing a Prediction Problem

We have established that a recipe's features, such as its number of steps, number of ingredients, and nutritional content, have some relationship with its cook time. This naturally leads to the following prediction problem: can we accurately predict how long a recipe takes to prepare based on its measurable features?

Since minutes is a continuous numerical variable, this is a regression problem. We chose minutes as our response variable because cook time is the most direct and practical measure of a recipe's feasibility, particularly for individuals with limited time. At the time of prediction, we would realistically have access to all features used — n_steps, n_ingredients, calories, and time_category — as these are all properties of the recipe itself, determined before any user interaction such as reviews or ratings occurs. No post-hoc information is used.

To evaluate model performance, we use both RMSE and R². RMSE is preferred as a primary metric because it expresses error in the same units as the response variable (minutes), making it interpretable in practical terms. R² is reported alongside it to convey how much of the variance in cook time the model explains. We opt for RMSE over MAE because it penalizes larger errors more heavily, which is desirable here. A prediction that is off by 60 minutes is disproportionately more problematic than one off by 5 minutes, and RMSE reflects that asymmetry.

## Baseline Model

The baseline model is a linear regression trained to predict a recipe's cook time in minutes. It uses two quantitative features: n_steps, the number of steps in a recipe, and n_ingredients, the number of ingredients. No ordinal or nominal features were included at this stage. Both features were preprocessed using standard scaling, normalizing each to have zero mean and unit variance, which is appropriate for linear regression as it ensures neither feature dominates due to differences in scale.

The model was evaluated using Root Mean Squared Error (RMSE) and R². On the training set, the model achieved an RMSE of 20.60 minutes and an R² of 0.1648, and on the test set, an RMSE of 20.62 minutes and an R² of 0.1750. The near-identical train and test scores indicate the model is not overfitting, but the overall performance is poor. An R² of roughly 0.17 means the model explains only about 17% of the variance in cook time, and an RMSE of ~20 minutes reflects substantial prediction error relative to the scale of the target variable. This is not a good model,  n_steps and n_ingredients alone are insufficient predictors of cook time, and additional features will be necessary to improve predictive power in subsequent iterations.

## Final Model

Since linear regression assumes a linear relationship between features and the response variable, it is poorly suited for a target like cook time, which likely depends on complex, non-linear interactions between recipe attributes. We therefore selected a Random Forest Regressor as our final model, which is capable of capturing non-linear relationships and feature interactions without requiring explicit specification of those interactions.

Two new features were introduced in this model. The first is time_category, derived by extracting time-related tags from each recipe's tags column using a regular expression pattern. Recipes on food.com are tagged by their contributors with labels such as "30-minutes-or-less" or "4-hours-or-less", meaning this information is available at the time of prediction as it is part of the recipe's metadata. Because these tags directly reflect contributor-estimated cook time, they carry strong signal for predicting minutes and were encoded ordinally in increasing order of time: unknown, 15m, 30m, 60m, 4h. The second added feature is calories, motivated by the observation that caloric content tends to coincide with recipe complexity, more calorie-dense recipes often involve richer, more involved preparation, making it a reasonable proxy for cook time.

Hyperparameter tuning was performed via 5-fold cross-validated grid search over max_depth values of [2, 5, 10, 12, 15, 22, 28] and min_samples_split values of [2, 3, 4, 6], optimizing for R². The best performing configuration used a max_depth of 28 and min_samples_split of 2. The final model achieved a Train RMSE of 1.91 minutes, a Test RMSE of 4.44 minutes, a Train R² of 0.9928, and a Test R² of 0.9617. This represents a dramatic improvement over the baseline model's Test RMSE of 20.62 minutes and R² of 0.1750, suggesting that the addition of time_category and calories, combined with a model capable of capturing non-linear structure, substantially increased predictive power. The small gap between train and test performance indicates the model generalizes well, though the high train R² does suggest mild overfitting that could be addressed with further regularization.

We also plotted Train vs. Test R-squared for our final model
<iframe src="assets/plots/Train vs. Test R-squared by Max Depth.html" 
        width="800" 
        height="500" 
        frameborder="0">
</iframe>

## Fairness Analysis
To assess whether our final model performs equitably across different recipe types, we conducted a permutation test comparing model error between high-calorie and low-calorie recipes. Group X was defined as high-calorie recipes, those with calorie counts above the mean, and Group Y as low-calorie recipes, those at or below the mean. The evaluation metric used was RMSE, and the test statistic was the absolute difference in RMSE between the two groups.

The null hypothesis states that our model is fair: any observed difference in RMSE between high-calorie and low-calorie recipes is due to random chance alone. The alternative hypothesis states that our model is unfair: the RMSE for high-calorie recipes is significantly different from that for low-calorie recipes. We used a significance level of 0.05.

A permutation test was conducted over 500 iterations, randomly shuffling the calorie group labels each time and recomputing the RMSE difference to construct an empirical null distribution. The observed RMSE difference was 0.5938 minutes, and the resulting p-value was 0.0, meaning that none of the 500 permuted differences met or exceeded the observed value. The histogram above illustrates this clearly, with the observed difference falling well outside the bulk of the null distribution.

At a significance level of 0.05, we reject the null hypothesis. The evidence suggests that our model does not perform equally across high-calorie and low-calorie recipes, indicating a potential fairness concern. This is not entirely surprising, as calories was used as a feature in training, which may have introduced a systematic bias in how the model handles recipes at different ends of the caloric spectrum.
