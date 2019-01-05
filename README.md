# Course 8 Assignment

1. First read the CSV and set na.strings=c("NA","#DIV/0!","") to make cleaning more simple.

2. Removed column names with at least 80% NA (and null and 'division by 0' combined) and also removed column names without "accel" just as described in the lab instructions.

3. Partitioned 60% of data for training, 20% for testing and 20% for validation.

4. Trained with Random Forest (RF) and Gradient Boosting (GB). I think non-linear methods are strongest in such case as for this lab, which is the reason why I picked these two methods. Because of curiousity I also picked Linear Discriminant Analysis. My expectation was to get 70% accuracy with RF and GB and 50% with LDA. A reason for believing in such low accuracy, was that I threw away so many factors according to the instructions.

5. Predicted and viewed confusion matrices of RF, GB and LDA. The predictions were compared with the testing set. It showed the results were 94%, 83% and 52% for respective method. RF was much above my expectations.

6. I believed that stacking would improve my method by another 1-2% in the validation set, but I could not see any improvment by stacking further with RF, GAM, GB or LDA, so I continued to use the original RF method to pass the quiz.

7. Thank you! :)

> confusionMatrix(pred_rf,testing$classe)$overall
      Accuracy          Kappa  AccuracyLower  AccuracyUpper 
  0.9421361203   0.9268047385   0.9343667992   0.9492383388 
  AccuracyNull AccuracyPValue  McnemarPValue 
  0.2844761662   0.0000000000   0.0004292635 
> confusionMatrix(pred_gbm,testing$classe)$overall
      Accuracy          Kappa  AccuracyLower  AccuracyUpper 
  8.251338e-01   7.784878e-01   8.128790e-01   8.369023e-01 
  AccuracyNull AccuracyPValue  McnemarPValue 
  2.844762e-01   0.000000e+00   1.685778e-15 
> confusionMatrix(pred_lda,testing$classe)$overall
      Accuracy          Kappa  AccuracyLower  AccuracyUpper 
  5.197553e-01   3.844292e-01   5.039840e-01   5.354971e-01 
  AccuracyNull AccuracyPValue  McnemarPValue 
  2.844762e-01  6.696869e-210  5.161408e-114 
