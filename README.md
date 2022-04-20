# Azure-ML-vs-AWS-SageMaker-

## Overview
In this tutorial, you will go through various ways of importing, transforming, analyzing, and exporting data with SageMaker. This tutorial will walk you through the steps in Data Wrangler within the AWS SageMaker Studio, AWS SageMaker Canvas, and AWS SageMaker Studio Notebooks. You will learn to:

 * Create

## Data Wrangler 

### Step 0: Before You Start 
* AWS Account - Sign up for a free AWS account here 



### Step 1: Download Dataset and Upload to S3 Bucket 
* Download Titanic Dataset here - Grab the .CSV file
* Navigate to AWS portal and type S3 in the Search Bar 
  *  Hit Create a bucket:  Name your bucket and leave all other settings as default.

![p1](https://user-images.githubusercontent.com/33441411/164117324-657bf0ce-5a65-4418-b653-0271424d61f2.png)

  *  Upload .csv file into the created bucket using the Upload button

### Step 2: Set up Studio
* Navigate to AWS portal and type in AWS SageMaker 
* Create a SageMaker Domain 

![p2](https://user-images.githubusercontent.com/33441411/164118649-8be00282-58d8-4edd-9c88-4b32ee6db4ea.png)
![p3](https://user-images.githubusercontent.com/33441411/164118664-d9344532-092b-445b-b422-b143f94b097f.png)

### Step 3: Get Started with Data Wrangler 
* Hit Launch App -> Studio 
![p4](https://user-images.githubusercontent.com/33441411/164119815-137ae878-cb0b-4369-add3-63069cbae506.png)

* Create a Data Wrangler Flow by going to File -> New -> Data Wrangler Flow
![p5](https://user-images.githubusercontent.com/33441411/164119824-00eb5c9b-5176-4b6b-842e-b4cb557cbead.png)

* Import S3 Data 
  * Choose your S3 bucket and Click on the .CSV file that you uploaded. You should be able to see a Preview version of your data on the very bottom. Then click Import at the very top.
![p6](https://user-images.githubusercontent.com/33441411/164120598-5607027e-9e82-4a5b-b681-9c56907ab660.png)
![p7](https://user-images.githubusercontent.com/33441411/164120605-5c99301a-fd1e-41b9-9a27-a4f0b68674dc.png)

### Step 4: Begin adding to the Flow
* In the Data Flow, you should just see 2 steps - the data from S3 and the data types. Data Wrangler has inferred the data types for you already.
* Analyze: Now let's start analyzing the data. To get a feel of the data, we can do + Add Analysis. For Analysis Type, click on "Table Summary" from the Drop Down. As you can see from the output, there are many missing values in the columns: cabin, embarked, and home.dest. In addition, there are outliers as well. You can play around with other pre-built Analysis features like creating a Histogram. 
![p8](https://user-images.githubusercontent.com/33441411/164122242-d4e9da00-4cfc-4b0c-b17f-a09d64bd0d46.png)
![p9](https://user-images.githubusercontent.com/33441411/164122250-9e468961-b2d9-41f5-b423-b00163fb0bea.png)

* Transform: Now let's transform the data. This is where you can manipulate the data by cleaning it up. Click on + Add Transform. Then + Add Step on the top right of the screen. 
   * Drop Columns: Now here, we want to select the built in option to drop the columns that are unnecessary. In Transform, we are going to select the Drop column option. And for column's to drop, we will select: cabin, ticket, name, sibsp, parch, home.dest, boat, and body. Then hit preview. And finally, click Add.
   ![p10](https://user-images.githubusercontent.com/33441411/164125305-881e8c1c-f4d6-44bd-8669-f85f1181105b.png)

   * Missing Data: + Add Step again. Click on the Handle Missing built in option. Under Transform, click on Drop missing. Then for Input columns, select age. Then hit preview. Lastly, click Add.  Repeat this step for the column, fare.

   ![p11](https://user-images.githubusercontent.com/33441411/164125333-0dd3f095-b68f-4871-a2e9-b82d290691af.png)
   
   * Let's take a look at the Custom Tranform option. Select Python(Pandas). Enter this query: df.info(). You will see 1045 entries that are remaining after all the transformations. We do not have to save this. Let's click on Custom transform again. Select Python(Pandas). Insert this code.

import pandas as pd

dummies = []
cols = ['pclass','sex','embarked']
for col in cols:
    dummies.append(pd.get_dummies(df[col]))
    
encoded = pd.concat(dummies, axis=1)

df = pd.concat((df, encoded),axis=1)

Hit preview and click Add. You should see some new columns on the right.

 
    * SQL: Hit Custom Transform again. Click on SQL(PySpark SQL). Here we want to select the columns we want to keep. Insert this code:

SELECT survived, age, fare, 1, 2, 3, female, male, C, Q, S FROM df;






