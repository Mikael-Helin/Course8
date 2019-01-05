# Course 8 Assignment

1. First read the CSV and set na.strings=c("NA","#DIV/0!","") to make cleaning more simple.

2. Removed column names with at least 80% NA (and null and 'division by 0' combined) and also removed column names without "accel" just as described in the lab instructions.

3. Partitioned 60% of data for training, 20% for testing and 20% for validation.

4. Trained with Random Forest (RF) and Gradient Boosting (GB). I think non-linear methods are strongest in such case as for this lab, which is the reason why I picked these two methods. Because of curiousity I also picked Linear Discriminant Analysis. My expectation was to get 70% accuracy with RF and GB and 50% with LDA. A reason for believing in such low accuracy, was that I threw away so many factors according to the instructions.

5. Predicted and viewed confusion matrices of RF, GB and LDA. The predictions were compared with the testing set. It showed the results were 94%, 83% and 52% for respective method. RF was much above my expectations.

6. I believed that stacking would improve my method by another 1-2% in the validation set, but I could not see any improvment by stacking further with RF, GAM, GB or LDA, so I continued to use the original RF method to pass the quiz.

7. Thank you! :)


suppressMessages(library(caret))
set.seed(0)

train<-read.csv(file="pml-training.csv", header=TRUE, sep=",",na.strings=c("NA","#DIV/0!",""))
test<-read.csv(file="pml-testing.csv", header=TRUE, sep=",",na.strings=c("NA","#DIV/0!",""))

mynames<-intersect(names(train[,colSums(is.na(train))/nrow(train)<0.8]),names(train))
mynames<-mynames[grepl("accel",mynames)]

train<-train[,c(mynames,"classe")]
test<-test[,mynames]

inTrain<-createDataPartition(y=train$classe,p=.6,list=F)
training<-train[inTrain,]
nonTraining<-train[-inTrain,]
inTest<-createDataPartition(y=nonTraining$classe,p=.5,list=F)
testing<-nonTraining[inTest,]
validating<-nonTraining[-inTest,]

mod_rf<-train(classe~.,data=training,method="rf")
mod_gbm<-train(classe~.,data=training,method="gbm")
mod_lda<-train(classe~.,data=training,method="lda")

pred_rf<-predict(mod_rf,testing)
pred_gbm<-predict(mod_gbm,testing)
pred_lda<-predict(mod_lda,testing)
confusionMatrix(pred_rf,testing$classe)$overall
confusionMatrix(pred_gbm,testing$classe)$overall
confusionMatrix(pred_lda,testing$classe)$overall

DF_comb<-data.frame(pred_rf,pred_gbm,pred_lda,classe=testing$classe)
mod_comb<-train(classe~.,method="gbm",data=DF_comb)
pred_comb<-predict(mod_comb,DF_comb)
confusionMatrix(pred_comb,validating$classe)$overall

pred_quiz<-predict(mod_rf,test)

