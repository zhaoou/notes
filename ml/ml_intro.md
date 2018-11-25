* example - one instance, object of data
* feature - one field or value from example
* label - value that we will be predicting
* training  set - subset of examples used for training
* test set - subset of exampes used for testing (don't use training set to test!)
* feature engineering - selection and merging of features to minimize loss
* ReLU(Rectified Linear Unit) function: max(x | 0)
* Confusion matrix: matrix of True Positives True Negatives False Positives and False negatives
* Sensitivity(recall) = TP / (TP + FN)
* Specificity(precision) = TP / (TP + FP)

> gradient boosting is used for problems where structured data is available, whereas deep learning is used for perceptual prob- lems such as image classification.

# Supervised vs Unsupervised

## Supervised (classification and regression)

Supervised learning uses labeled data to generate models. Labels classify data into one or more categories or predict a continious value.
Some algorithms that belong to this group:

### Linear regression (predicts continious value)

1) find a line of least squares to minimize loss
2) use that line to predict values of y

Gradient descent - iterative method to minimize loss:

1) pick and random line formula(hyperparameters)
2) calculate loss in training data 
3) adjust line formula by learning rate(step size)
4) got to step 2

* Stochastic gradient descent (SGD) - calculate gradient descent using one randomly chosen example from training set
* Mini-batch SGD - choose a small batch of examples and use them to calculate descent


### Nearest neighbour classifier

1) Represent data in a multidimensional matrix
2) In a multidimensional space we calculate the distance to an odd nearest neighbours, and classify into that group.
3) use euclidian or manhattan distance to calculate the distance


### Support Vector Machine

* decision boundary (hyperplance) is computed to maximize the margin (distances between clusters).

### Neural Networks

Each layer stores weights
Backpropagation is used to optimize weights based on loss

## Unsupervised (clustering)

### k-means

1) Decide a number of clusters desired
2) Find a way to represent data in a multidimensional space








thisismetis curriculum:

Week 1: Introduction to the Data Science Toolkit
Exploratory Data Analysis, Bash, Git & GitHub, Python, pandas, matplotlib, Seaborn

Week 2: Linear Regression and Machine Learning Intro
Web scraping via BeautifulSoup and Selenium, regression with statsmodels and scikit-learn, feature selection overfitting and train/test splits, probability theory.

Week 3: Linear Regression and Machine Learning Continued
Regularization, hypothesis testing , intro to Bayes Theorem

Week 4: Databases and Introduction to Machine Learning Concepts
Classification and regression algorithms (Knn, logistic regression, SVM, decision trees, and random forest), SQL concepts, cloud servers

Week 5: More supervised learning algorithms & web tools
Naive Bayes, stochastic gradient descent and intro to Deep Learning, Full stack in a nutshell: Python Flask, Javascript and D3.js

Week 6: Statistical Fundamentals
MLE, GLM, Distributions, Databases ( RESTful APIs, NoSQL databases, MongoDB, pymongo) Natural Language Processing techniques

Week 7:  Unsupervised Machine Learning
Various clustering algorithms, including K-means and DBSCAN, dimension reduction techniques (PCA, SVD, LDA, NMF)

Week 8: More Deep Learning & Unsupervised Learning
Deep Learning via Keras, Recommender Systems
