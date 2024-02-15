# KMeans Package

This package includes functionalities to identify unknown clusters of multidimensional data using the well known (at least in the machine-learning field) kmeans algorithm.

The kmeans algorithm divides a set of `N` samples `X` into `k` disjoint clusters `C`, each described by the mean of the samples in the cluster. The means are called the cluster centroids.

The objective is to minimize some loss. For instance, the objective is to minimize "inertia", or within-cluster sum-of-squares criterion in case of the Euclidean distance function.

For more information see:

https://scikit-learn.org/stable/modules/clustering.html#kmeans

Please ask questions and report bugs on the Gretl mailing list if possible. Alternatively, create an issue ticket on the github repo (see below).
Source code and test script(s) can be found here: https://github.com/atecon/kmeans

# Public Functions

## kmeans_fit

```
kmeans_fit (const list xlist, const int n_clusters[2::2], bundle opts[null])
```

Execute the kmeans algorithm and estimate the clusters.

**Arguments:**

- `xlist`: list, Features (regressors) to train the model.
- `n_clusters`: int, Number of assumed clusters (default: 2)
- `opts`: bundle, Optional parameters affecting the kmeans algorithm. You can pass the following parameters:

    * `algorithm`: string, kmeans algorithm to use. Currently, only `full` is supported (classical EM-style algorithm).
    * `distance_type`: string, Name of the distance metric applied (default: `euclidean`). For more distance metrics, see gretl's built-in function `distance()`.
    * `initializer`: string, Method for initialization. Either `random`: Choose `n_clusters` observations (rows) at random from data for the initial centroids. Or `pca`: Try to pick data points that are as far apart as possible by means of PCA.
    * `max_iter`: int, Maximum number of iterations of the kmeans algorithm to run.
    * `n_draws`: int, Number of time the kmeans algorithm will be run with different centroid seeds. The final results will be the best output of `n_draws` consecutive runs in terms of inertia.
    * `tolerance`:  scalar, Minimum improvement of the `within_variation_total` (Sum of the squared distances across all clusters) required before early stopping the algorithm (default: 1e-4)
    * `verbose`: int, Level of verbosity: `0`: don't print anything, `1`: print some details, `2`: print more details (default: `0`)


**Return:** Bundle holding various items.

- `between_variation`: scalar, Between cluster sum of squares = `total_ssq - within_variation_total`
- `centroids`: matrix, Estimated mean values (centroids) for each feature (columns) and for each cluster (rows).
- `cluster_id`: matrix, Estimated cluster ID for each observation for the best draw minimizing `inertia`.
- `distances`: matrix, Estimated distance for the best draw minimizing `inertia`.
- `error`: int, Error code. In case of no error `FALSE`, otherwise positive integer.
- `nobs`: int, Number of non-missing observations used for training.
- `pointsize`: scalar, Size of points being plotted when calling the `kmeans_plot()` function.
- `total_ssq`: scalar, Sum of the squared distances of the features from its mean values
- `use_circles`: bool, Plot circles instead of point when calling the `kmeans_plot()` function.
- `within_variation_total`: scalar, Sum of the squared distances across all clusters.
- `within_variation_avg`: scalar, Sum of the average squared distances across all clusters.


## kmeans_predict

```
kmeans_predict (const list xlist, const bundle Model)
```

Predict cluster belonging based on the estimated model.

**Arguments:**

- `xlist`: list, Features (regressors) used for predicting cluster belonging.
- `Model`: bundle, Model object returned by the `kmeans_fit()` function.

**Return:** Series holding the predicted cluster ID for each observation.


## kmeans_summary

```
kmeans_summary (const bundle Model)
```

Print summarizing information on estimation step after having applied the `kmeans_fit()` function.

**Arguments:**

- Model: bundle, Bundle returned by the `kmeans_fit()` function.

**Return:** Nothing.


## kmeans_plot

```
kmeans_plot (const list xlist, const bundle self[null])
```

Factorized scatter plot estimated clusters for each 2-dimensional combination of features. This function calls the user-defined package "PairPlot" which must be installed.

**Arguments:**

- `xlist`: list, Features (regressors) used for plotting.
- `self`: bundle, Bundle for manipulating the plot. **Note** Here you can also pass options accepted by the "PairPlot" package which is used in the background.

**Return:** Nothing.


# Changelog

* **v0.3 (February 2024)**
    * Move to markdown-based help file
    * Internal improvements

* **v0.2 (July 2022)**
    * Fix bug that arises if the sample range is restricted, and you're trying to coerce a column vector that's not the full length of the dataset into a series on adding it to a bundle.
    * Returned objects `cluster_id` and `distances` when calling the `kmeans_fit()` function are of type matrix instead of series, now.

* **v0.1 (February 2022)**
    * initial release
