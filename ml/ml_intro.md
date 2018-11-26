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
* gradient boosting is used for problems where structured data is available
* deep learning is used for perceptual problems such as image classification

# Supervised vs Unsupervised

## Supervised (classification, regression) & ML theory

Supervised learning uses labeled data to generate models. Labels classify data into one or more categories or predict a continious value.
Some algorithms that belong to this group:

### Linear regression (predicts continious value)

https://matterhorn.dce.harvard.edu/engage/player/watch.html?id=afe70053-b8b7-43d3-9c2f-f482f479baf7

1) find a line of least squares to minimize loss
2) use that line to predict values of y

Gradient descent - iterative method to minimize loss:

1) pick and random line formula(hyperparameters)
2) calculate loss in training data 
3) adjust line formula by learning rate(step size)
4) got to step 2

* Stochastic gradient descent (SGD) - calculate gradient descent using one randomly chosen example from training set
* Mini-batch SGD - choose a small batch of examples and use them to calculate descent

If using objects(feature vectors) in linear regression, we assign coeffiecients to features to calculate good x values.

### Logistic regression

https://matterhorn.dce.harvard.edu/engage/player/watch.html?id=664f668e-e008-4f44-8600-e09ee6d629b0

### Nearest neighbour classifier

https://matterhorn.dce.harvard.edu/engage/player/watch.html?id=c322c0d5-9cf9-4deb-b59f-d6741064ba8a

1) Represent data in a multidimensional matrix
2) In a multidimensional space we calculate the distance to an odd nearest neighbours, and classify into that group.
3) use euclidian or manhattan distance to calculate the distance


### Support Vector Machine

https://matterhorn.dce.harvard.edu/engage/player/watch.html?id=f21fcc8f-93a8-49f6-9ff8-0f339b0728bd

* decision boundary (hyperplance) is computed to maximize the margin (distances between clusters)

### Decision trees & Random forest

https://matterhorn.dce.harvard.edu/engage/player/watch.html?id=8892a8b7-25eb-4bc5-80b6-47b9cf681a05


## Unsupervised (clustering)

### k-means

1) Decide a number of clusters desired
2) Find a way to represent data in a multidimensional space

### DBSCAN

### dimension reduction techniques (PCA, SVD, LDA, NMF)


## Natural Language Processing techniques


## Neural Networks(supervised)

Neurons review https://www.youtube.com/watch?v=uXt8qF2Zzfo&index=13&list=PLUl4u3cNGP63gFHB6xb-kVBiQHYe_4hSi&t=1508s

Each layer stores weights

Backpropagation is used to optimize weights based on loss

#### Deep Learning with Keras/TF

https://developers.google.com/machine-learning/crash-course/prereqs-and-prework


