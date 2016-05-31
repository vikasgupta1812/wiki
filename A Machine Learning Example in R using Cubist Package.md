## A Machine Learning Example in R using Cubist

[Source](https://www.linkedin.com/pulse/machine-learning-example-r-using-cubist-kirk-mettler)
Cubist is a machine learning algorithm for continous outcomes. Cubist is a __rule-based decision tree__ that automatically deals with missing values. This makes using Cubist ideal for baselining the perdictive value of your data set because if it is messy with a lot missing values, you do not have to deal with it. Cubist has become my first-try model for all continous outcome data sets.

Cubist was developed by Quinlan, and the R package for Cubist is maintained by Max Kuhn who also maintains the Caret package.

The code for calling a Cubist model is fairly standard for most predictive models in R.

	cubist( x= trainingpredictors, y = trainingoutcomes)

There are some other elements that help improve the basic Cubist model’s performance, but let’s start with the simple model and go from there. For this example, we are going to use the `BostonHousing` data set the is contained in the `mlbench` package. It is a very well-know data set with 506 rows and 19 variables. Let’s look at that data set before we move on to creating and evaluating a predictive model in R.

```
require(mlbench)
require(caret)
require(Cubist)
data(BostonHousing)
dim(BostonHousing)
## [1] 506 14
```

```
str(BostonHousing)
## 'data.frame': 506 obs. of 14 variables:
## $ crim : num 0.00632 0.02731 0.02729 0.03237 0.06905 ...
## $ zn : num 18 0 0 0 0 0 12.5 12.5 12.5 12.5 ...
## $ indus : num 2.31 7.07 7.07 2.18 2.18 2.18 7.87 7.87 7.87 7.87 ...
## $ chas : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
## $ nox : num 0.538 0.469 0.469 0.458 0.458 0.458 0.524 0.524 0.524 0.524 ...
## $ rm : num 6.58 6.42 7.18 7 7.15 ...
## $ age : num 65.2 78.9 61.1 45.8 54.2 58.7 66.6 96.1 100 85.9 ...
## $ dis : num 4.09 4.97 4.97 6.06 6.06 ...
## $ rad : num 1 2 2 3 3 3 5 5 5 5 ...
## $ tax : num 296 242 242 222 222 222 311 311 311 311 ...
## $ ptratio: num 15.3 17.8 17.8 18.7 18.7 18.7 15.2 15.2 15.2 15.2 ...
## $ b : num 397 397 393 395 397 ...
## $ lstat : num 4.98 9.14 4.03 2.94 5.33 ...
## $ medv : num 24 21.6 34.7 33.4 36.2 28.7 22.9 27.1 16.5 18.9 ...
```

As you can, see it is a data set with 506 rows and 19 columns of all numeric values. We are going to try to predict the value of the last column (medv) which is the median value of owner-occupied homes in USD 1000’s. Here is a description of the data in each of the 19 columns.

- `crim` crime rate of town 
- `zn` proportion of residential land zoned for lot over 25,000 sq.ft. 
- `indus` proportion of non-retail business acres per town 
- `chas` Charles River Dummy Variable ( = 1 if tract bounds Charles River, = 0 if not) 
- `nox` nitrix oxides concentration in parts per 10 million 
- `rm` average number of rooms per dwelling 
- `age` proportion of owner occupied units built before 1940 
- `dis` weighted distances to five Boston Employment centers 
- `rad` index of accessibility to radial highways 
- `tax` full value property tax per USD 10,000 
- `ptratio` pupil to teacher ratio per town b 1000(B-0.63)^2 where B is the proportion of African Americans in the town 
- `lstat` percentage of lower status of the population 
- `medv` median value of owner-occupied homes in USD 1000’s

Normally when we build a predictive model, we break that data set into two or three data sets - training, test, and hold out data set. That may differ slightly if we are using cross-validation, but in general I make a training and a test set. Here I will use an 80/20 split . I am also going to do a little modification to the `chas` variable

	BostonHousing$chas <- as.numeric(BostonHousing$chas) - 1

	set.seed(1)

	inTrain <- sample(1:nrow(BostonHousing), floor(.8*nrow(BostonHousing)))

	trainingPredictors <- BostonHousing[ inTrain, -14]
	trainingOutcome <- BostonHousing$medv[ inTrain]

	testPredictors <- BostonHousing[-inTrain, -14]
	testOutcome <- BostonHousing$medv[-inTrain]
Now all we have to do is fit the model, make a prediction and then evaluate the prediction. Since we are predicting a continous variable here, we will use Root Mean Squared Error (RSME).

Fit the model


	modelTree <- cubist(x = trainingPredictors, y = trainingOutcome)
	modelTree
	## 
	## Call:
	## cubist.default(x = trainingPredictors, y = trainingOutcome)
	## 
	## Number of samples: 404 
	## Number of predictors: 13 
	## 
	## Number of committees: 1 
	## Number of rules: 4
Look at the model

	summary(modelTree)
	## 
	## Target attribute `outcome'
	## 
	## Read 404 cases (14 attributes) from undefined.data
	## 
	## Model:
	## 
	## Rule 1: [88 cases, mean 13.81, range 5 to 27.5, est err 2.10]
	## 
	## if
	## nox > 0.668
	## then
	## outcome = 2.07 + 3.14 dis - 0.35 lstat + 18.8 nox + 0.007 b
	## - 0.12 ptratio - 0.008 age - 0.02 crim
	## 
	## Rule 2: [153 cases, mean 19.54, range 8.1 to 31, est err 2.16]
	## 
	## if
	## nox <= 0.668
	## lstat > 9.59
	## then
	## outcome = 34.81 - 1 dis - 0.72 ptratio - 0.056 age - 0.19 lstat + 1.5 rm
	## - 0.11 indus + 0.004 b
	## 
	## Rule 3: [39 cases, mean 24.10, range 11.9 to 50, est err 2.73]
	## 
	## if
	## rm <= 6.23
	## lstat <= 9.59
	## then
	## outcome = 11.89 + 3.69 crim - 1.25 lstat + 3.9 rm - 0.0045 tax
	## - 0.16 ptratio
	## 
	## Rule 4: [128 cases, mean 31.31, range 16.5 to 50, est err 2.95]
	## 
	## if
	## rm > 6.23
	## lstat <= 9.59
	## then
	## outcome = -1.13 + 1.6 crim - 0.93 lstat + 8.6 rm - 0.0141 tax
	## - 0.83 ptratio - 0.47 dis - 0.019 age - 1.1 nox
	## 
	## 
	## Evaluation on training data (404 cases):
	## 
	## Average |error| 2.27
	## Relative |error| 0.34
	## Correlation coefficient 0.94
	## 
	## 
	## Attribute usage:
	## Conds Model
	## 
	## 78% 100% lstat
	## 59% 53% nox
	## 41% 78% rm
	## 100% ptratio
	## 90% age
	## 90% dis
	## 62% crim
	## 59% b
	## 41% tax
	## 38% indus
	## 
	## 
	## Time: 0.0 secs
Make a prediction

	mtPred <- predict(modelTree, testPredictors)
Get the RMSE

	sqrt(mean((mtPred - testOutcome)^2))
	## [1] 3.337924
That is not bad, but we can do better using Committees and Neighbors

Model Committes are created by generating a rule-based sequence of models similar to boosting. The number of committees can range from 1 to 100.

Let’s do a committee Cubist model with committees set to 100

	set.seed(1)
	committeeModel <- cubist(x = trainingPredictors, y = trainingOutcome, committees = 100)
	## Get RMSE of COmmittee
	comPred <- predict(committeeModel, testPredictors)
	## RMSE
	sqrt(mean((comPred - testOutcome)^2))
	## [1] 2.779002
Now let’s add neighbors to the committees, which adjusts the model based adjacent solutions.

	instancePred <- predict(committeeModel, testPredictors, neighbors = 4)
	sqrt(mean((instancePred - testOutcome)^2))
	## [1] 2.566348
So now the question is, **what combination of committees and neighbors yields the best prediction?** We can answer that by creating a vector of possible committees, and a vector of possible neighbors, then seeing where the RSME is best.

	set.seed(1)
	cTune <- train(x = trainingPredictors, y = trainingOutcome,"cubist",
					tuneGrid = expand.grid(.committees = c(1, 10, 50, 100),
					.neighbors = c(0, 1, 5, 9)),
					trControl = trainControl(method = "cv"))
	cTune
	## Cubist 
	## 
	## 404 samples
	## 13 predictor
	## 
	## No pre-processing
	## Resampling: Cross-Validated (10 fold) 
	## Summary of sample sizes: 363, 363, 363, 363, 362, 365, ... 
	## Resampling results across tuning parameters:
	## 
	## committees neighbors RMSE Rsquared RMSE SD Rsquared SD
	## 1 0 4.081800 0.7916640 1.3007653 0.15005686 
	## 1 1 4.111955 0.7950087 1.2540113 0.13995896 
	## 1 5 3.943515 0.8054412 1.2727587 0.14070680 
	## 1 9 3.959522 0.8022459 1.3305391 0.14884521 
	## 10 0 3.371765 0.8597818 0.9354412 0.08111265 
	## 10 1 3.370218 0.8681521 0.8462733 0.07253983 
	## 10 5 3.168392 0.8767757 0.9409569 0.07777561 
	## 10 9 3.207153 0.8725973 0.9499315 0.07980860 
	## 50 0 3.238911 0.8704658 0.9819922 0.08369843 
	## 50 1 3.257555 0.8741483 0.9284914 0.08006349 
	## 50 5 3.035711 0.8845178 1.0167411 0.08284853 
	## 50 9 3.071004 0.8810091 1.0233749 0.08444221 
	## 100 0 3.211165 0.8713608 1.0185290 0.08500905 
	## 100 1 3.254918 0.8739276 0.9853192 0.08458200 
	## 100 5 3.005851 0.8855715 1.0492541 0.08529563 
	## 100 9 3.044205 0.8820627 1.0572761 0.08671512 
	## 
	## RMSE was used to select the optimal model using the smallest value.
	## The final values used for the model were committees = 100 and neighbors
	## = 5.
As you can see, Cubist does really well as a predictive model.

