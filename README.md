# Steelers Analytics 2016 Data Visulization
This is my final project for my Data Visualization Class.  For this project we were tasked with the
following:
1. Select a dataset of our choosing (Human Resources)
2. Chop the dataset using pandas or any other method in python
3. Clean the dataset and drop missing values if needed
4. Create two types of visualization minimum
5. Create an excellent readme file
6. Push to Github and Kaggle

## About the Readme
This readme is to be used as a more straight forward walkthrough of the project that was created.
In this readme you will find the following:
1. Steelers Analytics 2016 Data Visualization
2. About the Readme
3. About the Dataset
4. How to Run the Code
5. Importing the Data
6. Cleaning the Data
7. General Data Visualization
8. Summary

## About the Dataset
![picture alt](https://cleardatasports-n99kqxp.netdna-ssl.com/wp-content/uploads/2016/11/NFL.jpg)
For this project I have chosen to use a dataset that includes detailed NFL Play-by-Play from 
2009-2016. The dataset contains all regular season plays from the 2009-2016 NFL seasons. 
The dataset has 356,768 rows and 100 columns.  For the sake of this project I have narrowed it down
to one team and that is the Pittsburgh Steelers.

Unfortunately I was unable to upload the .csv file to github as it was too large.  However, you can
access this dataset by creating an acount on Kaggle and following this link:
https://www.kaggle.com/maxhorowitz/nflplaybyplay2009to2016

## How to Run the Code
To run the code you should use anaconda3 or jupyter notebook with python 3.
You will also need numpy, pandas, matplotlib, and seaborn libraries in python.

## Importing the Data
## Importing the Data
After you have the previously mentioned libraries in python 3, it is now time to start importing the 
libraries and the data.
To import the libraries enter the following code:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib as matplot
import seaborn as sns
%matplotlib inline
```
Note: If any of the libraries fail to load please try reintstalling the libraries in order to 
continue.

Next we are going to import the data and put it into a dataframe called NFL. To do this we run the
following code:
```python
NFL=pd.read_csv("NFL by Play 2009-2016 (v2).csv",low_memory=False)
```

## Cleaning the Data
This dataset is missing some values but throughout this project we will scrub exaclty what we want.
So let's narrow the dataset down so we are looking just at the Steelers.
```python
Steelers = NFL[((NFL["HomeTeam"] == 'PIT') | (NFL["AwayTeam"] == 'PIT')) 
             & (NFL["Season"] == 2016) & (NFL['Touchdown'] == 1)]
grouped = Steelers.groupby(by='Date')
len(grouped)
```

Now that we have all of the Steelers' data we want to look at all offensive plays that lead to a 
touchdown.
```python
offense = Steelers[(Steelers["DefensiveTeam"] != 'PIT')]

Top_Plays = offense.sort_values(by='Yards.Gained',ascending=False)[:48]

Top_Plays['scorer'] = Top_Plays["Rusher"]
Top_Plays['scorer'].fillna(Top_Plays['Receiver'], inplace = True)

Touchdowns = Top_Plays[['PlayType',
          'down',
          'Yards.Gained',
          'Date',
          'qtr',
          'desc',
          'scorer',
          'Rusher',
          'Receiver']]
Touchdowns.head()
```

## General Data Visualization
 Now that the data is how we want it we can finally visualize the data.  To do some general graphs we 
 can enter the following code:
 ```python
 x = sns.countplot(Top_Plays['scorer'])
plt.setp(x.get_xticklabels(), rotation=45)
plt.tight_layout()
plt.show()
```
![picture alt](https://github.com/ainnes84/Steelers_Analytics/blob/master/Images/TD_Scores.png)
As we can see our two leading scorers were Antonio Brown and Levion Bell.
* 4 players were tied for fewest touchdowns scored at 1
* Antonio Brown had the most touchdowns scored at 12.

More Code:
```python
sns.countplot(x="PlayType", data=Top_Plays)
```
![picture alt](https://github.com/ainnes84/Steelers_Analytics/blob/master/Images/Playtype.png)
When looking at the information above, We get three playtypes because one was a punt return for a touchdown. 
We can also see the Steelers scored more on pass plays than they did on the run.

Distribution Plots:
```python
fig, axs = plt.subplots(ncols = 3, figsize=(12,6))
sns.distplot(Top_Plays["down"], ax=axs[0])
sns.distplot(Top_Plays["Yards.Gained"], ax=axs[1])
sns.distplot(Top_Plays["qtr"], ax=axs[2])
plt.tight_layout()
plt.show()
```
![picture alt](https://github.com/ainnes84/Steelers_Analytics/blob/master/Images/distribution.png)

As we can see from the distribution plot above:
* The average down scored on was 2nd down.
* The average yards gained during a scoring play was around 5 yards.
* The average quarter scored in was the 4th quarter.

Rushing TD Leader:
```python
runs = offense[(offense["PlayType"] == 'Run')]
sns.countplot(x="Rusher",data=runs)
```
![picture alt](https://github.com/ainnes84/Steelers_Analytics/blob/master/Images/rusher.png)

Receiving TD Leader:
```python
passes = offense[(offense["PlayType"] == 'Pass')]
x = sns.countplot(x="Receiver",data=passes)
plt.setp(x.get_xticklabels(), rotation=45)
plt.show()
```
![picture alt](https://github.com/ainnes84/Steelers_Analytics/blob/master/Images/receiver.png)

## Summary
Based on the data interpreted in this dataset, we can conclude the following:
* The Steelers had a total of 48 touchdowns scored in 2016
  * Of the 48 only 13 players on the team scored
  * 34 were passing touchdowns
  * 13 were rushing touchdowns
  * 1 was a punt return for a touchdown
* Levion Bell lead the team in rushing touchdowns
  * 7 Total
* Antonio Brown lead the team with receiving touchdowns and most toucdowns scored
  * 12 Total
  
  ### Author
  Andrew Innes
  Contact @ ainnes84@live.com

