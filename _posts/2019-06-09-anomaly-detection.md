---
title:  Anomaly detection - review
categories: data-science
date: 2019-06-09
tags: 
  - data science
  - anomaly detection
  - time series
---

This is an introduction of anomaly detection and possible approaches for
time series. This article is heavily based on the paper ["Anomaly Detection: a Survey", by Chandola et. al.](http://doi.acm.org/10.1145/1541880.1541882).

![anomaly](http://www.simonstalenhag.se/bilderbig/l_fb_08_big.jpg)
*Simon Stalenhag http://www.simonstalenhag.se*

## Definitions

**Anomaly detection** is the activity of finding patterns that do not
conform to expected behavior. These data instances are usually called
anomalies or outliers. Anomaly detection is important because the
anomalies can be often related to significant or even critical and
actionable information.

**Anomaly** is a pattern in data that does not conform to a well defined
notion of normal behavior. They can have several different causes but
have in common that they are interesting to the analyst and is relevant
in real life. This differs from **noise removal**, as noise is unwanted,
it is not relevant to the analyst, and its removal facilitates the
analysis. **Novelty detection** is related to anomaly detection, as it
searches for new patterns in data. The distinction is that after the
novelty is detected, it is usually included in what is considered normal
behavior.

**Data** is a record of a phenomena. A data instance input is a
collection of data instances of the same phenomena. A data instance can
be described by a set of **attributes** (features, dimensions, fields).
Each attribute has a **type** (binary, categorical, continuous). The data
instance can be univariate or multivariate (one or several attributes).
The data instances can be related to each other in a **sequential**,
**graph** or **spatial** form.

Sequential forms are ordered, where they can be related to the preceding
one, like time series or genomic data. Spatial data instances are
related to its neighbors and graph forms are related by nodes and
vertices. Combinations of data forms can occur, like spatio-temporal
data instances.

The data can also be **labeled**. Labeled data instances are instances
that are marked as normal or abnormal. Fully labeled datasets are subject
to split into train and test datasets. Labeled datasets can have
problems with class imbalance.

## Types of anomalies

Anomalies can be point anomalies or contextual anomalies.

### Point anomalies

Point anomalies are anomalies that one data instance is considered
anomalous with respect of the remainder of the data.

### Contextual anomalies

Contextual anomalies considers the structure of the data as part of what
constitutes an anomaly. They are defined by two kinds of attributes:

- contextual attributes: the attributes that define the context
    (neighborhood, sequence) of the data points, like time in a time
    series
- behavioral attributes: attributes that define the non-contextual
    information of a data instance, being measured on the scale of the
    contextual ones.

It's the value of the data itself. The anomalies are found in the
behavioral attributes.

### Collective anomalies

Anomalies that are composed by a set of data instances, that form an
anomaly when they are together but it can be normal otherwise.

## Challenges

The straightforward way of detecting anomalies is to define a *normal*
region in the data and flag any data instances that are outside this
normal region as anomalies. This approach has several problems:

- The definition of a normal region for all normal behaviors
    is difficult. The boundary between normal and abnormal often is not
    very precise.
- Malicious players can try to mask its activities emulating the
    normal behavior
- What is normal can change and new patterns would have to be added to
    the normal one
- The exact definition on what constitutes an anomaly changes
    between domains. What is considered an anomaly in body temperature

variation, which small variations are considered anomalies, would not be
considered an anomaly in finance data, where larger variations are
expected.

- Labeled data is hard to come by
- Noise has a tendency to be similar to anomalies.

## Anomaly detection categories and methods

The anomaly detection methods can be classified into supervised,
semi-supervised or unsupervised methods.

### Supervised methods

Methods that learn the patterns from the labeled data are supervised
methods. These methods are usually classification methods and need the
dataset to be fully labeled.

### Semi supervised methods

Semi supervised methods can be applied to datasets that are partially
labeled, like only one class (the normal one in this case) is labeled.

### Unsupervised methods

Unsupervised methods are employed when the data is not labeled.
Generally these methods assume that the proportion of normal and abnormal
data instances are heavily skewed towards normal data instances.

Anomaly detection techniques categories

- Classification
- Clustering
- Near neighbor
- Statistical
- Information theoretic
- Spectral

Machine learning is usually associated with all the categories above
except Information theoretic and Spectral.

### Classification Anomaly Detection Techniques

The main assumption for classification based anomaly detection is that
the method can learn to distinguish between normal and anomalous classes
given the feature space.

Classification methods can be **multi-class** or **one-class**. One
class classifiers assume that the data instances belong to one class
(the normal one) and the instances that are not part of this class are
anomalous. Multi-class classifiers identify the different classes of
normal and abnormal behavior.

**Neural networks** are used to classify anomalies. The most common
networks used for it are Replicator networks and Autoencoders.

The Naive Bayes classifier considers that each attribute is independent
of the others to define if an instance belongs to a class. The
classifier then works using a decision rule. For example, one common
decision rule is to choose the class based on the highest posterior
given the parameters and the data instance.

**Bayesian networks** are also used as classification method for anomaly
detection. The classifier labels the test instances as normal or abnormal
given the largest posterior probability distribution. Zero probabilities
are smoothed out using Laplace Smoothing. (Laplace smoothing is used to
treat these classes that are possible to occur but may not appear in the
sample. If a check would be made on this dataset, classes would have
zero probability instead of a larger than zero, so this smoothing takes
care of that.).

**Support Vector Machines** are also used as standard classifiers for
anomalies. The Radial Based Kernel (RBF) is generally a good kernel for
anomaly detection.

**Rule based** classifiers are more basic than the other methods, but
when the data set is simple enough or there are restrictions for the
need of rules or decision trees, they can be applied.

Advantages:

- flexibility with several classes
- fast testing after model is trained

Disadvantages:

- need labeled data, worse for multi-class problems
- binary classifiers can be very limited

- one input neuron for one KPI
- each time step corresponds to one input value
- network is trained with data without outliers
- score of test data gives the anomaly score
- same approach can be used for deviations from statistic instead of
    raw values
- the score could be recalibrated based on new requirements and
    instances

### Nearest Neighbor ADT

The assumption for nearest neighbor methods is that normal instances
occur in dense neighborhoods while anomalies occur far from the closest
neighbors.

The definition of a distance or similarity metric is key in this
approach. The distance or similarity measure depends on the data
attributes. The measures are required to be positive-definite and
symmetric but don't need to hold the triangle inequality, although in
this case the distance is not a metric.

Two approaches: one is calculating the distance of a data instance to
its kth instances as an anomaly score; relative density between each
data instance as an anomaly score.

kNN uses the distance as a score for anomaly. Either a threshold is used
or the top n instances by distance are defined as anomalies. Another
approach is the sum of all k distances to the data instance as an
anomaly score (called Peer Group Analysis).

Approaches based on density apply either a fixed radius around a data
instance and uses 1/k instances as an anomaly score, or fixes a number d
of near instances and uses 1/d as anomaly score. These techniques may
not have good performance if the there are different densities for
normal data. One approach to handle this kind of situation is the Local
Outlier Factor, that calculates the ratio between the average local
density of the data instance and its k nearest neighbors. The local
density is defined by finding the radius that encompass the k nearest
neighbors and dividing by k. For normal instances, these values should
be similar to its neighbors while for anomalies it should be different.
There are other variations of these methods, like ODiN, MDEF, etc.

To improve performance, pruning can be used.

Advantages:

- Unsupervised
- Semi-supervised can be used to minimize missing anomalies
- Only the distance metric is needed

Disadvantages:

- Varying densities may prevent the method from working
- Computational cost of checking test instances is high
- The distance metric is critic for the method, so if cannot be well
    defined or the computational

cost is too high, this approach mail fail

- Sequences are complicated to be modeled

### Clustering based ADT

Three assumptions can be made for clustering methods:

1. Normal data belong to clusters, anomalous data does not (DBSCAN)
2. Normal data are closer to the cluster centroid, while anomalies are
    far away (Kmeans, Expectation

Maximization)

1. Normal data belong to dense and large clusters while anomalies
    belong to small and sparse clusters

(CBLOF)

The main difference between nearest neighbors and clustering methods is
that NN, even if the two techniques need a distance metric, clustering
methods evaluate distances based on cluster properties (centroid, etc)
while NN uses a instance per instance approach.

Advantages:

- Unsupervised
- Complex data types are easy to handle
- Checking test instances is fast: measures are calculated against
    well know and well defined clusters

Disadvantages:

- Highly dependent on the structure of normal instances
- Anomaly detection is a byproduct of the method
- Computationally intensive

The problem with this is that events that are in the same time are
correlated and that deviations occur in both KPIs.

### Statistical based ADT

The underlying principle of statistical approaches is "An anomaly is an
observation which is suspected of being partially or wholly irrelevant
because it is not generated by the stochastic model assumed". In other
words, the process that creates the normal data is different from the
process that generates the anomalies. The basic assumption is that
normal data occur in high probability regions of the stochastic model
while anomalous data occur in regions with low probability.

Two approaches can be used for stochastic ADT: **parametric** and
**non-parametric**. Parametric models assume a distribution for the
normal data (Gaussian, etc) and estimates the parameters of said
distribution based on the data, using, for example, maximum likelihood
estimation. Methods based on the number of standard deviations of the
average or median belong to this class, similarly to the box plot
methods. Given a data distribution, either assumed or not, several
statistical tests can be employed to test the data and attribute a
score. One example is the Grubb test.

Regression based tests assume that a model can be fitted on the normal
data and values that are different from the predicted by the model
residuals are anomalous, with a score. Robust methods, auto-regressive
models (ARMA, ARIMA) and even linear models can be used.

Mixture models assume that the probability density distributions of
normal and abnormal data are different and tries to capture it. Box
models assume that normal data is given by a Gaussian distribution with
*μ* and *σ*, and an anomaly distribution, also Gaussian, with same *μ*
but different *σ*. After fitting both distributions, a statistical test
is applied to verify if the data instance belongs to which distribution,
classifying into normal or abnormal.

**Non-parametric** techniques don't assume a statistical model
structure. Instead, they determine it from the data itself. Histogram
(counting) methods belong to this class. There are several caveats to
these methods, like the binning size for histograms.

Advantages:

- If the model has a good fit with the underlying data distribution,
    the decision of if it's an

anomaly or not has a statistical base

- The anomaly score can be associated with a confidence interval
- If the distribution estimation is robust, these methods can be used
    unsupervised

Disadvantages:

- If the model representation is poor or there is no underlying
    probability density distribution,

these models cannot be used

- Choosing the best test statistic is very hard
- It is hard to take into account attribute relationships in counting
    methods

### Information theoretic ADT

The basic assumption about information theoretic approaches for anomaly
detection is that anomalies in the data include irregularities in the
information content of the dataset.

Metrics of information content are e.g. Kolmogorov Complexity, entropy,
relative entropy, etc. These techniques are based on evaluating the
complexity of the data set, C(D), and searching for a minimal subset of
D where C(D) - C(D-I) is maximum. The key is to find a subset that is
most different from the dataset, based on a information content metric.

There are several possible optimus to be found with this approach. It
is also needed to solve a double optimization problem (finding the
subset and maximizing the difference of this subset and the data set), so
the complexity of the computational code can be exponential. Local
Search Algorithm can help with the performance side.

Considering sequence data, the data set is broken into subsets and
compared with other subsets to find substructures.

Advantages:

- Unsupervised
- No assumptions about underlying data structure

Disadvantages:

- The information content measurement is critical
- The size of the substructures in sequence data sets is fundamental
    and hard to find
- Hard to associate an anomaly score with test score

### Spectral ADT

This approach assumes that a combination of the attributes of the data
can capture the bulk of the variability of it. The basic idea is that
the data can be embedded into a lower dimensional subspace in which
normal and abnormal instances appear differently.

Techniques that belong to this class are, for example, PCA and Robust
PCA. In this case, normal data would be closer to the projection line
while anomalies would be off.

Advantages:

- Can be used in high dimensional data sets, even as a preprocessing
    step
- Unsupervised

Disadvantages

- Only works if there is separation in lower dimensional space
- High computational complexity

## Contextual anomalies ADT

There are two ways to handle contextual anomalies:

- reduction to point anomalies
- structure modeling

Reduction to point anomalies consist as mapping a point technique to a
selected context. One way to do it is to map a variable like "user" and
will be used as a context attribute, with the remaining attributes as
behavioral attributes. This can also be done in groups. The use of
statistics to extract information, like average, median and rolling
statistics and comparison with deviations can also be done in this
approach.

For structure modeling, this is similar to what was presented about
Statistical modeling. A structural model of the data is created and new
instances are compared with this model. If they differ from the model
prediction, they are considered anomalies.
