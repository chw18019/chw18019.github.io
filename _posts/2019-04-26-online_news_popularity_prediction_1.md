---
title: "Online News Popularirty Prediction - Part I"
date: 2019-04-26
tags: [Machine Learning, Data Pre-processing, Python]
header:
  overlay_image: /assets/190426/ytcount-online_news.jpg
  caption: "Photo credit: [**YTcount**](https://unsplash.com)"
  teaser: /assets/190426/ytcount-online_news.jpg

excerpt: "Using Python to perform data pre-processing and data modeling. (Difficulty: ★★☆☆☆)"
---
This is a school project that helps to get familiar with algorithms and analytical skills. The team used JMP software (from SAS) to do all the work. This work focus on the methodology. For more details, you can find it in [this whitepaper](https://github.com/chw18019/OnlineNewsPopularity/blob/master/Whitepaper_OnlineNewsPopularity.pdf).


## Data Introduction
The dataset is called “Online News Popularity Data Set” and it can be accessed from [UCI Machine Learning Repository](https://archive.ics.uci.edu). Each row represents a news article and was collected from January 7, 2013 to January 7, 2015. There are 39,797 rows and 61 columns.

The original dataset contained 37 attributes of articles, and several natural language processing features were extracted by previous researchers (Fernandes, Vinagre & Cortez, 2015). In this study, 17 selected predictors are used to build models.

{% include figure image_path="/assets/190426/appendix1.png" alt="Online News Table1" %}

## The previous work
The previous work is available in this [website](https://www.researchgate.net/publication/283510525_A_Proactive_Intelligent_Decision_Support_System_for_Predicting_the_Popularity_of_Online_News). The best result was provided by a Random Forest with a discrimination power of 73%. The goal is trying to beat them with higher accuracy.


## Data Pre-processing
There're several things should be eliminated to achieve a refined model and interpretation. Missing values, outliers, and some variables that cannot interpret due to a lack of documentation.


## Methodolgy
The methodology of this study is followed by SEMMA(Shmueli, Bruce, Stephens & Patel, 2017, p. 18.)
- **Sample**: 50% training, 30% validation, and 20% testing dataset
- **Explore**: 
  - The distribution of shares
  - The correlations between variables
  - Missing values and outliers
- **Modify**: 
  - excluding missing data
  - excluding outliers
  - principal component analysis
  - combined dummy variables into one categorical variables
  - creating an alternate decision variable
  - variable selection
  - undersampling
- **Models**: 
  - Logistic regression
  - Decision tree
  - Bootstrap forest
  - Boosted tree
  - K-Nearest Neighbor (KNN)
  - Neural Network
  - Linear Discriminant Analysis
- **Assess**: 
  - Root Mean Square Error (RMSE)
  - Accuracy
  - Accuracy of 1's
  - test AUC (area underneath the test ROC curve)

## Results
Based on the logistic regression model, we made several recommendations to maximize shares on social media as following:
1. do not worry about including many images in their articles
2. make less world, business, and entertainment articles
3. make more articles categorized as social media, tech, lifestyle, and others.
4. focus on keyword strength
5. publish more articles on the weekend
 

## Conlcusion
To improve social media virality, we suggest Mashable focus most on publishing the right types of articles, with the right keywords, at the right time. Specifically, efforts like publishing articles on the weekend would cost Mashable little to nothing and would significantly increase social media visibility, while efforts like including many images in articles cost Mashable time and money and achieve very little. With our insights and recommendations, we believe that the average popularity of Mashable articles on social media would increase considerably.<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[In the next post](https://chw18019.github.io/online_news_popularity_prediction_2/), I'll demonstrate how to use `Python` to perform the exact analysis on this post, and also a web scraping technique to extrat more information from the url. Happy learning!
