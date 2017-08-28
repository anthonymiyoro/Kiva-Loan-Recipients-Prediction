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

	- Males ask for larger loan amounts compared to women

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic3.png "Gender Loan amounts")

	- Widowed couples ask for lower loan amounts than non-widowed couples.

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic4.png "Marital Status")

		Men asked for more loans regardless of marital status.

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic9.png "Marital Status 2")

	- The elderly ask for smaller loan amounts.

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic5.png "Elderly Loan amounts")

	- Borrowers with children requested for lower loan amounts

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic6.png "Children Loan amounts").

		Those with 2 to 6 children asked for the highest amount in loans with the amount decreasing with an increase in the number of children.

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic10.png "Children Loan amounts2").

	- Surprisingly, the elderly appeared to ask for higher loan amounts from the ages of 60 to 80 while females of 30 years of age asked for the lowest loan amounts on average. For males, the loan amounts requested stagnated from 30 to 50 years of age.

	![alt text](https://github.com/anthonymiyoro/kivaData/blob/master/photos/pic11.png "Loan amounts requested by age").






