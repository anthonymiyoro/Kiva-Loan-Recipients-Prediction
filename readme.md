# Insights from Kiva Data

### This repository contains a seriess of notebooks with which we try to predict the leading factors that influence the loan amount someone will recieve from Kiva

In the first notebook we summarise the main characterisitcs of the dataset so as to know which algorithm to use in the future.

### [1st Notebook (Exploratory Analysis)](exploratory_analysis.ipynb)

- We first merge our 3 datasets into 1 (df.csv)

- After this, we check the variables in our dataset. In our first example from cell 19 to 26 we convert an object (in panda format) to datetime format.

- Once thats done, we check for missing values in the dataset. When we're done with exploratory analysis we then move on to feature engineering.


### [2nd Notebook (Feature Engineering)](exploratory_analysis.ipynb)

- With feature engineering, we begin to attempt to answer what factors drive the loan amounts requested by Kiva borrowers.

- After importing all the data created from the previous notebook, we start by removing the outlier loans. As we can see from the image below:
![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic1.png "pic 1")

- The outlier loans are those whose amounts fall outside 3 standard deviations from the mean. After running 
```
# dataset without outliers
df_removed_outliers=df[(((df['loan_amount']-df['loan_amount'].mean())/(df['loan_amount'].std())).abs()<3)] 

```
Our data becomes much less skewed as below:
![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic2.png "Data Skewness")

Unfortunately upon looking at some of the data we see that most of the loans are in fact around $250 so instead we'll remove all loans hogher than $6000 as those are rare. 

- Since our data looks much less skewed we can begin working on selecting the features we deem most important. These features that determine how much we ask for in a loan can be found by asking:
			- What job does the borrower have?
			- Have you taken a loan before?
			- Who is lending the money?
			- Where do you live?

- In the following cells, we went through these features and found that:

	- Widowed couples ask for lower loan amounts than non-widowed couples.

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic4.png "Marital Status")

		Men asked for higher loan amounts regardless of marital status.

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic9.png "Marital Status 2")

	- Borrowers with children requested for lower loan amounts

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic6.png "Children Loan amounts").

		Those with 2 to 6 children asked for the highest amount in loans with the amount decreasing with an increase in the number of children.

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic10.png "Children Loan amounts2").

	- Surprisingly, the elderly appeared to ask for higher loan amounts from the ages of 60 to 80 while females of 30 years of age asked for the lowest loan amounts on average. For males, the loan amounts requested stagnated from 30 to 50 years of age.

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic11.png "Loan amounts requested by age").


### [3rd Notebook (Linear Regression)](linear_regression.ipynb)

- Apart from using boxplots, barcharts and pie charts to test our features, we can use linear regression. Before we do this however, we must remember that the basic assumption underlying linear regression is that there is a normal distribution of variables. To ensure this we run a skew test with a result of 0 meaning a normally distributed dataset with a skew score of -+5 deemed as acceptable.

- Once that is done we check the different features for multicollearity using Pandas correlation matrix. Those with high correliniarity can be removed as features.

- We can then split our data into 2 sets, training and test. From now we'll run a regression on the train set alone. The outcome can be explained as below:

```

   p-value => probability of getting results as extreme as those witnessed [Ideal Value: <=0.05]
   [Read more here.](http://www.investopedia.com/terms/s/standard-error.asp). 

   Adjusted R Squared => Adjusted R Squared is an explanation of how much of the outcome features can be explained by the models explanatory features.

   Intercept => Predicted loan amount regardless of explanatory features

   [+-Feature] => The amount by which the loan amount (Located at the intercept) will be affected

```
- We first do univariate regressions (1 vairable) on each of our features from which we combine our best features in a multivariate regression. The output of a univariate regression should appear as below:

![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic%2012.png "Unvariate Regression").

### INTERPRATION
- From our diagram above we see a p-value of 0. A small p-value of <=0.05 says the result is not of random chance (which is a good result).
- The coefficient shows how the amount of influence the feature has on y which may be -ve or +ve.

- The result of our regression shows that a business owned by a business recieves on average a loan $28.7 less than the average loan amount. While having a p-value of 0 shows that we can be confident about this value, it has a low adjusted R-squared (0.001)showing that the predicting value of this value is very low. 

- We can try running a multivariate regression to improve our adjusted R-squared and we end up with the outcome below:

![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic13.png "Multivariate Regression").

- We used 24 different features using the sklearn template below with sector containing all the different loan sectors we found important:

'''

model = sm.ols(formula="loan_amount ~ borrower_count + sector + kids + gender + widowed + posted_year +\
                days_to_expire + partner_profitability + eco_friendly + animals + elderly", data=train_set)

model_results = model1.fit()

print(model_results.summary())


'''

From the image above our adjusted R-squares has improved to .211 as a result of the numerous features we've just added.
While .211 is a good improvement, ots not good enpugh. We begin by taking a look at out features again and only selecting the features that we think are important. After droping the features we dont need, we begin by checking for multicolliniarity to make sure that we don't have any unneccesary features. The following was the outcome:

	- gender_female and gender_male are perfectly correlated. 
	- sector_agriculture and activity_farming are correlated.
	- country_kiambu and province_central are highly correlated (They are basically the same place)
	- number of tags is very correlated with any given tag. This makes sense because most lenders only have one tag.

We then build a correlation matrix collecting the top correlations and remove county, activity and gender_male. The number of tags that a loan has are also removed as features as they appeared to have started use in later years.

![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic14.png "Loan amounts requested by age").

After working on correlation, we look at our final features and chexk them for any null values. Upon finding none, we split our data into trainig data and test data. We then run a multivariate regression on the remaining features using the training data. Once thats done, we run the same regression on our test data.

Both our training and test Adjusted R-squareds are low which suggests underfitting.

## DECISION TREES

### BAGGING
Upon running a single decision tree, our training data set has a very high R^2 of 0.99 while the test R^2 is low with 0.28. This may be a result of overfitting. 

![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/training.png "Training results").

![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/test2.png "Testing results").

To overcome this issue, we can use bagging with sklearns bagging regressor allowing us to measure the out of bag score which calculates the error rate of the predictions on the sample not used while training an individual tree. 

The training score has reduced while the test score has increased which is a good sign. The data appears to be overfitting less in the training set less, improving its ability to predict loan amounts on unseen samples.

![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/training2.png "Training results 2").

![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/test2.png "Testing results 2").

Upon increasing the number of trees from 10 to 100, we see a further imporovement on all scores. This tells us that splitting the dataset to more trees leads to a moire accurate prediction of the loan amount.

![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/splitting.png "Spliting results").

### RANDOM FOREST

There is still a large descripancy between the training and test data due to the high correlation in the decision trees. We can use a random forest regressor to de-correlate the trees as it only considers a random sub-sample of the features at each split.

![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/training3.png "Training results 3").

![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/test3.png "Testing results 3").


We then look at what features are driving our models preditions by looking at what percentage of our model each feature is driving. These features are then illustrated below:

![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/features.png "Explanatory features").

There doesnt appear to be a clear explanatory feature with the highest being above 20% but all the features combined lead to predictions of R^2=0.67  











