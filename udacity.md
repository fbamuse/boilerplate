
#Background

![](.spokify/2020-09-05-20-20-07.png)

The music distribution service, which pioneered subscription services, has become a part of our lives and has changed the way we enjoy music.
Since various providers compete, you can easily use one-click joining and leaving, free trial, service promotion, etc.,
Users can choose the service that suits their taste and style.
Gaining market share is very important because the main sources of income for music distribution are advertising revenue and user fees. 
The lifeline of the service is to grasp the user's satisfaction and trends, and to maintain and acquire users through first-class services.
They are aiming to provide music tailored to each customer, and is trying to differentiate itself from others.

In these subscription services, operation logs are a powerful tool for grasping the market.


# General

In this project, from the user operation log of music distribution service SPOTIFY
Build a classification model that identifies users who are likely to opt out without being satisfied with the service.


# Software Requirements
- Python3
- Pyspark 2.4.3
- Plotly 2.0.15

# Data
Sparkify user operation log From October 1, 2018 to early December
- file name:"mini_sparkify_event_data.json"  

The data is a time-series log that records user operations for two months.
In real business, we need to handle a larger amount of data, so we will adopt Pyspark, a scalable analysis platform.
Note:The data contains personal information and can only be accessed by Udacity teachers and students.


# Steps
## Data

Data contain following columns
![](2020-09-05-20-28-37.png)


column"page" show fotprint.

![](2020-09-05-20-31-31.png)

## Define Churn(Define Objective variable)

The objective variable on the classification model is defined as the history of access 
to the "Cancellation Confirmation" page, which is the operation for identifying the canceled user in the operation log.

## Explore Data    

From the original log, we will derive and confirm the variables that are considered to have a high correlation with the churn user.
I paid attention to the following.

-Ratio of cancellations by men and women

-Rate of cancellation with paid members and free members

-Average of song plays per session

-Average of song plays per day

-Standard deviation of songs play per day

-Standard deviation of songs play per month

-Total length to play the song so far

-Number of likes and dislike

-Number of songs added to playlist

-Number of friends

-days from registration


**Some examples**
![](2020-09-05-20-54-37.png)

![](2020-09-05-20-55-52.png)

![](2020-09-05-20-56-37.png)

![](2020-09-05-20-59-16.png)



Logs are time-series data that record each operation by individual users. 
Now, We will convert into characteristic data for each user.


### Correlation coefficient
When the variables are identified, check the correlation between the objective variable and each variable and find a valid variable.

![](2020-09-05-20-01-32.png)

### Metric Definition
This is unbalanced data.
There are only a small percentage of churners, so if you predict not to churn all, Recall will get a good score.
Precision and Recall are not suitable for evaluating the model.Uses F-Scoore.

### Model selection
Check the following three methods supported by PysparkML.


#### Logistic Regression
when the objective variable and the design variable have a linear relationship, it is an excellent method in terms of calculation cost and model readability. Check as a base model.
On the other hand, it should be noted that the prediction accuracy deteriorates due to Multicol linearity when the variables are highly correlated.

#### Random Forest/ GBT
Assuming the nonlinearity of the objective variable and the design variable, select a tree-based random forest boosting method that can support some readability.
This method can also be expected to be sparse so that the classifier can extract valid variables.




### Result

We conducted three models and obtained the following results. In this case, f1 score is suitable evaluation metric.

|model               | f1 score | accuracy | recall  | Precision |
|--------------------|----------|----------|---------|-----------|
|Logistic Regression | 0.78     | 0.82     | 0.80    | 0.82      |
|GBTClassifier       | 0.87     | 0.87     | 0.86    | 0.87      |
|Random forest       | 0.79     | 0.84     | 0.86    | 0.84      |


## Future considerations

Check the Featue Impottance for a tree-based model.
You can see that the following are variables that contributed to the classification accuracy.

- Standard deviation of songs play per month
- days from registration
- song per session


**GBT**
![](2020-09-05-20-03-49.png)

**Random Forest**
![](2020-09-05-20-08-17.png)




# Conlcusion

A classifier on a distributed platform was created by suppressing the characteristics of users who tend to cancel. Prediction with a linear model is a difficult event, and the result is that a Tree-based classifier is suitable.
We selected important Featurer, but in order to understand the actual market trends, it is necessary to use larger scale data, and we can imagine that the prediction accuracy and features may change. A large-scale distributed infrastructure is indispensable for developing services based on vast amounts of customer data.


In the implementation, machine learning can be implemented in the same procedure as Sklearn, so it was found to be a powerful candidate tool for handling large-scale data.
On the other hand, the widespread use of powerful columnar databases suitable for large-scale data has increased the choices when dealing with large-scale data. It is necessary to understand the merits and demerits of various methods and select the appropriate method.
