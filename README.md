# BankPredict
This repository contains the code and analysis of direct marketing campaigns of a Portuguese banking institution. The marketing campaigns were based on phone calls. Often, more than one contact to the same client was required, in order to access if the product (bank term deposit) was ('yes') or was not ('no') subscribed. 

The dataset can be found [here](https://archive.ics.uci.edu/ml/datasets/Bank+Marketing).


## Initial Analysis
The first step in analysing the data was applying Decision Tree, Random Forest and Support Vector Machine on the dataset.
The results of the analysis were as follows:

1. Decision Tree
     * Training Accuracy: 89.25%
     * Validation Accuracy: 68.86%
     * f1-score: 0.158

    ![Alt text](https://github.com/shivangchopra11/BankPredict/blob/master/Assets/ConfDTFull.png)

2. Random Forest
     * Training Accuracy: 89.32%
     * Validation Accuracy: 69.38%
     * f1-score: 0.024
     
     ![Alt text](https://github.com/shivangchopra11/BankPredict/blob/master/Assets/ConfRFFull.png)
     
3. Support Vector Machine
     * Training Accuracy: 89.07%
     * Validation Accuracy: 69.16%
     * f1-score: 0.0007
     
     ![Alt text](https://github.com/shivangchopra11/BankPredict/blob/master/Assets/ConfSVMFull.png)

An important aspect revealed in this analysis was the the duration attribute was hugely influencing the predictions. However, as pointed out by the data documentation, the duration of call is not known in prior and hence is not a valid metric to be used for inference. Hence, it was removed from the dataset.

## Applying PCA
To reduce the sparsity in the data in order to obtain better inferences, PCA was applied to the model and the number of features was reduced from 20 to 5. The results after applying PCA were as follows:

1. Decision Tree
     * Training Accuracy: 88.94%
     * Validation Accuracy: 67.62%
     * f1-score: 0.220
     
     ![Alt text](https://github.com/shivangchopra11/BankPredict/blob/master/Assets/ConfDTFullPCA.png)
     
2. Random Forest
     * Training Accuracy: 89.34%
     * Validation Accuracy: 69.19%
     * f1-score: 0.035
     
     ![Alt text](https://github.com/shivangchopra11/BankPredict/blob/master/Assets/ConfRFFullPCA.png)
     
3. Support Vector Machine
     * Training Accuracy: 88.95%
     * Validation Accuracy: 69.11%
     * f1-score: 0.0015
     
     ![Alt text](https://github.com/shivangchopra11/BankPredict/blob/master/Assets/ConfSVMFullPCA.png)

## Upsampling to reduce Class Bias
A deeper analysis of the data reveals a huge class bias towards the negative class. This bias induces a huge degrading effect on the prediction of the models. In order to reduce this bias, the positive class was upsampled to about 1/2 of the negative samples. The results after upsampling were as follows:

1. Decision Tree
     * Accuracy on upsampled data: 84.79%
     * Accuracy on original data: 89.80%
     * f1-score: 0.6485
     
     ![Alt text](https://github.com/shivangchopra11/BankPredict/blob/master/Assets/DTUpsampledFull.png)
     
2. Random Forest
     * Accuracy on upsampled data: 85.81%
     * Accuracy on original data: 90.65%
     * f1-score: 0.6988
     
     ![Alt text](https://github.com/shivangchopra11/BankPredict/blob/master/Assets/RFUpsampledFull.png)
     
3. Support Vector Machine
     * Accuracy on upsampled data: 88.49%
     * Accuracy on original data: 92.86%
     * f1-score: 0.797
     
     ![Alt text](https://github.com/shivangchopra11/BankPredict/blob/master/Assets/SVMUpsampledFull.png)


## Final Analysis
The complete analysis of the data reveals the following important observations:

1. SVM applied on the upsampled data gives the best results and reaches an Accuracy of about 92.86% and an f1-score of 0.797. The SVM was initially performing very poorly due to the huge class bias which was taken care by the upsampling.
2. The main Principal Components and the attributes they were mapped to were as follows:
    * PC1 - pdays
    * PC2 - nr.employed
    * PC3 - age
    * PC4 - cons.conf.idx
    * PC5 - job
3. The attributes with the most information gain in the Decision Tree were as follows:
    * nr.employed
    * pdays
    * cons.conf.idx
    * age
    * contact

It is clear from the above observations that `pdays` (number of days that passed by after the client was last contacted from a previous campaign), `nr.employed` (number of employees), `age`, and `cons.conf.idx` (consumer confidence index) were the factors that had the greatest influence on the outcome of the call.

The following conditions will ensure a greater response rate from the customers:

1. nr.employed <= 5087
2. pdays <= 0.725
3. age <= 67.5
4. conf.idx <= -28.35
5. job: {admin, blue-collar, entrepreneur, housemaid}

Therefore, in order to increase the revenue, no of employes employed should not be increased beyond 5087 as increasing them won't recover the cost of their salary as more potential customers cannot be tapped. The customers should be contacted frequently to avoid loss of potential customers and thereby increasing the response. Moreover, the main target customers should be the ones having a blue-collar jobs, Entrepreuners and housemaids as they will be the ones most interested and benefitted from the bank policy.
