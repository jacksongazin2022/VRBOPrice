## Abstract/Executive Summary

VRBO is a rental company which allows individuals to put their homes, apartments, or rooms up for short term rent. It is extremely popular for people looking for places to stay during vacation. Unsurprisingly, the price of these apartments often determines how frequently they are rented. In this report, we will discuss the process of predicting the price of a new rental unit based on characteristics of that unit. This will help the client determine prices for new rentals to make them attractive to customers.  Data was collected from 1561 different rental units and included thirteen characteristics about each rental including its unit number, average satisfaction, number of reviews, and neighborhood along with the daily price of the rental. We used Ridge Regression, Lasso Regression, and Logistic Regression to predict the price of a new rental. We found that Ridge Regression was the most effective model at predicting rental price. More specifically, we found that on average the predicted price from our Ridge Regression model was around 62 dollars and 35 cents off from the actual price. We further discuss our findings and the limitations of the study in the following paper.

## Part 1: Introduction

VRBO is a rental company which allows individuals to put their homes, apartments, or rooms up for short term rent. It is extremely popular for people looking for places to stay during vacation. Unsurprisingly, the price of these apartments often determines how frequently they are rented.
Our client's goal is for us to build a model which can be used to predict the price of a new rental. This will help the client determine prices for new rentals to make them attractive to customers. In this report, we will discuss the process of predicting the price of a new rental unit based on characteristics of that unit. Data was collected from 1561 different rental units and included thirteen characteristics or "features" of each rental along with the rental's price. These thirteen features were the rental's unit number, average satisfaction, number of reviews, number of accommodates, number of bedrooms, minimum night stay, district, neighborhood, walk score, transit score, bike score, and percentage of rentals in the neighborhood.   In the following sections, we will prepare our data so that it is adequate for model use, create a Ridge Regression, Lasso Regression, and Elastic Net Regression model to predictive price, and then choose the model which has the highest predictive accuracy. 


## Part 2: Data Cleaning


In preparation for building the model, my first step was to clean the data. One aspect of data cleaning is the process of dealing with incomplete data. Most models require our data set to be complete in the way variables are represented across different data points. Making predictions and building models are impossible when data sets have incomplete information. I first cleaned the data by checking if any of the data points were lacking values for any of the thirteen features. 

\begin{table}[ht]
\centering
\begin{tabular}{rrr}
  \hline
 & 0 & 138 \\ 
  \hline
\# Rows with NAs present at least one column (not including minstay) &   1 &   0 \\ 
  \# Rows with NAs present in at least one colum (for transformed data) &   1 &   0 \\ 
  \# Rows with NAs present in at least one column &   0 &   1 \\ 
   \hline
\end{tabular}
\end{table}
