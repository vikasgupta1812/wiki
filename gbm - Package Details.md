
# gbm

## Generalized Boosted Regression Modeling

### Description

Fits generalized boosted regression models.

### Usage

This package implements extensions to Freund and Schapire’s AdaBoost algorithm and J. Friedman’s gradient boosting machine. Includes regression methods for least squares, absolute loss, logistic, Poisson, Cox proportional hazards partial likelihood, multinomial, t-distribution, AdaBoost exponential loss, Learning to Rank, and Huberized hinge  loss.


`gbm` Generalized Boosted Regression Modeling

**Description** Fits generalized boosted regression models

**Usage**

```r
gbm(formula = formula(data),
			distribution = "bernoulli",
			data = list(),
			weights,
			var.monotone = NULL,
			n.trees = 100,
			interaction.depth = 1,
			n.minobsinnode = 10,
			shrinkage = 0.001,
			bag.fraction = 0.5,
			train.fraction = 1.0,
			cv.folds=0,
			keep.data = TRUE,
			verbose = "CV",
			class.stratify.cv=NULL,
			n.cores = NULL)
```

```r
gbm.fit(x, y,
        offset = NULL,
        misc = NULL,
        distribution = "bernoulli",
        w = NULL,
        var.monotone = NULL,
        n.trees = 100,
        interaction.depth = 1,
        n.minobsinnode = 10,
        shrinkage = 0.001,
        bag.fraction = 0.5,
        nTrain = NULL,
        train.fraction = NULL,
        keep.data = TRUE,
        verbose = TRUE,
        var.names = NULL,
        response.name = "y",
        group = NULL)
```

```r
gbm.more(object,
         n.new.trees = 100,
         data = NULL,
         weights = NULL,
         offset = NULL,
         verbose = NULL)
```

### Arguments

**formula**
A symbolic description of the model to be fit. The formula may include an offset term (e.g. y~offset(n)+x). If `keep.data=FALSE` in the initial call to `gbm` then it is the user's responsibility to resupply the offset to `[gbm.more][2035]`.

**distribution**
Either a character string specifying the name of the distribution to use or a list with a component `name` specifying the distribution and any additional parameters needed. If not specified, `gbm` will try to guess: if the response has only 2 unique values, bernoulli is assumed; otherwise, if the response is a factor, multinomial is assumed; otherwise, if the response has class "Surv", coxph is assumed; otherwise, gaussian is assumed.

Currently available options are "gaussian" (squared error), "laplace" (absolute loss), "tdist" (t-distribution loss), "bernoulli" (logistic regression for 0-1 outcomes), "huberized" (huberized hinge loss for 0-1 outcomes), "multinomial" (classification when there are more than 2 classes), "adaboost" (the AdaBoost exponential loss for 0-1 outcomes), "poisson" (count outcomes), "coxph" (right censored observations), "quantile", or "pairwise" (ranking measure using the LambdaMart algorithm).

If quantile regression is specified, `distribution` must be a list of the form `list(name="quantile",alpha=0.25)` where `alpha` is the quantile to estimate. The current version's quantile regression method does not handle non-constant weights and will stop.

If "tdist" is specified, the default degrees of freedom is 4 and this can be controlled by specifying `distribution=list(name="tdist", df=DF)` where `DF` is your chosen degrees of freedom.

If "pairwise" regression is specified, `distribution` must be a list of the form `list(name="pairwise",group=...,metric=...,max.rank=...)` (`metric` and `max.rank` are optional, see below). `group` is a character vector with the column names of `data` that jointly indicate the group an instance belongs to (typically a query in Information Retrieval applications). For training, only pairs of instances from the same group and with different target labels can be considered. `metric` is the IR measure to use, one of

**`conc`**:
Fraction of **concordant** pairs; for binary labels, this is equivalent to the Area under the ROC Curve

**`mrr`**:
**Mean reciprocal rank** of the highest-ranked positive instance

**`map`**:
**Mean average precision**, a generalization of `mrr` to multiple positive instances

**`ndcg:`**
**Normalized discounted cumulative gain**. The score is the weighted sum (DCG) of the user-supplied target values, weighted by log(rank+1), and normalized to the maximum achievable value. This is the default if the user did not specify a metric.

`ndcg` and `conc` allow arbitrary target values, while binary targets {0,1} are expected for `map` and `mrr`. For `ndcg` and `mrr`, a cut-off can be chosen using a positive integer parameter `max.rank`. If left unspecified, all ranks are taken into account.

Note that splitting of instances into training and validation sets follows group boundaries and therefore only approximates the specified `train.fraction` ratio (the same applies to cross-validation folds). Internally, queries are randomly shuffled before training, to avoid bias.

Weights can be used in conjunction with pairwise metrics, however it is assumed that they are constant for instances from the same group.

For details and background on the algorithm, see e.g. Burges (2010).

**data**
An optional data frame containing the variables in the model. By default the variables are taken from `environment(formula)`, typically the environment from which `gbm` is called. If `keep.data=TRUE` in the initial call to `gbm` then `gbm` stores a copy with the object. If `keep.data=FALSE` then subsequent calls to `[gbm.more][2035]` must resupply the same dataset. It becomes the user's responsibility to resupply the same data at this point.

**weights**
An optional vector of weights to be used in the fitting process. Must be positive but do not need to be normalized. If `keep.data=FALSE` in the initial call to `gbm` then it is the user's responsibility to resupply the weights to `[gbm.more][2035]`.

**var.monotone**
An optional vector, the same length as the number of predictors, indicating which variables have a monotone increasing (+1), decreasing (-1), or arbitrary (0) relationship with the outcome.

**n.trees**
The total number of trees to fit. This is equivalent to the number of iterations and the number of basis functions in the additive expansion.

**cv.folds**
Number of cross-validation folds to perform. If `cv.folds`> 1 then `gbm`, in addition to the usual fit, will perform a cross-validation, calculate an estimate of generalization error returned in `cv.error`.

**interaction.depth**
The maximum depth of variable interactions. 1 implies an additive model, 2 implies a model with up to 2-way interactions, etc.

**n.minobsinnode**
minimum number of observations in the trees terminal nodes. Note that this is the actual number of observations not the total weight.

**shrinkage**
A shrinkage parameter applied to each tree in the expansion. Also known as the learning rate or step-size reduction.

**bag.fraction**
The fraction of the training set observations randomly selected to propose the next tree in the expansion. This introduces randomnesses into the model fit. If `bag.fraction`<1 then running the same model twice will result in similar but different fits. `gbm` uses the R random number generator so `set.seed` can ensure that the model can be reconstructed. Preferably, the user can save the returned `[gbm.object][2036]` using `[save][2037]`.

**train.fraction**
The first `train.fraction * nrows(data)` observations are used to fit the `gbm` and the remainder are used for computing out-of-sample estimates of the loss function.

**nTrain**
An integer representing the number of cases on which to train. This is the preferred way of specification for `gbm.fit`; The option `train.fraction` in `gbm.fit` is deprecated and only maintained for backward compatibility. These two parameters are mutually exclusive. If both are unspecified, all data is used for training.

**keep.data**
A logical variable indicating whether to keep the data and an index of the data stored with the object. Keeping the data and index makes subsequent calls to gbm `[more][2035]` faster at the cost of storing an extra copy of the dataset.

**object**
A `gbm` object created from an initial call to `[gbm][2035]`.

**n.new.trees**
The number of additional trees to add to `object`.

**verbose**
If TRUE, gbm will print out progress and performance indicators. If this option is left unspecified for gbm.more then it uses `verbose` from `object`.

**class.stratify.cv**
Whether or not the cross-validation should be stratified by class. Defaults to `TRUE` for `distribution="multinomial"` and is only implementated for `multinomial` and `bernoulli`. The purpose of stratifying the cross-validation is to help avoiding situations in which training sets do not contain all classes.

**x, y**
For `gbm.fit`: `x` is a data frame or data matrix containing the predictor variables and `y` is the vector of outcomes. The number of rows in `x` must be the same as the length of `y`.

**offset**
a vector of values for the offset

**misc**
For `gbm.fit`: `misc` is an R object that is simply passed on to the gbm engine. It can be used for additional data for the specific distribution. Currently it is only used for passing the censoring indicator for the Cox proportional hazards model.

**w**
For `gbm.fit`: `w` is a vector of weights of the same length as the `y`.

**var.names**
For `gbm.fit`: A vector of strings of length equal to the number of columns of `x` containing the names of the predictor variables.

**response.name**
For `gbm.fit`: A character string label for the response variable.

**group**
`group` used when `distribution = 'pairwise'.`

**n.cores**
The number of CPU cores to use. The cross-validation loop will attempt to send different CV folds off to different cores. If `n.cores` is not specified by the user, it is guessed using the `detectCores` function in the `parallel` package. Note that the documentation for `detectCores` makes clear that it is not failsave and could return a spurious number of available cores.

### Value

`gbm`, `gbm.fit`, and `gbm.more` return a `[gbm.object][2036]`.

### See also

`[gbm.object][2036]`, `[gbm.perf][2038]`, `[plot.gbm][2039]`, `[predict.gbm][2040]`, `[summary.gbm][2041]`, `[pretty.gbm.tree][2042]`.

### Details

See the [gbm vignette][2043] for technical details.

This package implements the generalized boosted modeling framework. Boosting is the process of iteratively adding basis functions in a greedy fashion so that each additional basis function further reduces the selected loss function. This implementation closely follows Friedman's Gradient Boosting Machine (Friedman, 2001).

In addition to many of the features documented in the Gradient Boosting Machine, `gbm` offers additional features including the out-of-bag estimator for the optimal number of iterations, the ability to store and manipulate the resulting `gbm` object, and a variety of other loss functions that had not previously had associated boosting algorithms, including the Cox partial likelihood for censored data, the poisson likelihood for count outcomes, and a gradient boosting implementation to minimize the AdaBoost exponential loss function.

`gbm.fit` provides the link between R and the C++ gbm engine. `gbm` is a front-end to `gbm.fit` that uses the familiar R modeling formulas. However, `[model.frame][2044]` is very slow if there are many predictor variables. For power-users with many variables use `gbm.fit`. For general practice `gbm` is preferable.

### Examples

```r
# A least squares regression example # create some data

N  <- 1000
X1 <- runif(N)
X2 <- 2*runif(N)
X3 <- ordered(sample(letters[1:4],N,replace=TRUE),levels=letters[4:1])
X4 <- factor(sample(letters[1:6],N,replace=TRUE))
X5 <- factor(sample(letters[1:3],N,replace=TRUE))
X6 <- 3*runif(N)
mu <- c(-1,0,1,2)[as.numeric(X3)]

SNR <- 10 # signal-to-noise ratio
Y <- X1**1.5 + 2 * (X2**.5) + mu
sigma <- sqrt(var(Y)/SNR)
Y <- Y + rnorm(N,0,sigma)

# introduce some missing values
X1[sample(1:N,size=500)] <- NA
X4[sample(1:N,size=300)] <- NA

data <- data.frame(Y=Y,X1=X1,X2=X2,X3=X3,X4=X4,X5=X5,X6=X6)

# fit initial model
gbm1 <-gbm(Y~X1+X2+X3+X4+X5+X6,        	 # formula
			data=data,                     # dataset
			var.monotone=c(0,0,0,0,0,0),   # -1: monotone decrease,
												# +1: monotone increase,
												#  0: no monotone restrictions
			distribution="gaussian",     # see the help for other choices
			n.trees=1000,                # number of trees
			shrinkage=0.05,              # shrinkage or learning rate,
											# 0.001 to 0.1 usually work
			interaction.depth=3,         # 1: additive model, 2: two-way interactions, etc.
			bag.fraction = 0.5,          # subsampling fraction, 0.5 is probably best
			train.fraction = 0.5,        # fraction of data for training,
									# first train.fraction*N used for training
			n.minobsinnode = 10,         # minimum total weight needed in each node
			cv.folds = 3,                # do 3-fold cross-validation
			keep.data=TRUE,              # keep a copy of the dataset with the object
			verbose=FALSE,               # don't print out progress
			n.cores=1)                   # use only a single core (detecting #cores is
												# error-prone, so avoided here) 


# check performance using an out-of-bag estimator
# OOB underestimates the optimal number of iterations
best.iter <- gbm.perf(gbm1,method="OOB")
print(best.iter)

# check performance using a 50% heldout test set
best.iter <- gbm.perf(gbm1,method="test")
print(best.iter)

# check performance using 5-fold cross-validation
best.iter <- gbm.perf(gbm1,method="cv")
print(best.iter)

# plot the performance # plot variable influence
summary(gbm1,n.trees=1)         # based on the first tree
summary(gbm1,n.trees=best.iter) # based on the estimated best number of trees

# compactly print the first and last trees for curiosity
print(pretty.gbm.tree(gbm1,1))
print(pretty.gbm.tree(gbm1,gbm1$n.trees))

# make some new data
N <- 1000
X1 <- runif(N)
X2 <- 2*runif(N)
X3 <- ordered(sample(letters[1:4],N,replace=TRUE))
X4 <- factor(sample(letters[1:6],N,replace=TRUE))
X5 <- factor(sample(letters[1:3],N,replace=TRUE))
X6 <- 3*runif(N)
mu <- c(-1,0,1,2)[as.numeric(X3)]

Y <- X1**1.5 + 2 * (X2**.5) + mu + rnorm(N,0,sigma)

data2 <- data.frame(Y=Y,X1=X1,X2=X2,X3=X3,X4=X4,X5=X5,X6=X6)

# predict on the new data using "best" number of trees
# f.predict generally will be on the canonical scale (logit,log,etc.)
f.predict <- predict(gbm1,data2,best.iter)

# least squares error
print(sum((data2$Y-f.predict)^2))

# create marginal plots
# plot variable X1,X2,X3 after "best" iterations
par(mfrow=c(1,3))
plot(gbm1,1,best.iter)
plot(gbm1,2,best.iter)
plot(gbm1,3,best.iter)
par(mfrow=c(1,1))
# contour plot of variables 1 and 2 after "best" iterations
plot(gbm1,1:2,best.iter)
# lattice plot of variables 2 and 3
plot(gbm1,2:3,best.iter)
# lattice plot of variables 3 and 4
plot(gbm1,3:4,best.iter)

# 3-way plots
plot(gbm1,c(1,2,6),best.iter,cont=20)
plot(gbm1,1:3,best.iter)
plot(gbm1,2:4,best.iter)
plot(gbm1,3:5,best.iter)

# do another 100 iterations
gbm2 <- gbm.more(gbm1,100,
verbose=FALSE) # stop printing detailed progress
```
### Author(s)

Greg Ridgeway [gregridgeway@gmail.com][2045]

Quantile regression code developed by Brian Kriegler [bk@stat.ucla.edu][2046]

t-distribution, and multinomial code developed by Harry Southworth and Daniel Edwards

Pairwise code developed by Stefan Schroedl [schroedl@a9.com][2047]

### References

Y. Freund and R.E. Schapire (1997) "A decision-theoretic generalization of on-line learning and an application to boosting," _Journal of Computer and System Sciences,_ 55(1):119-139.

G. Ridgeway (1999). "The state of boosting," _Computing Science and Statistics_ 31:172-181.

J.H. Friedman, T. Hastie, R. Tibshirani (2000). "Additive Logistic Regression: a Statistical View of Boosting," _Annals of Statistics_ 28(2):337-374.

J.H. Friedman (2001). "Greedy Function Approximation: A Gradient Boosting Machine," _Annals of Statistics_ 29(5):1189-1232.

J.H. Friedman (2002). "Stochastic Gradient Boosting," _Computational Statistics and Data Analysis_ 38(4):367-378.

B. Kriegler (2007). [Cost-Sensitive Stochastic Gradient Boosting Within a Quantitative Regression Framework][2048]. PhD dissertation, UCLA Statistics.

C. Burges (2010). "From RankNet to LambdaRank to LambdaMART: An Overview," Microsoft Research Technical Report MSR-TR-2010-82.

[Greg Ridgeway's site][2049].

The [MART][2050] website.

_ Documentation reproduced from package gbm, version 2.1, License: GPL (>= 2) | file LICENSE _

[2035]: http://www.rdocumentation.org/gbm.html
[2036]: gbm.object.html
[2037]: ../../base/html/save.html
[2038]: gbm.perf.html
[2039]: plot.gbm.html
[2040]: predict.gbm.html
[2041]: summary.gbm.html
[2042]: pretty.gbm.tree.html
[2043]: ../doc/gbm.pdf
[2044]: ../../stats/html/model.frame.html
[2045]: mailto:gregridgeway@gmail.com
[2046]: mailto:bk@stat.ucla.edu
[2047]: mailto:schroedl@a9.com
[2048]: http://statistics.ucla.edu/theses/uclastat-dissertation-2007:2
[2049]: http://sites.google.com/site/gregridgeway
[2050]: http://www-stat.stanford.edu/~jhf/R-MART.html
[2051]: http://www.rdocumentation.org/assets/datacamp_upsell-14ef411929918c0895665e7598f2d497.png
[2052]: https://www.datacamp.com/training-paths/r-programmer-and-data-analyst?utm_source=rdocumentation&utm_medium=banner&utm_content=hello_bar&utm_campaign=subscription_rdocumentation?utm_source=rdocumentation&utm_medium=banner&utm_content=hello_bar&utm_campaign=subscription_rdocumentation
[2053]: http://www.rdocumentation.org/assets/cran_logo-7d15ced1779cb5e71663be445ebecc53.png
[2054]: /packages
[2055]: http://www.rdocumentation.org/assets/bioconductor_logo-cb887be554e6c8b298bcfdf7fe664f7a.png
[2056]: /packages?type=bioconductor
[2057]: http://www.rdocumentation.org/assets/github_name-5730966dea7ae588c0c740e53a75ee53.png
[2058]: /packages?type=github




