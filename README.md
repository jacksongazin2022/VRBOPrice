## Abstract/Executive Summary

VRBO is a rental company which allows individuals to put their homes, apartments, or rooms up for short term rent. It is extremely popular for people looking for places to stay during vacation. Unsurprisingly, the price of these apartments often determines how frequently they are rented. In this report, we will discuss the process of predicting the price of a new rental unit based on characteristics of that unit. This will help the client determine prices for new rentals to make them attractive to customers.  Data was collected from 1561 different rental units and included thirteen characteristics about each rental including its unit number, average satisfaction, number of reviews, and neighborhood along with the daily price of the rental. We used Ridge Regression, Lasso Regression, and Logistic Regression to predict the price of a new rental. We found that Ridge Regression was the most effective model at predicting rental price. More specifically, we found that on average the predicted price from our Ridge Regression model was around 62 dollars and 35 cents off from the actual price. We further discuss our findings and the limitations of the study in the following paper.

## Part 1: Introduction

VRBO is a rental company which allows individuals to put their homes, apartments, or rooms up for short term rent. It is extremely popular for people looking for places to stay during vacation. Unsurprisingly, the price of these apartments often determines how frequently they are rented.
Our client's goal is for us to build a model which can be used to predict the price of a new rental. This will help the client determine prices for new rentals to make them attractive to customers. In this report, we will discuss the process of predicting the price of a new rental unit based on characteristics of that unit. Data was collected from 1561 different rental units and included thirteen characteristics or "features" of each rental along with the rental's price. These thirteen features were the rental's unit number, average satisfaction, number of reviews, number of accommodates, number of bedrooms, minimum night stay, district, neighborhood, walk score, transit score, bike score, and percentage of rentals in the neighborhood.   In the following sections, we will prepare our data so that it is adequate for model use, create a Ridge Regression, Lasso Regression, and Elastic Net Regression model to predictive price, and then choose the model which has the highest predictive accuracy. 


## Part 2: Data Cleaning


In preparation for building the model, my first step was to clean the data. One aspect of data cleaning is the process of dealing with incomplete data. Most models require our data set to be complete in the way variables are represented across different data points. Making predictions and building models are impossible when data sets have incomplete information. I first cleaned the data by checking if any of the data points were lacking values for any of the thirteen features. 

We can see in Figure 2.1 that there were 138 data points in our original data set with a missing value for at least one of the variables. However, we can also see that this only occurred for the minstay variable; for every row all variables besides minstay had a value. Minstay indicates the minimum stay for a particular rental. I then transformed my data by looking at all data points which lacked a value for minstay and replaced that value with 1. If someone is using a rental, we can assume that the minimum stay for that rental is 1 night unless otherwise indicated. Since I was able to replace all of the values that were missing with a real value, there were 1561 data points before and after we dealt with missing values. 

In general, when we build a model, we want to make sure that our features have the potential to be used for prediction. In this case, a feature is only helpful if it contains information that could be useful in predicting the price of a new rental. For the feature UnitNumber, each data point has its own unique value, and the feature only indicates that each row is a unique rental. This idea is confirmed in Figure 2.2 as the number of rows in the data set is equal to the number of unique UnitNumber values. The fact that each data point is a different rental is not a feature that will be useful for prediction, and I therefore removed UnitNumber as a feature in the data set. We are trying to help the client "determine appropriate prices for new rental properties." Therefore, we also want our features to be useful for \textbf{new} rental properties. If a property is new, we will have no information on the customer feedback yet. Therefore, I am going to remove the features overall_satisfaction and reviews because they will not be available to a rental property that has not been on VRBO yet. Note, however, that I am going to include WalkScore, TransitScore, and BikeScore because these scores are independent of how many people have used the rental and will be determined before the rental is placed on VRBO. 

In Figure 2.3, we can see a table which displays the name, class, and description of each feature we have. Class tells us how these data points are currently formatted. 


Another aspect of data cleaning is ensuring that the format of the data is consistent with its intended representation. Thus, we want the class of each feature to be consistent with its description. 


 There are three variables which I do not believe are formatted consistently with their intended representation. These include room_type, neighborhood, and district. Each of these are formatted as characters which means that each value is a letter or a collection of letters.
 However, consider that the description for room_type is, "Is the rental for an entire house/apt, a private room, or a shared room?". The variable is defined to only take on one of three categories at each data point. Therefore, it would make more sense if this variable was mapped as a categorical variable which means that the variable can take on a finite amount of values which often have no numerical meaning. Likewise, both neighborhood and district take on a finite amount of categories and would be better off formatted as categorical features.
 
 We call the different categories of a particular categorical variable the levels of that categorical variable.  Note, that the three models we will eventually be testing on this data set are all linear models. We will explain what that means more specifically later, but for now, it is important to note that if a categorical feature has $k$ levels, our linear model will contain $k-1$ parameters for that feature. Each parameter quantifies how the presence of one level translates to a change in price. Consequently, we want each level to be present in at least $5\%$ of the data points, so our model has enough information to build reasonable parameters for each level. That being said, in practice we would prefer that our levels account for as close as possible to $10\%$ or more of all data points.
 
 In Figure 2.4, however, we can see that the level "shared room" in room_type only accounts for $\frac{50}{1561}\approx 3.2\%$ of the data points. Therefore, I am going to change room_type to have two features: "Entire Home/Apt" and "Single Room" where the latter includes both private rooms and shared rooms. We can see in Figure 2.5 that both levels are now relatively evenly distributed! We can now transform room_type into a categorical variable.
 
 When we look at neighborhood, we can see in Figure 2.6 that many of the levels contain a very few number of data points. I am going to transform our levels so that all levels with frequencies that account for less than 7 percent of the data are collapseed into a level called "Other", and the rest remain unchanged.  The neighborhoods which do account for more than 7 percent of the data are Humboldt Park, Logan Square, Rogers Park, and West Town. 
 
 We can now see in Figure 2.7 that each level accounts for at least $7\%$ of the data. Therefore, we can be much more confident about the estimates that our model will build for the categorical variable. We can now format neighborhood as a categorical feature. Note, we would prefer each level to contain at least $10\%$ of the data. Nevertheless, we will just need to be a little more careful about the conclusions we make regarding any parameters of this categorical variable. 
 
 For the feature district, we can see in Figure 2.8, that "Far Southeast", "Far Southwest", "Northwest", and  "Southwest" have few data points. I am therefore going to consider the districts "Far South East", "Far Southwest", and "Southwest" to be in the South. I am also going to consider Northwest to be in the North. Note, that "Central" does not have any logical level to collapse into, but only accounts for around $\frac{100}{1561}=6\%$ of the data points. Again, this is not quite ideal, and we will have to proceed with more caution in any of the parameters we derive for Central. 
 
 We can see in Figure 2.9, that the levels for district are much more balanced now. We can now transform district into a categorical feature.
 
 We can see in Figure 2.10 that each variable's format is consistent with its description. Note, that 'factor' is what R refers to as a categorical variable.

Data cleaning is now complete!

## Part 3: Ridge Regression

Ridge Regression is a type of linear model. Given a random sample of data with  $(\boldsymbol{X}\-_1,\cdots, \boldsymbol{X}_{p})$ features and a numeric response variable  $\boldsymbol{Y}$, a linear model describes the relationship between the features and the response value for each point $(i)$ as

$Y_i = \beta_0 + \beta_1X_{i1} + \cdots + \beta_p X_{ip} + \epsilon_i$
 where each $\epsilon_i$ is a random value representing the random error that is embedded in the world around us. We can use linear models to make predictions, $\hat{Y}_i$, where 
$\hat{Y_i} = \hat{\beta_0} + \hat{\beta_1}X_{i1} + \cdots + \hat{\beta_p} X_{ip}$
