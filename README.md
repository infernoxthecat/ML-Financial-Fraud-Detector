# ML Project: Financial Fraud Detection

## Overview

This project aims to analyse a financial dataset and develop an algorithm that can flag transactions as potentially fraudulent.

## The Problem

The program should be able to take a transaction and:
1) calculate probability of fraud
2) classify the risk level of the transaction
3) list down factors that contributed to its classification

### Class Imbalance

Real-world datasets for financial transactions are likely to be extremely imbalanced, i.e. the number of legitimate transactions vastly outweighs the number of fraudulent transactions.

This means that a model that predicts every transaction as legitimate would still achieve a high level of accuracy. However, such a model is obviously useless in detecting fraud.

A good technique to deal with this problem is *"Class Weighting"* where the programmer designs mistakes on fraud examples to be more costly. A baseline model can be implemented alongside a class-weighted model to observe the differences in performance.

### Precision & Recall

Precision = proportion of flagged transactions that were actually fraudulent

Recall = proportion of frauds that were successfully flagged

F1 score = 2 x (Precision x Recall) / (Precision + Recall)

Another consideration is balancing the frequency of false negatives and false positives. Naturally, one would aim to report frauds as thoroughly as possible, but flagging legitimate transactions might also degrade user experience and create more operational work.

### Threshold

Most classifiers are not either/or, and output something like "Probability of fraud" as a real number from 0 to 1.

Raising the threshold might reduce recall while lowering it might reduce precision.

### ROC-AUC

ROC stands for *Receiver Operating Characteristic*, and evaluates how well the model separates the two classes across many possible thresholds. It plots: True Positive Rate vs False Positive Rate

The AUC, *Area Under the Curve*, gives a summary number that can be roughly interpreted as:
- 0.5 → random guessing
- 0.7 → somewhat useful
- 0.8 → good
- 0.9 → very good
- 1.0 → perfect

However, ROC-AUC does not work as well in imbalanced datasets, and PR-AUC is used instead.

## Dataset

https://www.kaggle.com/datasets/ealaxi/paysim1

The dataset used is synthetically generated using the simulator called PaySim.

## Changelog

### 10/09/2026

Initialised the project and imported the dataset into a Jupyter Notebook.

<details>
    <summary>Output</summary>

    (6362620, 11)
    step      type    amount     nameOrig  oldbalanceOrg  newbalanceOrig  \
    0     1   PAYMENT   9839.64  C1231006815       170136.0       160296.36   
    1     1   PAYMENT   1864.28  C1666544295        21249.0        19384.72   
    2     1  TRANSFER    181.00  C1305486145          181.0            0.00   
    3     1  CASH_OUT    181.00   C840083671          181.0            0.00   
    4     1   PAYMENT  11668.14  C2048537720        41554.0        29885.86   

        nameDest  oldbalanceDest  newbalanceDest  isFraud  isFlaggedFraud  
    0  M1979787155             0.0             0.0        0               0  
    1  M2044282225             0.0             0.0        0               0  
    2   C553264065             0.0             0.0        1               0  
    3    C38997010         21182.0             0.0        1               0  
    4  M1230701703             0.0             0.0        0               0  
    Index(['step', 'type', 'amount', 'nameOrig', 'oldbalanceOrg', 'newbalanceOrig',
        'nameDest', 'oldbalanceDest', 'newbalanceDest', 'isFraud',
        'isFlaggedFraud'],
        dtype='str')
    step                int64
    type                  str
    amount            float64
    nameOrig              str
    oldbalanceOrg     float64
    newbalanceOrig    float64
    nameDest              str
    oldbalanceDest    float64
    newbalanceDest    float64
    isFraud             int64
    isFlaggedFraud      int64
    dtype: object
    step              0
    type              0
    amount            0
    nameOrig          0
    oldbalanceOrg     0
    newbalanceOrig    0
    nameDest          0
    oldbalanceDest    0
    newbalanceDest    0
    isFraud           0
    isFlaggedFraud    0
    dtype: int64
    isFraud
    0    6354407
    1       8213
    Name: count, dtype: int64
    isFraud
    0    0.998709
    1    0.001291
    Name: proportion, dtype: float64
</details>

### 17/09/2026

Performed exploratory data analysis on the dataset.
- All recorded fraud occurs in CASH_OUT and TRANSFER. While highly unlikely in a real-world dataset, type can be used as a strong predictor of fraud in this synthetic dataset.
- While both transaction types have a roughly equal number of fraud occurences, there are only ~530k TRANSFERs as compared to 2.2mil CASH_OUTs. hence, the fraud rate of TRANSFER is ~4.2 times that of CASH_OUT.
- Compiled the statistics for the amounts of money involved in both fraudulent and non-fraudulent transactions
- Compiled the statistics for the balance differences in original and destination accounts involved in both fraudulent and non-fraudulent transactions