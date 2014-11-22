setwd("/Users/caris/Documents/projects/DataScience/PracticalMachineLearning")

#Step 1. Loading Data
```  
traindata <- read.csv("pml-training.csv")
testdata <- read.csv("pml-testing.csv")
dim(training)
```
[1] 19622   160
```
dim(testing)
```
[1]  20 160

#Step 2.  Preparing data for analysis
```
library(caret)
set.seed(34543)
trainset <- createDataPartition(traindata$classe, p = 0.6, list = FALSE)
training <- traindata[trainset, ]
validation <- traindata[-trainset, ]
```

#Step 3. nearZeroVar return where co-variance is almost 
```
zeronzv <- nearZeroVar(training)
training_nzv  <- training[ ,-nzv]
```
#Step 4. Using Training data to model using Rain Forest Algorithm
```
modelRainForest <-train(classe~.,data=training_nzv,method="rf")
modelRainForest

Random Forest 

11776 samples
   61 predictor
    5 classes: 'A', 'B', 'C', 'D', 'E' 

No pre-processing
Resampling: Bootstrapped (25 reps) 

Summary of sample sizes: 142, 142, 142, 142, 142, 142, ... 

Resampling results across tuning parameters:

  mtry  Accuracy   Kappa      Accuracy SD  Kappa SD  
   2    0.7158013  0.6359672  0.055653690  0.07217477
  42    0.9687090  0.9600441  0.032741147  0.04159855
  83    0.9983974  0.9979479  0.005551445  0.00710876

Accuracy was used to select the optimal model using  the largest value.
The final value used for the model was mtry = 83. 
```

#Step 4. Validation and Out of Sample Error
```
predictTraining <- predict(modelRainForest, training_nzv)
confusionMatrix(predictTraining, training_nzv$classe)
predictTraining <- predict(modelRainForest, validation)
confusionMatrix(predictTraining, training$classe) 
```

#Step 5. Predict Value for given data
```
predictTraining <- predict(modelRainForest, testing)
confusionMatrix(predictTraining, training$classe)
```
