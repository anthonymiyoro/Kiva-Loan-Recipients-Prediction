# Insights from Kiva Data

### This repository contains a seriess of notebooks with which we try to predict the leading factors that can predict the loan amount someone can recieve from Kiva

In the first notebook we summarise the main characterisitcs of the dataset so as to know which algorithm to use in the future.

[1st Notebook (Exploratory Analysis)](exploratory_analysis.ipynb)

- We first merge our 3 datasets into 1 (df.csv)

- After this, we check the variables in our dataset. In our first example from cell 19 to 26 we convert an object (in panda format) to datetime format.

- Once thats done, we check for missing values in the dataset. When we're done with exploratory analysis we then move on to feature engineering.


[2nd Notebook (Feature Engineering)](exploratory_analysis.ipynb)

- With feature engineering, we begin to attempt to answer what factors drive the loan amounts requested by Kiva borrowers.

- After importing all the data created from the previous notebook, we start by removing the outlier loans.

- The outlier loans are those whose amounts fall outside 3 standard deviations from the mean. After running 
```
# dataset without outliers
df_removed_outliers=df[(((df['loan_amount']-df['loan_amount'].mean())/(df['loan_amount'].std())).abs()<3)] 

```
Our data becomes much less skewed 
Reference-style: 
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

