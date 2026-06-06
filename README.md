# Recipe Cook Time Analysis
DSC80 Final Project

This project captures various steps of analysis, starting from exploratory data analysis to hypothesis testing, creation of baseline models, and concluding with fairness analysis. The primary focus of this project involves the impact of different features of a recipe, such as ingredients, steps, nutritional value, and its effect on recipe cook time.

Authors: David Oh, Nathan Wong

## Introduction
### General Introduction 
Every step of this project was conducted on a dataset accessed from food.com, a website containing various recipes with their respective ratings, ingredients, and directions.

This project uses a merged version of two dataframes merged on recipe id: one containing logistical information on each recipe, and the other containing reviews and ratings for each recipe. Reviews and ratings are uploaded by individual users of website. 

The dataset spans a wide variety of foods, breakfast, lunch, dinner, desserts, ethnic cuisines, healthy alternatives, and everything in between. With over 230,000 rows, it offers a comprehensive snapshot of home cooking in the real world.

As college students with limited time and resources, we found ourselves asking: what actually makes a recipe quick to prepare? Specifically, our project centers around the question: hhow do a recipe's features relate to its cook time? Understanding the relationship between a recipe's features and its cook time has practical implications for meal planning and dietary decision-making, particularly for individuals with limited time.

Provide an introduction to your dataset, and clearly state the one question your project is centered around. Why should readers of your website care about the dataset and your question specifically? Report the number of rows in the dataset, the names of the columns that are relevant to your question, and descriptions of those relevant columns.
### Introduction of Columns
The merged dataset initially contains 234429 rows, and 16 columns. Each columns contains valueable information on each recipe. Here is a breakdown of each column. 

|Column                |Description|
|---                |---        |
|`'name'`                |Name of the recipe|
|`'recipe id'`                |Unique ID corresponding to the respective recipe|
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

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis

