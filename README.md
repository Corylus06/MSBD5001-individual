# MSBD5001-individual
In-class competition of course MSBD5001
## Programming language
Python3.8
## Required packages
pandas, numpy, sklearn, matplotlib
## How to start
You will be able to reproduce the results by running every cell sequencely.
## Structure
There are two folders, the 'EDA' folder and 'train' folder.

### 1.EDA folder 
It contains files for exploratory data analysis, including split.ipynb, 2017.csv, 2018.csv, EDA_2017.ipynb, EDA_2018.ipynb.

split.ipynb is used to divide the dataset by year. Due to the incomplete data in 2018, it is necessary to separate the data annually. Data of 2017 can be used to better observe data trends. In addition to division, missing values need to be handled appropriately. As described in the overview, speed information is sometimes lost due to equipment failure. Due to the small difference in quantity, I chose to fill in the data with linear filling to ensure the trend as much as possible

2017.csv and 2018.csv is the output of split.ipynb, within which only contains data of corresponding year.

EDA_2017.ipynb and EDA_2018.ipynb are the EDA (exploratory data analysis) processes in 2017 and 2018. On this basis, we can see that the data distribution is fluctuating every month, every week, and every hour, so we need to extract these contents to form our features in the modeling part.

### 2.train folder
At first, I try the KNN model. However, random forest, as an integrated learning model, composed of multiple weak learners (decision trees) has a strong learning ability. Moreover, the bagging calculation reflected the generalization ability can effectively reduce the occurrence of over-fitting phenomenon. So I choose random forest model instead.

randomForest.ipynb, it is the main file.

The whole process is divided into six steps.

Step 1. Read data from the CSV file and preprocess it. Some features that affect speed are extracted from the 'date' data: hour, day of week, date, month, and timestamp. They were standardized respectively.

Step 2. Fit the model preliminarily, and plot the fitting situation. At the same time, display the score, MSE, RSS, bag score, etc. to evaluate the model performance. Currently, the score is 0.9265

Step 3. Adjust the model parameters to further improve the model performance.

Step 4. Fit all the adjusted parameters into the model and check the performance improvement. At this time, the score is increased to 0.9336

Step 5. Read 'test.csv', extract the feature, use the generated model to predict. The prediction results are obtained

Step 6. Store the prediction results in the desired form.


rf-final.csv, it is the submit CSV file.

train.csv and test.csv are the raw data given by contest.
