# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
**In 1-2 sentences, explain the problem statement: e.g "This dataset contains data about... we seek to predict..."**
The data is related with direct marketing campaigns of a Portuguese banking institution. The marketing campaigns were based on phone calls. Often, more than one contact to the same client was required, in order to access if the product (bank term deposit) would be ('yes') or not ('no') subscribed. We are trying to predict whether the client subscribed a term deposit or not. If yes it is labeled as 'y', otherwise 'n'. To predict this, we have 20 explanatory features which includes age, job, marital, education and so on.

**In 1-2 sentences, explain the solution: e.g. "The best performing model was a ..."**
Our best performing model is: VotingEnsemble
Based on this model most impactful feature is: duration 


## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**
In this architecture, first I optimized my model with hyperdrive, then with Azure AutoML. 
First I set up a training script and created tabular dataset in train.py file. Dataset is coming from marketing campaigns of Portuguese banking institution. Before any other steps, the data has to be cleaned, and it is carried out by clean_data function. Shortly, in this function, I replaced text values with binary numbers. For example, in the martial column, I replaced marred values with 1, otherwise 0.
Then I split my data to training and testing, and the ratio is 4:1. I chose Logistic Regression from Scikit-Learn library for this classification problem.   
After setting train.py I move on the jupyter notebook and used HyperDrive to find optimal hyperparameters. For tuning I chose regularization parameter and max iteration number as my parameter sampler. I introduce Bandit policy with slack factor 0.3, evaluation interval 5 and delay evaluation 10. I used SKLearn to create estimator. My primary metric is Accuracy. I allowed maximum of 5 total runs in order to save time. 
After HyperDrive found an optimal tuning parameters by its powerful algorithm and combined with random search, I created a tabular dataset and notebook to find another optimized model, but this time with AutoML. In my script I specified machine learning algorithm as LogisticRegression, but with AutoML I am searching other classification algorithms and their optimal tuning parameters to solve my problem. Again, primary metric is Accuracy. Since limited time, I set 30 for experiment timeout minutes. Similarly, I did not enable deep learning algorithms but I also did not block any machine learning algorithms. Since the previous method yield 0.90 accuracy, I set experiment exit score as 0.95 I saved my best model by onnx.


**What are the benefits of the parameter sampler you chose?**
Hyperparameter tuning is the process of finding the configuration of hyperparameters that results in the best performance. The process is typically computationally expensive and manual. However, it is one of the core parts of any machine learning projects. Without proper tuning, we cannot make good use of powerful algorithms. I chose regularization and max iter parameters to be tuned. Smaller values of C specify stronger regularization. Max iter is Maximum number of iterations taken for the solvers to converge. There are also other tuning parameters but because of the time constraint I could not optimize them. And again because of the time constraint, I apply random sampling.

**What are the benefits of the early stopping policy you chose?**
Thanks to early stopping policy, program terminates poorly performing runs. Early termination improves computational efficiency. I choose Bandit Policy. Any run whose best metric is less than (1/(1+0.3)  of the best performing run will be terminated.

## AutoML
**In 1-2 sentences, describe the model and hyperparameters generated by AutoML.**
Best Model: Voting Ensemble, its accuracy is 0.91548
According to this model, the most important features are duration, nr.emploted,eurobor,cons.cof.idx,..
Parameters:
Its ensembled algortihms are XGBoostClassifier', 'XGBoostClassifier', 'LightGBM', 'LightGBM', 'SGD', 'SGD', 'ExtremeRandomTrees'

## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**
Model generated by AutoML is slightly better than the first one.Accuracy is increased around 1 percent. This is very expected, actually I was expecting little bit more improvement. Since AutoML applies feature selection, model selection, hyperparameter tuning by its powerful algorithm.

## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**
Maybe we can add more data to the model, or we can add more columns. Also we can make new columns with existing ones with future engineering, not future selection. As it is discussed in the lectures, thanks to AutoML, engineers/scientist play more role in modeling process. We have to apply our domain knowledge and obtain better results.
Also for this experiment, I did not want to spend too much money since I am working on my own azure account. However, I should have definitely tried at least 50 models.


