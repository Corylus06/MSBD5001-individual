# MSBD5001-individual
In-class competition of course MSBD5001
## Programming language
Python3.8
## Required packages
pandas, numpy, sklearn, matplotlib, xgboost
## How to start
You will be able to reproduce the results by running every cell sequencely on train/2_model_merge_lr_xgboost.ipynb
## Structure
There are two folders, the 'EDA' folder and 'train' folder.

### 1.EDA folder 
It contains files for exploratory data analysis, including split.ipynb, 2017.csv, 2018.csv, EDA_2017.ipynb, EDA_2018.ipynb.

#### split.ipynb 
It is used to divide the dataset by year. Due to the incomplete data in 2018, it is necessary to separate the data annually. Data of 2017 can be used to better observe data trends. In addition to division, missing values need to be handled appropriately. As described in the overview, speed information is sometimes lost due to equipment failure. Due to the small difference in quantity, I chose to fill in the data with linear filling to ensure the trend as much as possible

#### 2017.csv and 2018.csv 
They are the output of split.ipynb, within which only contain data of corresponding year.

#### EDA_2017.ipynb and EDA_2018.ipynb 
They are the EDA (exploratory data analysis) processes in 2017 and 2018. On this basis, we can see that the data distribution is fluctuating every month, every week, and every hour, so we need to extract these contents to form our features in the modeling part.

#### train.csv
It is the raw training data given by contest.

### 2.train folder
At first, I try the KNN model. However, xgboostRegressor, as an integrated learning model, composed of multiple weak learners has a strong learning ability. It has other advantages, such as, regularization, parallel processing, high flexibility, and tree pruning, ect. As a result, I choose random forest model instead.

I also find out that model fusion might achieved a even better result. As a result, I choose to use the tuned xgboost model as the first layer of the confusion model, and Logistic Regression as the second layer, in this way, I have successfully achieved a better result. 

#### xgboost_final_output.csv
It is the submit CSV file.

#### xgboost_model1_pred.csv, xgboost_model2_pred.csv, xgboost_model3_pred.csv

Output for each tuned xgboost models. These models are used as the first layer of model fusion.

#### train.csv and test.csv 
They are the raw data given by contest.

#### 2_model_merge_lr_xgboost.ipynb

This is the main file. We attain three xgboost model that have achieved a relatively good result, to improve the generaliztion ability, we choose to ensemble those models. Here, two layers of model fusion are used. Level 1 uses: 3 XGBoost models with different parameters, and Level 2 uses LinearRegression to fit the results of the first layer.

#### 1_model_tuning_params1.ipynb, 1_model_tuning_params2.ipynb, 1_model_tuning_params3.ipynb
Tunning parameters and presenting the result on the testing set. Choose three models that performs relatively better.

The whole process is divided into six steps.

1. Read data from the CSV file and preprocess it. Some features that affect speed are extracted from the 'date' data: hour, day of week, date, month, and timestamp. They were standardized respectively. We can also see the importance of each feature through xgboost. 
   Based on EDA, we can discover that there is a big difference in the distribution of traffic speed between weekdays and weekends. Meanwhile, through the average velocity distribution, we can see that the velocity of identical months and each day within each month has a large fluctuation. So it's admirable to take month, date, day of week, and hour as the feature of the model, and also include the given timestamp as an analysis feature after normalization

2. Fit the model preliminarily, and plot the fitting situation. At the same time, display the score, MSE, RSS, bag score, etc. to evaluate the model performance. Currently, the <img src="http://latex.codecogs.com/gif.latex?{R}^{2}" /> score is 0.9457

3. Adjust the model parameters to further improve the model performance. Here we introduce the main parameters that have been tuned to form a better model.
> - n_estimator
> Number of gradient boosted trees. Equivalent to number of boosting rounds. The larger the n_estimator, the better performance of xgboost Regressor, but after certain round, increasing this parameter will not increase the performance significantly
> - max_depth && min_child_weight
> 'max_depth' is the maximum depth of a tree, 'min_child_weight' defines the minimum sum of weights of all observations required in a child. Both works for avoiding overfitting, the smaller the depth, the higher the min_child_weight, the better model generalization.
> - gamma
> Makes the algorithm conservative. The values can vary depending on the loss function and should be tuned.
> - subsample && colsample_bytree
> Also aiming at avoiding overfitting. 'subsample' denotes the fraction of observations to be randomly samples for each tree. 'colsample_bytree' denotes the fraction of columns to be randomly samples for each tree, as the nuumber of feature selected for this model is small, usually we keep 'colsample_bytree' to be one.
> - reg_alpha && reg_lambda
> reg_alpha is the l1 regularization term on weight, while reg_lambda is the l2 regularization ter on weights. Also work as ways to avoid overfitting.
> - learning_rate
> A higher learning rate helps the model converge quicker, but it may affect the precision, so we need to check whether lowering the learning rate may lead to a better result.

4. Fit all the adjusted parameters into the model and check the performance improvement. At this time, the <img src="http://latex.codecogs.com/gif.latex?{R}^{2}" /> score is increased to 0.9490

5. Read 'test.csv', extract the feature, use the generated model to predict. The prediction results are obtained

6. Store the prediction results in the desired form.


