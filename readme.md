# The Blocker Fraud Company

![Fraud Detection](/images/fraud_header.jpg)

# Business Problem

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

I have been hired as a Data Science Consultant to create a model of high precision and accuracy in detecting fraud of transactions made through mobile devices.

At the end of your consultancy, you need to deliver to the CEO of Blocker Fraud Company a model in production in which your access will be made via API, that is, customers will send their transactions via API so that your model classifies them as fraudulent or legitimate.

In addition, you will need to submit a report reporting your model's performance and results in relation to the profit and loss that the company will have when using the model you produced. Your report should contain the answers to the following questions:

* What is the model's Precision and Accuracy?
* How Reliable is the model in classifying transactions as legitimate or fraudulent?
* What is the Expected Billing by the Company if we classify 100% of transactions with the model?
* What is the Loss Expected by the Company in case of model failure?
* What is the Profit Expected by the Blocker Fraud Company when using the model?


# The Solution Strategy

Develop a Machine Learning Model that predict with 95% the frauds.

**Step 01. Data Description** : Use statistics metrics to identify data distribuctions.
**Step 02. Feature Engieneering** : Create new features on the original that better describe the fenomonous.
**Step 03. Exploratory Data Analysis** : Explore data to find insights and the features that better describe the fenomonous.
**Step 04. Feature Selection** : Select the most importante feature that better describe the fenomonous.
**Step 05. Machine Learning Models** : Machine Learning Model Trainning
**Step 06. Tunning Hyper Parameters** : Find the best values of each parameter of the select Model.
**Step 07. Convert Model Performance to Business Values** : Convert the performance of the Machine Learning model into a business result.

# The Dataset

The dataset is available on kaggle plataform(https://www.kaggle.com/ntnu-testimon/paysim1).


# Top Data Insights 

The balance difference between old balance and new balance by amount transaction.

The most fraud transaction was negative balance on field diffOrign. The amount transfer was higher than orig balance.

![diffOrig](/images/diffOrig.png)

The most fraud transaction was positive balance on field diffDest. The amount transfer no credit the account.

![diffDest](/images/diffDest.png)

The most frauds, have 1 transaction on account orig.

![qtdTransferOrig](/images/qtdTransferOrig.png)

The most frauds, have several transactions to account dest.

![qtdTransferDest](/images/qtdTransferDest.png)


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

XGBoost Final Performance

![xgboostFinalPerformance](/images/xgboostFinalPerformance.png)

Random Forest Final Performance

![randomForestFinalPerformance](/images/randomForestFinalPerformance.png)


## Final Model

The models performance were similar, beeing "Random Forest Model" with 99,51% of Precision/Recall and "XGBoost Model" with 99,57% of Precision/Recall. However Random Forest presents best execution performance, executing about 33% faster then "XGBoost". The pickle exports of "Random Forest" also show significantily smaller, beeign 78% smaller then "XGBoost". So we'll continue with "Random Forest Model".


## Convert Model Performance to Business Values

In test data set have 1.272.524 transcation, being 1643 frauds transactions, totalizing $2,507,288,036.22$ of frauds. 

Our model is able to detected 99,51 percent of all frauds, avoiding $ 2,504,426,229.95$ of frauds.

Frauds not detected by our model, totalize 0,49 percentl of all frauds, not avoiding $2,861,806.27$ of frauds. All this amount will be refound to customer.

In our bussiness strategy, we are able to generate a economy of $1,881,181,478.73$ **to customer**, generating a revenue of $623,244,751.22$ to **Blocker Fraud Company**.


# Who i am

My name is Saulo Ferreira Cunha, IT student since 2004 and i'm a Data scientist in formation. 

Email: saulofcunha@outlook.com

Linkedin: https://www.linkedin.com/in/saulo-ferreira-cunha-6a6ba232/