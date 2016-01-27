---
title       : Coursera Data Science Course 9 Project
subtitle    : Analyis of Random Forest Algorithm Tree Count
author      : Jeremy Hess 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Project Description

This program uses the mtcars data set and a random forest prediction algorithm to predict whether the car is an automatic or manual transmission.  The following variables are provided to the algorithm for use:
amFlag, mpg, cyl, disp, hp, drat, wt, qsec, vs, gear, carb.

Just choose the number of trees to run using the slide bar and then press the Submit button.  
This app will run generate the model and display 
the test accuracy based on your choice.

The data was extracted from the 1974 Motor Trend US magazine, and comprises fuel consumption and 10 aspects of automobile design and performance for 32 automobiles (1973-74 models).

--- .class #id 
## Code for Preparing the Data


```r
library(e1071)
library(UsingR)
library(lattice)
library(ggplot2)
library(caret)
library(randomForest)
library(dplyr)

data("mtcars")
mtcarsAllData <- mutate(mtcars,amFlag = ifelse(mtcars$am ==1,'Y','N') )
set.seed(4242)
inTrain = createDataPartition(mtcarsAllData$amFlag,p=.9)[[1]]
training = mtcarsAllData[ inTrain,]
trainFieldList <- c("amFlag","mpg","cyl","disp","hp","drat","wt","qsec","vs","gear","carb")
train <- training[,trainFieldList]
```


--- .class #id 
## Build the Model and show the results


```r
treeCount <- 5
rfModFit <- train(amFlag ~ .,data = train, method = 'rf', ntree=treeCount, importance=TRUE,verbose=FALSE)
```


```r
rfModFit$finalModel  
```

```
## 
## Call:
##  randomForest(x = x, y = y, ntree = ..1, mtry = param$mtry, importance = TRUE,      verbose = FALSE) 
##                Type of random forest: classification
##                      Number of trees: 5
## No. of variables tried at each split: 6
## 
##         OOB estimate of  error rate: 20.83%
## Confusion matrix:
##    N Y class.error
## N 11 3   0.2142857
## Y  2 8   0.2000000
```

--- .class #id 
## Summary

You can see that the estimate of error rate is 20.83% with five trees.  This application allows you to increase or decrease the value of the treeCount variable and review the accuracy of the model.  What you will find is that the accuracy does not alway improve a the number of trees increases.  

Note: This example was created to show the functionality of Shiny.  I may have not chosen the best model for this problem.  Also, I definitely did not complete the analysis.  I simply created an example showing how you can allow a user to modify the settings for a prediction algorithm to impact the accuracy and results.


