# Mortgage-Default-Risk
This project contains a comparison of multiple machine learning models to predict whether a customer is likely to default on their mortgage with little to no credit history. The project leverages Python, Jupyter Notebooks, and SQL.

## Index
[Project Brief](#Project-Brief)

[Tech Tools & Credits](#Tech-Tools--Credits)

[Setting Up the Environment](#Setting-Up-the-Environment)

[Project Outline](#Project-Outline)

[Project Findings](#Project-Findings)

## Project Brief
Home Credit's "responsible lending model empowers customers with little or no credit history to access financing, enabling customers to borrow easily and safely, both online and offline" [[1]](https://www.homecredit.net/about-us.aspx/#who-we-are). With limited credit history, how can Home Credit leverage machine learning to find hidden trends in the data they do have and estimate mortgage default risk for a prospective borrower?

Based on data from 7 tables for over 300,000 customers, I build 4 classification models to quantify the risk that a prospective borrower will default on their mortgage. I conclude by comparing the accuracy of the 4 models. The models are as follows:
1. A classification tree based on my hypothesis of the top 10 predictive features.
2. A random forest classifier to find the true top 10 predictive features.
3. A re-engineered classification tree based on the true top 10 predictive features.
4. A random forest classifier with only the top 10 predictive features (for efficiency gains).

## Tech Tools & Credits
This project leverages:
- a [dataset available on Kaggle](https://www.kaggle.com/competitions/home-credit-default-risk) provided by Home Credit
- scikit learn's [DecisionTreeClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html)
- scikit learn's [RandomForestClassifier])https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)
- scikit learn's [roc_auc_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)
- scikit learn/s [confusion_matrix](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html)
- seaborn's [heatmap](https://seaborn.pydata.org/generated/seaborn.heatmap.html)
- matplotlib's [pyplot](https://matplotlib.org/3.5.3/api/_as_gen/matplotlib.pyplot.html)

## Setting Up the Environment
Jupyter Notebook:
1. Install Jupyter Notebook via Anaconda. Instructions are available [here](https://docs.jupyter.org/en/latest/install/notebook-classic.html).
2. Download the [dataset available on Kaggle](https://www.kaggle.com/competitions/home-credit-default-risk).
3. Unzip the data files and rename "application_train.csv" to "default_risk.csv". Note that I performed this step to standardize the table naming conventions across the csv files. Rename the unzipped repository to "data" to maintain the links within the Jupyter Notebook.

MySQL Workbench:
1. Install MySQL workbench. Here is a [YouTube video](https://www.youtube.com/watch?v=7S_tz1z_5bA&t=300s) with installation instructions.
2. Build a new connection and double click to enter it.
3. Under Administration > User and Privileges > Administrative Roles, ensure your appropriate roles have DBA-level (unrestricted) access. This ensures no user restrictions and that data can be loaded. (Note that this is **not** recommended for a production environment.)
4. Setting the configuration file:
   1. Navigate to the workbench home page, right click the connection, and select "Edit Connection".
   2. Set the "Configuration File" setting to the location of the my.cnf file included. Note that this file includes settings that are **not** recommended for a production environment for security reasons.
5. Open and run the file CREATE_TABLES_LOAD_DATA.sql. (Note: Update the filepaths as needed depending on where the .csv files are saved locally.)
6. Run queries to explore the data as needed.

## Project Outline
- Part 0: Exploratory Data Analysis: In this section, data is loaded into data frames and explored. I hypothesize the top ten data features on which to built a basic decision tree. I splice the data on the primary key for the customer, merging multiple tables to build the final dataframe used for training.
- Part 1: Build a basic classification tree using the top ten hypothesized data features.
- Part 2: Build a random forest (RF) to determine the actual top ten features.*
- Part 3: Re-build a basic classification tree using the actual top ten features, as identified by the RF.
- Part 4: Re-build a new random forest using only the actual top ten features (enhancing run-time/model efficiency vs. 150+ features).
- Part 5: Compare the models' accuracy, confusion matrices, and AUC's.* Select the model to use for productionalization.
- Part 6: Export the list of customers identified to be at risk of defaulting on their mortgages.

## Project Findings
Below are the accuracy scores and area under the curve (AUC) of each tree/forest iteration during my final runs for each of them. (Note that these may vary slightly per run, based on the randomness of some of the train/test splits and the Random Forest.)

| | **Top 10 Single Tree** | **Random Forest** | **New Top 10 Single Tree** | **New Random Forest** 
:-|:-:|-:|-:|-:
**Accuracy**|91.81%|91.78%|91.87%|91.87%
**TN / FP**|40014 / 67|40002 / 0|39810 / 158|39962 / 6
**FN / TP**|3479 / 16|3573 / 1|3543 / 65|3599 / 9
**AUC**|.71|.75|.72|.74

All trees ran with about 92% accuracy, with little variation between all of them. The accuracy did go up slightly from my guessed top 10 and the actual top 10 features (as identified by the random forest). I would expect this, since the random forest can more programmatically determine the top ten variables. The AUC for each model is also quite close, with all falling between .7 and .75. However, when breaking these results down into true negatives, false positions, false negatives, and true positives (TN, FP, FN, and TP respectively), it's clear that most of the models struggled to positively identify any customers at risk of default.

I would still be wary of using this algorithm to flag possible default risk/lending decisions. As seen in the variables selected, there is a chance that the algorithms are all perpetuating age or gender bias that's present in the data. Given the four confusion matrices, I would recommend utilizing the true top 10 single tree. However, I want to stress that this data should not be used to make lending decisions without a human underwriter confirming the findings. 


*Note that due to the randomness of the random forest, many of the models' results can vary per run of the notebook. This variability can impact the models' calculated accuracies, areas under the curve (AUC's), type I/II error counts, and even the final top ten features selected by the random forest.

