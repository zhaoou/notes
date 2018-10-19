* example - one instance, object of data
* feature - one field or value from example
* label - value that we will be predicting


# Supervised vs Unsupervised

## Supervised (classification and regression)

Supervised learning uses labeled data to generate models. Labels classify data into one or more categories or predict a continious value.
Some algorithms that belong to this group:

* Linear regression (predicts continious value)

1) find a line of least squares to minimize loss
2) use that line to predict values of y



* Nearest neighbour classifier

1) Represent data in a multidimensional matrix
2) In a multidimensional space we calculate the distance to an odd nearest neighbours, and classify into that group.
3) use euclidian or manhattan distance to calculate the distance



## Unsupervised (clustering)

* k-means

1) Decide a number of clusters desired
2) Find a way to represent data in a multidimensional space
