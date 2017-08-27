# Insights from Kiva Data

### This repository contains a seriess of notebooks with which we try to predict the leading factors that can predict the loan amount someone can recieve from Kiva

In the first notebook we summarise the main characterisitcs of the dataset so as to know which algorithm to use in the future.

[1st Notebook (Exploratory Analysis)](exploratory_analysis.ipynb)

- We first merge our 3 datasets into 1 (df.csv)

- After this, we check the variables in our dataset. In our first example from cell 19 to 26 we convert an object (in panda format) to datetime format.

- Once thats done, we check for missing values in the dataset. When we're done with exploratory analysis we then move on to feature engineering.


[2nd Notebook (Feature Engineering)](exploratory_analysis.ipynb)

- With feature engineering, we begin to attempt to answer what factors drive the loan amounts requested by Kiva borrowers.

- After importing all the data created from the previous notebook, we start by removing the outlier loans. As we can see from the image below:
![alt text][photos/pic1]

- The outlier loans are those whose amounts fall outside 3 standard deviations from the mean. After running 
```
# dataset without outliers
df_removed_outliers=df[(((df['loan_amount']-df['loan_amount'].mean())/(df['loan_amount'].std())).abs()<3)] 

```
Our data becomes much less skewed as below:
![alt text][photos/pic2]

Unfortunately upon looking at some of the data we see that most of the loans are in fact around $250 so instead we'll remove all loans hogher than $6000 as those are rare. 

- Since our data looks much less skewed we can begin working on selecting the features we deem most important. These features that determine how much we ask for in a loan can be found by asking:
			- What job does the borrower have?
			- Have you taken a loan before?
			- Who is lending the money?
			- Where do you live?

- In the following cells, we went through these features and found that:

	- Males ask for larger loan amounts compared to women
	![alt text][photos/pic3]
	- Widowed couples ask for lower loan amounts than non-widowed couples
	![alt text][photos/pic4]
	- The elderly ask for smaller loans
	![alt text][photos/pic4]





