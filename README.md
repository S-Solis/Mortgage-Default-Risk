# Mortgage-Default-Risk
This Jupyter Notebook project contains a comparison of multiple machine learning models to predict whether a customer is likely to default on their mortgage with little to no credit history.

## Index
[Project Brief](#Project-Brief)
[Environment Set Up](#Environment-Set-Up)
[Project Outline](#Project-Outline)
[Project Findings](#Project-Findings)

## Project Brief
Home Credit's "responsible lending model empowers customers with little or no credit history to access financing, enabling customers to borrow easily and safely, both online and offline" [[1]](https://www.homecredit.net/about-us.aspx/#who-we-are). With limited credit history, how can Home Credit leverage machine learning to find hidden trends in the data they do have and estimate mortgage default risk for a prospective borrower?

Based on data from 7 tables, I build 4 classification models to quantify the risk that a prospective borrower will default on their mortgage. I conclude by comparing the accuracy of the 4 models. The models are as follows:
1. A classification tree based on my hypothesis of the top 10 predictive features.
2. A random forest classifier to find the true top 10 predictive features.
3. A re-engineered classification tree based on the true top 10 predictive features.
4. A random forest classifier with only the top 10 predictive features, which performs more efficiently than the random forest built on all features.

This project utilizes a [dataset available on Kaggle](https://www.kaggle.com/competitions/home-credit-default-risk) provided by Home Credit.

## Environment Set Up
1. Install Jupyter Notebook via Anaconda. Instructions are available [here](https://docs.jupyter.org/en/latest/install/notebook-classic.html).
2. Download the [dataset available on Kaggle](https://www.kaggle.com/competitions/home-credit-default-risk).
3. Unzip the data files and rename "application_train.csv" to "default_risk.csv". Note that I performed this step to standardize the table naming conventions across the csv files. Rename the unzipped repository to "data" to maintain the links within the Jupyter Notebook.

## Project Outline
- Part 0: Exploratory Data Analysis: In this section, data is loaded into data frames and explored. I hypothesize the top ten data features on which to built a basic decision tree. I splice the data on the primary key for the customer, merging multiple tables to build the final dataframe used for training.
- Part 1: Build a basic classification tree using the top ten hypothesized data features.
- Part 2: Build a random forest (RF) to determine the actual top ten features.*
- Part 3: Re-build a basic classification tree using the actual top ten features, as identified by the RF.
- Part 4: Re-build a new random forest using only the actual top ten features (enhancing run-time/model efficiency vs. 150+ features).
- Part 5: Compare the models' accuracy, confusion matrices, and AUC's. Select the model to use for productionalization.
- Part 6: Export the list of customers identified to be at risk of defaulting on their mortgages.

## Project Findings



*Note that due to the randomness of the train/test data splits and the random forest, many of the models' results can vary per run of the notebook. This variability can impact the models' calculated accuracies, areas under the curve (AUC's), type I/II error counts, and even the final top ten features selected by the random forest.

