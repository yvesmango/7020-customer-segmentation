**[Home](https://yvesmango.github.io/) >> [Projects](https://yvesmango.github.io/projects) >>  7020 Customer Segmentation Analysis**

# [Project] Customer Segmentation, Course Project

**Goal**: Perform customer segmentation on retail data. The goal of this project is to perform  unsupervised learning using k-means clustering to segment the data. More attribute information and the data context can be read here: [Kaggle source](https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis) 

## Table of Contents

- [Introduction](#introduction)
- [Project Description](#project-description)
- [Data Description](#data-description)
- [Conclusion](#conclusion)
- [License](#license)

## Introduction

This notebook was submitted as my final project for my statistics and probablistic learning class, using machine learning method to segment the customer data on which to conduct series of statistical analyses.

## Project Description

The objective of this project is to use k-means clustering to segment customers based on their purchase behavior since a typical unsupervised learning project requires the dataset include no target variable (y_pred). The dataset consists of transactional data containing customer ID, purchase amount, and purchase frequency. The project will involve data preprocessing, exploratory data analysis, and the application of k-means clustering to identify distinct customer segments based on their purchasing behavior. The project aims to provide insights into customer behavior, which can be used for targeted marketing and customer retention strategies. The project will be implemented using the R programming language and its associated data science libraries, such as tidyverse, dplyr, and ggplot2.

Find additional information about [k-means clustering algorithm here.](https://towardsdatascience.com/understanding-k-means-clustering-in-machine-learning-6a6e67336aa1)

## Data Description

Data used is customer data collected during marketing compaign period. More attribute details can be [read here.](https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis)

## Conclusion


Through clustering, I was able to distinguish the patterns by which the data is grouped. In the analysis, you will see that among the different variables, spending habits and parent status is one of the top identifiers. Overall, I think this was a pretty good project and run-through as I incorporated statistical analyses to suport some of the inferences made – on top of being my first attempt at clustering analysis.


Potential future directions to try would be to use more than 2 clusters for segmentation, then try to identigy subclasses. It would be interesting to see what the algorithm comes up with as additional classifiers.

## License

MIT License
