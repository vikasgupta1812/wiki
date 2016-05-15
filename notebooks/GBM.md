

```R
require(gbm)
```

    Loading required package: gbm
    Loading required package: survival
    Loading required package: lattice
    Loading required package: splines
    Loading required package: parallel
    Loaded gbm 2.1.1



```R
?gbm
```






<h2>Generalized Boosted Regression Modeling</h2>

<h3>Description</h3>

<p>Fits generalized boosted regression models.</p>


<h3>Usage</h3>

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

gbm.more(object,
         n.new.trees = 100,
         data = NULL,
         weights = NULL,
         offset = NULL,
         verbose = NULL)
```


<h3>Arguments</h3>

<table summary="R argblock">
<tr valign="top"><td><code>formula</code></td>
<td>
<p>a symbolic description of the model to be fit. The formula may include an offset term (e.g. y~offset(n)+x). If <code>keep.data=FALSE</code> in the initial call to <code>gbm</code> then it is the user's responsibility to resupply the offset to <code>gbm.more</code>.</p>
</td></tr>
<tr valign="top"><td><code>distribution</code></td>
<td>
<p>either a character string specifying the name of the distribution to use or a list with a component <code>name</code> specifying the distribution and any additional parameters needed. If not specified, <code>gbm</code> will try to guess: if the response has only 2 unique values, bernoulli is assumed; otherwise, if the response is a factor, multinomial is assumed; otherwise, if the response has class &quot;Surv&quot;, coxph is assumed; otherwise, gaussian is assumed.
</p>
<p>Currently available options are &quot;gaussian&quot; (squared error), &quot;laplace&quot; (absolute loss), &quot;tdist&quot; (t-distribution loss), &quot;bernoulli&quot; (logistic regression for 0-1 outcomes),
&quot;huberized&quot; (huberized hinge loss for 0-1 outcomes),
&quot;multinomial&quot; (classification when there are more than 2 classes), &quot;adaboost&quot; (the AdaBoost exponential loss for 0-1 outcomes), &quot;poisson&quot; (count outcomes), &quot;coxph&quot; (right censored observations), &quot;quantile&quot;, or &quot;pairwise&quot; (ranking measure using the LambdaMart algorithm).
</p>
<p>If quantile regression is specified, <code>distribution</code> must be a list of the form <code>list(name="quantile",alpha=0.25)</code> where <code>alpha</code> is the quantile to estimate. The current version's quantile regression method does not handle non-constant weights and will stop.
</p>
<p>If &quot;tdist&quot; is specified, the default degrees of freedom is 4 and this can be controlled by specifying <code>distribution=list(name="tdist", df=DF)</code> where <code>DF</code> is your chosen degrees of freedom.
</p>
<p>If &quot;pairwise&quot; regression is specified, <code>distribution</code> must be a list
of the form 
<code>list(name="pairwise",group=...,metric=...,max.rank=...)</code>
(<code>metric</code> and <code>max.rank</code> are optional, see
below). <code>group</code> is a character vector with the column names of
<code>data</code> that jointly indicate the group an instance belongs to
(typically a query in Information Retrieval  applications).
For training, only pairs of
instances from the same group and with different target labels can be
considered. <code>metric</code> is the IR measure to use, one of 
</p>

<dl>
<dt><code>conc</code>:</dt><dd><p>Fraction of concordant pairs; for binary labels,
this is equivalent to the Area under the ROC Curve</p>
</dd>
<dt><code>mrr</code>:</dt><dd><p>Mean reciprocal rank of the highest-ranked positive instance</p>
</dd>
<dt><code>map</code>:</dt><dd><p>Mean average precision, a generalization of
<code>mrr</code> to multiple positive instances</p>
</dd>
<dt><code>ndcg:</code></dt><dd><p>Normalized discounted cumulative gain. The score is
the weighted sum (DCG) of the user-supplied target values, weighted by
log(rank+1), and normalized to the maximum achievable value. This 
is the default if the user did not specify a metric.</p>
</dd> 
</dl>

<p><code>ndcg</code> and <code>conc</code> allow arbitrary target values, while binary
targets {0,1} are expected for <code>map</code> and <code>mrr</code>. For
<code>ndcg</code> and <code>mrr</code>, a cut-off can be chosen using a positive
integer parameter <code>max.rank</code>. If left unspecified, all ranks are
taken into account.
</p>
<p>Note that splitting of instances into training and validation sets
follows group boundaries and therefore only approximates the specified
<code>train.fraction</code> ratio (the same applies to cross-validation
folds). Internally, queries are randomly shuffled before training, to
avoid bias.
</p>
<p>Weights can be used in conjunction with pairwise metrics, however it is
assumed that they are constant for instances from the same group.
</p>
<p>For details and background on the algorithm, see e.g. Burges (2010).
</p>
</td></tr>
<tr valign="top"><td><code>data</code></td>
<td>
<p>an optional data frame containing the variables in the model. By default the variables are taken from <code>environment(formula)</code>, typically the environment from which <code>gbm</code> is called. If <code>keep.data=TRUE</code> in the initial call to <code>gbm</code> then <code>gbm</code> stores a copy with the object. If <code>keep.data=FALSE</code> then subsequent calls to <code>gbm.more</code> must resupply the same dataset. It becomes the user's responsibility to resupply the same data at this point.</p>
</td></tr>
<tr valign="top"><td><code>weights</code></td>
<td>
<p>an optional vector of weights to be used in the fitting process. Must be positive but do not need to be normalized. If <code>keep.data=FALSE</code> in the initial call to <code>gbm</code> then it is the user's responsibility to resupply the weights to <code>gbm.more</code>.</p>
</td></tr>
<tr valign="top"><td><code>var.monotone</code></td>
<td>
<p>an optional vector, the same length as the number of predictors, indicating which variables have a monotone increasing (+1), decreasing (-1), or arbitrary (0) relationship with the outcome.</p>
</td></tr>
<tr valign="top"><td><code>n.trees</code></td>
<td>
<p>the total number of trees to fit. This is equivalent to the number of iterations and the number of basis functions in the additive expansion.</p>
</td></tr>
<tr valign="top"><td><code>cv.folds</code></td>
<td>
<p>Number of cross-validation folds to perform. If <code>cv.folds</code>&gt;1 then <code>gbm</code>, in addition to the usual fit, will perform a cross-validation, calculate an estimate of generalization error returned in <code>cv.error</code>.</p>
</td></tr>
<tr valign="top"><td><code>interaction.depth</code></td>
<td>
<p>The maximum depth of variable interactions. 1 implies an additive model, 2 implies a model with up to 2-way interactions, etc.</p>
</td></tr>
<tr valign="top"><td><code>n.minobsinnode</code></td>
<td>
<p>minimum number of observations in the trees terminal nodes. Note that this is the actual number of observations not the total weight.</p>
</td></tr>
<tr valign="top"><td><code>shrinkage</code></td>
<td>
<p>a shrinkage parameter applied to each tree in the expansion. Also known as the learning rate or step-size reduction.</p>
</td></tr>
<tr valign="top"><td><code>bag.fraction</code></td>
<td>
<p>the fraction of the training set observations randomly selected to propose the next tree in the expansion. This introduces randomnesses into the model fit. If <code>bag.fraction</code>&lt;1 then running the same model twice will result in similar but different fits. <code>gbm</code> uses the R random number generator so <code>set.seed</code> can ensure that the model can be reconstructed. Preferably, the user can save the returned <code>gbm.object</code> using <code>save</code>.</p>
</td></tr>
<tr valign="top"><td><code>train.fraction</code></td>
<td>
<p>The first <code>train.fraction * nrows(data)</code>
observations are used to fit the <code>gbm</code> and the remainder are used
for computing out-of-sample estimates of the loss function.</p>
</td></tr>
<tr valign="top"><td><code>nTrain</code></td>
<td>
<p>An integer representing the number of cases on which to
train. This is the preferred way of specification for <code>gbm.fit</code>;
The option <code>train.fraction</code> in <code>gbm.fit</code> is deprecated and
only maintained for backward compatibility. These two parameters are
mutually exclusive. If both are unspecified, all data is used for training.</p>
</td></tr>
<tr valign="top"><td><code>keep.data</code></td>
<td>
<p>a logical variable indicating whether to keep the data and an index of the data stored with the object. Keeping the data and index makes subsequent calls to <code>gbm.more</code> faster at the cost of storing an extra copy of the dataset.</p>
</td></tr>
<tr valign="top"><td><code>object</code></td>
<td>
<p>a <code>gbm</code> object created from an initial call to <code>gbm</code>.</p>
</td></tr>
<tr valign="top"><td><code>n.new.trees</code></td>
<td>
<p>the number of additional trees to add to <code>object</code>.</p>
</td></tr>
<tr valign="top"><td><code>verbose</code></td>
<td>
<p>If TRUE, gbm will print out progress and performance indicators. If this option is left unspecified for gbm.more then it uses <code>verbose</code> from <code>object</code>.</p>
</td></tr>
<tr valign="top"><td><code>class.stratify.cv</code></td>
<td>
<p>whether or not the cross-validation should be stratified by class. Defaults to <code>TRUE</code> for <code>distribution="multinomial"</code> and is only implementated for <code>multinomial</code> and <code>bernoulli</code>. The purpose of stratifying the cross-validation is to help avoiding situations in which training sets do not contain all classes.</p>
</td></tr>
<tr valign="top"><td><code>x, y</code></td>
<td>
<p>For <code>gbm.fit</code>: <code>x</code> is a data frame or data matrix containing the predictor variables and <code>y</code> is the vector of outcomes. The number of rows in <code>x</code> must be the same as the length of <code>y</code>.</p>
</td></tr>
<tr valign="top"><td><code>offset</code></td>
<td>
<p>a vector of values for the offset</p>
</td></tr>
<tr valign="top"><td><code>misc</code></td>
<td>
<p>For <code>gbm.fit</code>: <code>misc</code> is an R object that is simply passed on to the gbm engine. It can be used for additional data for the specific distribution. Currently it is only used for passing the censoring indicator for the Cox proportional hazards model.</p>
</td></tr>
<tr valign="top"><td><code>w</code></td>
<td>
<p>For <code>gbm.fit</code>: <code>w</code> is a vector of weights of the same length as the <code>y</code>.</p>
</td></tr>
<tr valign="top"><td><code>var.names</code></td>
<td>
<p>For <code>gbm.fit</code>: A vector of strings of length equal to the number of columns of <code>x</code> containing the names of the predictor variables.</p>
</td></tr>
<tr valign="top"><td><code>response.name</code></td>
<td>
<p>For <code>gbm.fit</code>: A character string label for the response variable.</p>
</td></tr>
<tr valign="top"><td><code>group</code></td>
<td>
<p><code>group</code> used when <code>distribution = 'pairwise'.</code></p>
</td></tr>
<tr valign="top"><td><code>n.cores</code></td>
<td>
<p>The number of CPU cores to use. The cross-validation loop
will attempt to send different CV folds off to different cores. If
<code>n.cores</code> is not specified by the user, it is guessed using the
<code>detectCores</code> function in the <code>parallel</code> package. Note that
the documentation for <code>detectCores</code> makes clear that it is not
failsave and could return a spurious number of available cores.</p>
</td></tr>
</table>


<h3>Details</h3>

<p>See the <a href="../doc/gbm.pdf">gbm vignette</a> for technical details.
</p>
<p>This package implements the generalized boosted modeling framework. Boosting is the process of iteratively adding basis functions in a greedy fashion so that each additional basis function further reduces the selected loss function. This implementation closely follows Friedman's Gradient Boosting Machine (Friedman, 2001).
</p>
<p>In addition to many of the features documented in the Gradient Boosting Machine, <code>gbm</code> offers additional features including the out-of-bag estimator for the optimal number of iterations, the ability to store and manipulate the resulting <code>gbm</code> object, and a variety of other loss functions that had not previously had associated boosting algorithms, including the Cox partial likelihood for censored data, the poisson likelihood for count outcomes, and a gradient boosting implementation to minimize the AdaBoost exponential loss function.
</p>
<p><code>gbm.fit</code> provides the link between R and the C++ gbm engine. <code>gbm</code> is a front-end to <code>gbm.fit</code> that uses the familiar R modeling formulas. However, <code>model.frame</code> is very slow if there are many predictor variables. For power-users with many variables use <code>gbm.fit</code>. For general practice <code>gbm</code> is preferable.</p>


<h3>Value</h3>

 <p><code>gbm</code>, <code>gbm.fit</code>, and <code>gbm.more</code> return a <code>gbm.object</code>. </p>


<h3>Author(s)</h3>

<p>Greg Ridgeway <a href="mailto:gregridgeway@gmail.com">gregridgeway@gmail.com</a>
</p>
<p>Quantile regression code developed by Brian Kriegler <a href="mailto:bk@stat.ucla.edu">bk@stat.ucla.edu</a>
</p>
<p>t-distribution, and multinomial code developed by Harry Southworth and Daniel Edwards
</p>
<p>Pairwise code developed by Stefan Schroedl <a href="mailto:schroedl@a9.com">schroedl@a9.com</a></p>


<h3>References</h3>

<p>Y. Freund and R.E. Schapire (1997) &ldquo;A decision-theoretic generalization of on-line learning and an application to boosting,&rdquo; <em>Journal of Computer and System Sciences,</em> 55(1):119-139.
</p>
<p>G. Ridgeway (1999). &ldquo;The state of boosting,&rdquo; <em>Computing Science and Statistics</em> 31:172-181.
</p>
<p>J.H. Friedman, T. Hastie, R. Tibshirani (2000). &ldquo;Additive Logistic Regression: a Statistical View of Boosting,&rdquo; <em>Annals of Statistics</em> 28(2):337-374.
</p>
<p>J.H. Friedman (2001). &ldquo;Greedy Function Approximation: A Gradient Boosting Machine,&rdquo; <em>Annals of Statistics</em> 29(5):1189-1232.
</p>
<p>J.H. Friedman (2002). &ldquo;Stochastic Gradient Boosting,&rdquo; <em>Computational Statistics and Data Analysis</em> 38(4):367-378.
</p>
<p>B. Kriegler (2007). <a href="http://statistics.ucla.edu/theses/uclastat-dissertation-2007:2">Cost-Sensitive Stochastic Gradient Boosting Within a Quantitative Regression Framework</a>. PhD dissertation, UCLA Statistics.
</p>
<p>C. Burges (2010). &ldquo;From RankNet to LambdaRank to LambdaMART: An Overview,&rdquo; Microsoft Research Technical Report MSR-TR-2010-82.
</p>
<p><a href="http://sites.google.com/site/gregridgeway">Greg Ridgeway's site</a>.
</p>
<p>The <a href="http://www-stat.stanford.edu/~jhf/R-MART.html">MART</a> website. </p>


<h3>See Also</h3>

 <p><code>gbm.object</code>, <code>gbm.perf</code>, <code>plot.gbm</code>,
<code>predict.gbm</code>, <code>summary.gbm</code>, <code>pretty.gbm.tree</code>. </p>


<h3>Examples</h3>

```r
 # A least squares regression example # create some data

N <- 1000
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
gbm1 <-
gbm(Y~X1+X2+X3+X4+X5+X6,         # formula
    data=data,                   # dataset
    var.monotone=c(0,0,0,0,0,0), # -1: monotone decrease,
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