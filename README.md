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
## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis

