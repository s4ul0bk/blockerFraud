# The Blocker Fraud Company

The Blocker Fraud Company is a company specialized in detecting fraud in financial transactions made through mobile devices. The company has a service called “Blocker Fraud” in which it guarantees the blocking of fraudulent transactions.

And the business model of the company is of the Service type with the monetization made by the performance of the service provided, that is, the user pays a fixed fee on the success in detecting fraud in the customer's transactions.

However, the Blocker Fraud Company is expanding in Brazil and to acquire customers more quickly, it has adopted a very aggressive strategy. The strategy works as follows:

 * The company will receive 25% of the value of each transaction that is truly detected as fraud.
 * The company will receive 5% of the value of each transaction detected as fraud, but the transaction is truly legitimate.
 * The company will return 100% of the value to the customer, for each transaction detected as legitimate, however the transaction is truly a fraud.
 * With this aggressive strategy, the company assumes the risks of failing to detect fraud and is remunerated for assertive fraud detection.

For the client, it is an excellent business to hire the Blocker Fraud Company. Although the fee charged is very high on success, 25%, the company reduces its costs with fraudulent transactions correctly detected and the damage caused by an error in the anti-fraud service will be covered by the Blocker Fraud Company itself.

For the company, in addition to getting many customers with this risky strategy to guarantee reimbursement in the event of a failure to detect customer fraud, it depends only on the precision and accuracy of the models built by its Data Scientists, that is, how much the more accurate the “Blocker Fraud” model, the greater the company's revenue. However, if the model has low accuracy, the company could have a huge loss.


# The challenge

You have been hired as a Data Science Consultant to create a model of high precision and accuracy in detecting fraud of transactions made through mobile devices.

At the end of your consultancy, you need to deliver to the CEO of Blocker Fraud Company a model in production in which your access will be made via API, that is, customers will send their transactions via API so that your model classifies them as fraudulent or legitimate.

In addition, you will need to submit a report reporting your model's performance and results in relation to the profit and loss that the company will have when using the model you produced. Your report should contain the answers to the following questions:

* What is the model's Precision and Accuracy?
* How Reliable is the model in classifying transactions as legitimate or fraudulent?
* What is the Expected Billing by the Company if we classify 100% of transactions with the model?
* What is the Loss Expected by the Company in case of model failure?
* What is the Profit Expected by the Blocker Fraud Company when using the model?


# The Planning

Develop a Machine Learning Model that predict with 95% the frauds.


# The Dataset

The dataset is available on kaggle plataform(https://www.kaggle.com/ntnu-testimon/paysim1), but all the business context was extracted from page "Seja um Data Scientists (https://sejaumdatascientist.com/crie-uma-solucao-para-fraudes-em-transacoes-financeiras-usando-machine-learning/)


# The Solution


## Data Description

In this step i tried to know the dataset. We look to dataset, to find how big it is, how many columns and rows it have, if exists null rows, and some statisticals from data.
Two points made me attention on this step. The size of dataset (6MM of rows) and unbalanced between frauds(0,13%) and legithimcs transactions(99,87%).


## Feature Engieneering

After overview, we start to create Hypotheses about data. I did this step before analyse all data to avoid be influenced by what i saw. I start to create a Mind map with all features of dataset and try get insights by this features.

![hypotheses](/images/hypothesesMindMap.png)

**H1** - Most Frauds occurs between days 5 and 15

**H2** - Most Frauds occurs on afternoon

**H3** - Most Frauds occurs on month's second week

**H4** - Most Frauds occurs on Transfer Transactions




After hypotheses, i try derivate new features that could help to explain the frauds. Some new features was created:

**week** - Transform hour of month information in week of month

**day** - Transform hour of month  information in day of month

**hour** - Transform hour of month information in hour of day

**origType** - Transform customer name information in customer type to another feature

**nameOrigNumber** - Transform customer name information in customer id

**destType** - Transform customer name information in customer type to another feature

**nameDestNumber** - Transform customer name information in customer id

**diffOrig** - Delta between old Balance and new Balance based on Amount Transaction. In regular case must to be equal zero

**diffDest** - Delta between old Balance and new Balance based on Amount Transaction, in transfer transactions to regular customer. Anothers transactions types dont update dest Balance. Mercatil customer dont have update on destBalance. In regular case must to be equal zero

**qtdTransferOrigName** - Quantity transactions from accountName

**qtdTransferDestName** - Quantity transactions to accountName



The features diffOrig,diffDest,qtdTransferOrigName,qtdTransferDestName increased significantly the performance of model. 



## Exploratory Data Analysis

In this step i try look insights, values and informations about the business and fenomenos. I Applied logarithm to allow see behavior of frauds, because of unbalanced proportion of frauds e legitimacy transactions.

The points made me attention was the balance difference between old balance and new balance by amount transaction.

The most fraud transaction was negative balance on field diffOrign. The amount transfer was higher than orig balance.

![diffOrig](/images/diffOrig.png)

The most fraud transaction was positive balance on field diffDest. The amount transfer no credit the account.

![diffDest](/images/diffDest.png)

The most frauds, have 1 transaction on account orig.

![qtdTransferOrig](/images/qtdTransferOrig.png)

The most frauds, have several transactions to account dest.

![qtdTransferDest](/images/qtdTransferDest.png)

In this step i try to validate all Hypotheses created.


**Hypothese 1** - Most Frauds occurs between days 5 and 15

**False** most fraud occurs after the 15th

**Hypothese 2** - Most Frauds occurs on afternoon

**True** On afternoon and dawn occours the most quantity of frauds

**Hypothese 3** - Most Frauds occurs on month's second week

**False** Between week 1 and 4 the quantity of fraud was close

**Hypothese 4** - Most Frauds occurs on Transfer Transactions

**False** The quantity of fraud was close in all Type Transactions  


## Data preparation

In this step i prepare the data to be processed by a Machine Learning Model. We apply some transformation of data according to your characteristic.

**Rescaling** - step,qtdTransferOrigName,qtdTransferDestName,oldbalanceOrg,newbalanceOrig,oldbalanceDest,newbalanceDest,week,day,nameOrigNumber,nameDestNumber,diffOrig,diffDest. In this features was applied reescaling to change the range of data, by whitout change the fenomenos.

**Enconding** - type,origType,destType - The Most of Machine Learning Model dont work with categorical feature, so i need to transform this feature in numerical types.Was applied dummy transformation. This transformation make a new column to each variety of possible data.

**Nature Transformation** - hour - Applied transformation sin and cos, to create a event cycle. This transformation helps the model understand  the spacing of data.


## Feature Selection

In this step i try reduce the features to minimal necessary to explain to Machine Learning Model the fenomenos. This is necessary based the principal of Occam's Razor, and this reduction influce significantily on model performance and the comportament of frauds.

We chouce the columns using 2 techniques: 
* Feature Importance  - using a Random Forest Regressor that calculate the influency of feature on fenomenos
* Boruta  - using a Forest to find the best features that explain the fenomenos

Based on result we continue the processes whit features: amount,oldbalanceOrg,newbalanceOrig,oldbalanceDest,newbalanceDest,diffOrig,diffDest,day,hour_sin,hour_cos,type_CASH_OUT,type_TRANSFER,qtdTransferOrigName,qtdTransferDestName.


## Machine Learning Models

In this step we applied some machine learning models to try predict the frauds.
I used in this step the models:
* Random - Baseline
* Ridge Classifier
* XGBoost Classifier
* Random Forest Classifier

The perfromance result indicate that i continue with XGBoost and Random Forest Models.
![modelsPerformance](/images/modelsPerformance.png)


## Models Performance

In this step i try to validate the performance resultaded from previous step. I applied the cross validation, that is a tecnique to split the trainning dataset in smaller peaces and try to predict the next piece . This step is necessary to understand if the model is not overffiting, that is when model memorize the dataset, but it cant predict data that it never saw.

XGBoost with tradicional parameters

![crossValidationXGBoost](/images/crossValidationXGBoost1.png)

XGBoost with parameters to unbalanced data

![crossValidationXGBoost](/images/crossValidationXGBoost2.png)

Random Forest

![crossValidationRandomForest](/images/crossValidationRandomForest.png)


## Tunning High Parameters

In this step i'll search the best parameters to execute the XGBoost Model.
The best parameters founded: 

{'n_estimators': 500,

         'eta': 0.03,

         'max_depth': 9,

         'subsample': 0.7,

         'colsample_bytree': 0.7,

         'min_child_weight': 15}


## Models Test Predict

In this step, i execute the last predict with the models, after tunning, and now i used the test data. Until now, all execution was maded with trainngin e validation data. With this result we can see if the real performance and efficientily of the model.

XGBoost Final Performance

![xgboostFinalPerformance](/images/xgboostFinalPerformance.png)

Random Forest Final Performance

![randomForestFinalPerformance](/images/randomForestFinalPerformance.png)


## Final Model

The models performance were similar, beeing "Random Forest Model" with 99,51% of Precision/Recall and "XGBoost Model" with 99,57% of Precision/Recall. However Random Forest presents best execution performance, executing about 33% faster then "XGBoost". The pickle exports of "Random Forest" also show significantily smaller, beeign 78% smaller then "XGBoost". So we'll continue with "Random Forest Model".


## The Result - Final Model

In test data set have 1.272.524 transcation, being 1643 frauds transactions, totalizing $2,507,288,036.22$ of frauds. 

Our model is able to detected 99,51 percent of all frauds, avoiding $ 2,504,426,229.95$ of frauds.

Frauds not detected by our model, totalize 0,49 percentl of all frauds, not avoiding $2,861,806.27$ of frauds. All this amount will be refound to customer.

In our bussiness strategy, we are able to generate a economy of $1,881,181,478.73$ **to customer**, generating a revenue of $623,244,751.22$ to **Blocker Fraud Company**.


# Who i am

My name is Saulo Ferreira Cunha, IT student since 2004 and i'm a Data scientist in formation. 

Email: saulofcunha@outlook.com

Linkedin: https://www.linkedin.com/in/saulo-ferreira-cunha-6a6ba232/