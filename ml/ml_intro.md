:book:
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

In linear regression, both x and y are continious variables, but in logistic regression x is continious and y is binary: (Passed/Failed, True/False, etc)

* Odds vs probability: on a fair die, we have odds 1 to 5 of getting a 3, but probability of this is 1/6th.

```
         p
odds = ----
       1 - p
```

* logistic function is a sigmoid function with domain R and range 0-1
* logit is log of odds function

### Nearest neighbour classifier

https://matterhorn.dce.harvard.edu/engage/player/watch.html?id=c322c0d5-9cf9-4deb-b59f-d6741064ba8a

1) Represent data in a multidimensional matrix
2) In a multidimensional space we calculate the distance to an odd nearest neighbours, and classify into that group.
3) use euclidian or manhattan distance to calculate the distance


### Support Vector Machine

https://www.youtube.com/watch?v=N1vOgolbjSc

https://matterhorn.dce.harvard.edu/engage/player/watch.html?id=f21fcc8f-93a8-49f6-9ff8-0f339b0728bd


#### Linear SVM

* support vectors represent the model, and we don't need training data in memory
* classification algorithm
* decision boundary (hyperplane) is computed to maximize the margin (distances between clusters)
* finds the widest margin between the groups
* points closest to the hyperplane(line) are called support vectors
* margin is the distance between the dividing line and the nearest datapoint, we want to maximize it


#### Multidimensional SVM

* in a complex situation we can calculate another dimension of data to spread the datapoint into more dimensions and use a plane to separate the points:
```
|     0
| 0  *    0
|0    **
| 0     0
------------

Rescale and shift:

     |0
 0  *|   0
0----**-----
  0  | 0
     |
     

For this example, a good z value could be a sum of absolute values of x and y: 
higher z values will be assigned to 0s and lower to *.


From a side perspective, we will have a layer of * at the bottom and 0s on top:

| 0    0   0
|0 0     0 
 /---------------/
/____*__________/  plane separating the groups
|     **
|
|z
```

![visualized](http://beta.cambridgespark.com/courses/jpm/figures/mod5_kernel_trick.png)


[morebetter](http://beta.cambridgespark.com/courses/jpm/05-module.html)

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


