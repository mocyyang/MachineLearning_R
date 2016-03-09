ML Project


Background:

Data: Six young health participants were asked to perform one set of 10 repetitions of the Unilateral Dumbbell Biceps Curl in five different fashions (variable “classe”). Class A corresponds to the specified execution of the exercise, while the other 4 classes correspond to common mistakes.

Task: This program uses the “Random Forest” with n-fold cross validation in the R caret package to the “classe” variable given a list of input features. Below is a step by step process:


1. Load the data: 

library(caret)
## Loading required package: lattice
## Loading required package: ggplot2
trainData_URL<-"http://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv"
testData_URL<-"http://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv"
trainData<-read.csv(trainData_URL,na.strings=c("NA",""))
testData<-read.csv(testData_URL,na.strings=c("NA",""))



2. Data clean-up (get rid of trivial or mostly “NA” columns) 

trainData<-trainData[,7:160]
nonNA_data<-apply(!is.na(trainData),2,sum)>19621
trainData<-trainData[,nonNA_data]


3. Data partition 

InTrain<-createDataPartition(y=trainData$classe,p=0.5,list=FALSE)
training1<-trainData[InTrain,]



4. Train the model use the random forest method with 10 fold corss validation to train
 
  fit_rf<-train(classe~.,data=training1,method="rf",
                trControl=trainControl(method="cv",number=10),
                prox=TRUE,allowParallel=TRUE)
## Loading required package: randomForest
## randomForest 4.6-12
## Type rfNews() to see new features/changes/bug fixes.
## 
## Attaching package: 'randomForest'
## The following object is masked from 'package:ggplot2':
## 
##     margin
print(fit_rf)
## Random Forest 
## 
## 9812 samples
##   53 predictor
##    5 classes: 'A', 'B', 'C', 'D', 'E' 
## 
## No pre-processing
## Resampling: Cross-Validated (10 fold) 
## Summary of sample sizes: 8831, 8831, 8832, 8831, 8830, 8830, ... 
## Resampling results across tuning parameters:
## 
##   mtry  Accuracy   Kappa      Accuracy SD  Kappa SD   
##    2    0.9903183  0.9877535  0.003879918  0.004907193
##   27    0.9960252  0.9949722  0.002736949  0.003461920
##   53    0.9914393  0.9891717  0.002725878  0.003445849
## 
## Accuracy was used to select the optimal model using  the largest value.
## The final value used for the model was mtry = 27.

