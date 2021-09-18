# Zindi_DSN_2020_ExpressoChurnPrediction
Solution code of my 6th place submission to Data Science Nigeria's 2020 Pre-Bootcamp Hackthon  on Zindi.africa
## Problem Statement
This hackathon was organized by Data Science Nigeria (DSN) on Zindi.africa. The goal is to develop a predictive model that determines the likelihood of a customer to churn from Expresso, an African telecommunications company that provides customers with airtime and mobile data bundles in Mauritania and Senegal. 'Churn' was conceptualised as the customer becoming inactive and not making any transactions for 90 days. Insights gained from this project will help Expresso to understand which customers are at risk of leaving so they can find ways to better serve their customers and improve customer retention. This problem was treated as a classification problem.
## About the data
The available data contained information on 500 000 customers collected from the Senegal market. This was divided into train set [400 000 rows, 19 columns] and test set [100 000 rows, 18 columns], consisting of 14 numeric variables and 5 categorical variables.
Below is the list of the variables and their description:

![alt text](https://github.com/adeyinkaoresanya/Zindi_DSN_2020_ExpressoChurnPrediction/blob/master/variable%20description.PNG "Variable description")

Further information about the dataset and the company is available at https://zindi.africa/hackathons/dsn-pre-bootcamp-hackathon-expresso-churn-prediction-challenge

## Exploratory Analysis
* The data contained considerable amount of missing values--not less than 33%--in 14 out of 19 columns. Patterns were observed around the missing data as variables that were closely linked to each other had the same amount of missing values.

* The target variable had imbalanced classes--18.7% of the customers churned, which implies that Expresso had more of loyal customers than those who churned.

*	Majority of the customers who churned were from the city of Dakar (42.1%), have been with Expresso for more than two years (92.7%), earned less and spent less to top up and for data volume. They also called less on either Expresso, Tigo or Orange and did not subscribe to the top active packs (87.6%). Furthermore, on the average, the lines of customers who churned were not active for more than 6 times in the course of 90 days, whereas a typical loyal customer's line was active for more than 30 times in the same period. Some of the graphs are shown below:

![alt text](https://github.com/adeyinkaoresanya/Zindi_DSN_2020_ExpressoChurnPrediction/blob/master/churn%20vs%20regularity.PNG "churn by regularity")
![alt text](https://github.com/adeyinkaoresanya/Zindi_DSN_2020_ExpressoChurnPrediction/blob/master/churn%20vs%20data%20volume.PNG "churn by data volume")
![alt text](https://github.com/adeyinkaoresanya/Zindi_DSN_2020_ExpressoChurnPrediction/blob/master/churn%20vs%20inter-expresso%20call.PNG "churn by inter-expresso call")

*	Some variables such as ‘REVENUE’ and ‘MONTANT’ contained extreme outliers, which is expected of income distribution. Research on the evaluation metric, logloss, showed that it is sensitive to outliers. Thus, a scaling technique that is robust to outliers will be required to standardize the data.

![alt text](https://github.com/adeyinkaoresanya/Zindi_DSN_2020_ExpressoChurnPrediction/blob/master/churn%20vs%20income.PNG "churn by income")
![alt text](https://github.com/adeyinkaoresanya/Zindi_DSN_2020_ExpressoChurnPrediction/blob/master/churn%20vs%20top-up%20amount.PNG "churn by top-up amount")


## Data Cleaning
*	For the variable ‘TOP_PACK’, which represents the most active packages, it was assumed that the missing values represented customers who did not subscribe to the most active packages listed. Therefore, they were grouped under another class, ‘others’, for further analysis. On plotting against ‘CHURN’, it was discovered that majority of those who churned belonged to the ‘others’ class. On the average, customers under this class were active for just about 10 times in 90 days. Therefore, it was safe to move along with the earlier assumption and so missing values in ‘FREQ_TOP_PACK’, which is the number of times the customer has activated the top packages, were filled with zeros.

*	Missing values in ‘REGION’ variable were also grouped under a new category named ‘missing’ while those in numeric variables were imputed with arbitrary value, -99.

## Feature Engineering

*	Label encoding was performed on 'TOP_PACK' because of its numerous categories while 'REGION' was transformed into dummy variables. Every other feature was used as is.

* RobustScaler from Scikit-learn was employed to standardize the features. This type of scaler standardizes features by using statistics that are robust to outliers. This is done by removing the median and scaling the data according to the quantile range.

*	No additional feature was generated from the data set.

## Model Building and Evaluation

*	Three models were built using LightGBM and CatBoost algorithms using a 10-fold cross-validation strategy. StratifiedKFold from Scikitlearn was employed due to the target class imbalance. Hyperparameter optimization was carried out on each algorithm using RandomizedSearchCV in order to get the best parameters for the algorithms. Afterwards, VotingClassifier was used to ensemble the model which resulted in a logloss of 0.246.

*	On graphing the feature importances, it was discovered that ‘REGULARITY’, that is, the number of times the customer was active in 90 days, was the most important variable in predicting whether a customer will churn after 90 days or not. This was immediately followed by ‘DATA_VOLUME’ (number of connections), ON-NET (inter-Expresso call), and ‘REVENUE’ (monthly income of customer).

![alt text](https://github.com/adeyinkaoresanya/Zindi_DSN_2020_ExpressoChurnPrediction/blob/master/Feature%20importances.png "Feature importances")

## Conclusion
Customers of Expresso Telecommunications are likely to churn if they are not active for more than 10 times in 90 days. Important factors to also consider include monthly income of the customer, volume of Internet connections and number of times inter-Expresso calls are made.

## Recommendations
* A customer micro-segmentation should be conducted in oder to offer personalised incentives that the likely churner would find hard to refuse.
* Set up a customer engagement strategy for preventing customer inactivity.
