# Acute Liver Failure Patients Analysis
Acute liver failure is loss of liver function that occurs rapidly, usually for a person who has no pre existing liver disease.The JPAC centre for Health Diagnosis and control has conducted nationwide surveys of Indian adults from 1990. The centre had collected wide variety of health information through different modes. This dataset has information of 8785 adults 20 years of age or older from 2008-2009 and 2014-2015 surveys.

The dataset has been downloaded from Kaggle website. The link is :
https://www.kaggle.com/rahul121/acute-liver-failure

The steps done in this analysis are as follows:
1) Data collection
2) Data analysis
3) Data visualization
4) Data cleaning
5) Algorithm selection
6) Prediction

### Importing libraries:
```
import pandas as pd
import seaborn as sb
from matplotlib.pyplot import scatter as sm
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split as tts
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import confusion_matrix

```

# 1) Data Collection
### Data collection is the process of gathering and measuring information from the dataset.
```
data=pd.read_csv("C:\\Users\\Pavan R Shetty\\Desktop\\Project\\Acute Liver Failure.csv")
```
# 2) Data Analysis
### In this process, the data in the dataset is analyzed and useful information are obtained which makes it easy for decision making.

### a) This displays the entire dataset.
```
data       
```

![](Internship/Data.png)
![](Internship/Data1.png)

### b) This displays the first 5 entries of the dataset.
```
data.head()
```

![](Internship/head.png)
![](Internship/head1.png)

### c) This displays the last 5 entries of the dataset.
```
data.tail()
```

![](Internship/tail.png)
![](Internship/tail1.png)

### d) This shows the statistical information about the data
```
data.describe()
```

![](Internship/describe.png)
![](Internship/describe1.png)

### e) This gives the count of non empty rows of each column.This means there are some null values in the dataset. There are null values in many columns.
```
data.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 8785 entries, 0 to 8784
Data columns (total 30 columns):
Age                       8785 non-null int64
Gender                    8785 non-null object
Region                    8785 non-null object
Weight                    8591 non-null float64
Height                    8594 non-null float64
Body Mass Index           8495 non-null float64
Obesity                   8495 non-null float64
Waist                     8471 non-null float64
Maximum Blood Pressure    8481 non-null float64
Minimum Blood Pressure    8409 non-null float64
Good Cholesterol          8768 non-null float64
Bad Cholesterol           8767 non-null float64
Total Cholesterol         8769 non-null float64
Dyslipidemia              8785 non-null int64
PVD                       8785 non-null int64
Physical Activity         8775 non-null float64
Education                 8765 non-null float64
Unmarried                 8333 non-null float64
Income                    7624 non-null float64
Source of Care            8785 non-null object
PoorVision                8222 non-null float64
Alcohol Consumption       8785 non-null int64
HyperTension              8705 non-null float64
Family  HyperTension      8785 non-null int64
Diabetes                  8783 non-null float64
Family Diabetes           8785 non-null int64
Hepatitis                 8763 non-null float64
Family Hepatitis          8779 non-null float64
Chronic Fatigue           8750 non-null float64
ALF                       6000 non-null float64
dtypes: float64(21), int64(6), object(3)
memory usage: 2.0+ MB
``` 

### f) This shows if there are missing values in the dataframe.
```
data.isnull()
```

![](Internship/isull.png)
![](Internship/isnull1.png)

### g) This shows if there are any missing values in each column of the dataset. True means there are null values in that column and false means there are no missing values.
```
data.isnull().any()

Age                       False
Gender                    False
Region                    False
Weight                     True
Height                     True
Body Mass Index            True
Obesity                    True
Waist                      True
Maximum Blood Pressure     True
Minimum Blood Pressure     True
Good Cholesterol           True
Bad Cholesterol            True
Total Cholesterol          True
Dyslipidemia              False
PVD                       False
Physical Activity          True
Education                  True
Unmarried                  True
Income                     True
Source of Care            False
PoorVision                 True
Alcohol Consumption       False
HyperTension               True
Family  HyperTension      False
Diabetes                   True
Family Diabetes           False
Hepatitis                  True
Family Hepatitis           True
Chronic Fatigue            True
ALF                        True
dtype: bool
```

### h) This gives the count of the null values in each column .Max number of null values is in the target variable column which is ALF.
```
data.isnull().sum()

Age                          0
Gender                       0
Region                       0
Weight                     194
Height                     191
Body Mass Index            290
Obesity                    290
Waist                      314
Maximum Blood Pressure     304
Minimum Blood Pressure     376
Good Cholesterol            17
Bad Cholesterol             18
Total Cholesterol           16
Dyslipidemia                 0
PVD                          0
Physical Activity           10
Education                   20
Unmarried                  452
Income                    1161
Source of Care               0
PoorVision                 563
Alcohol Consumption          0
HyperTension                80
Family  HyperTension         0
Diabetes                     2
Family Diabetes              0
Hepatitis                   22
Family Hepatitis             6
Chronic Fatigue             35
ALF                       2785
dtype: int64

```

### i) Replacing M with 0 and F with 1.
```

classes={'M':0,'F':1}
data.replace(classes,inplace=True)
data.head()

```

![](Internship/secondhead.png)
![](Internship/secondhead1.png)

# 3) Data Visualization
### This is a graphical representation of data which helps the users to understand the relationship between various attributes in the graph.

### a) Bargraph to determine how many people have and do not have acute liver failure.
```
sb.countplot(data['ALF'])
```
![](Internship/ALFNotthere.png)

It is clear that most of the people whose survey was taken had no acute liver failure.

### b) Count of patients having acute liver failure and not having acute liver failure.
```
data['ALF'].value_counts()
0.0    4035
1.0     287
Name: ALF, dtype: int64
```

### c) Bargraph to determine the gender of the patients
```
sb.countplot(data['Gender'])
```

![](Internship/PlotGender.png)

It is clear that slightly more males took part in the survey than females.

### d) Pairplot to show the relation between all the attributes.
```
sb.pairplot(data,hue='ALF',height=7,markers=['o','D'],diag_kind='kde',kind='reg')
```

![](Internship/Pairplot.png)

### e) This displays the correlation between the attributes.
```
data.corr()
```

![](Internship/corr.png)
![](Internship/corr1.png)

### f) Heatmap to display the correlation.
```
plt.subplots(figsize=(20,20))
x=sb.heatmap(data.corr(),annot=True,cmap='coolwarm')  #now it shows correlation between the attributes
plt.show()
```

![](Internship/Heatmap.png)


# 4) Data Cleaning
### In this step, the inaccurate records or the records that are not required are corrected/removed. This helps in better modification of the dataset.

### a) Removing the unwanted attributes

The above heatmap shows the correlation between any 2 independent variables and variables with the target variable. Here, a. gender and height have coeff of 0.67 and are high +ve correlated. b. Weight and body mass index have coeff 0.86 and high +ve correlated. c. Weight and obesity have coeff of 0.66. d. Weight and waist have coeff of 0.87. weight, Body mass index waist and height are highly correlated to eachother e. Max B.P and hypertension have coeff of 0.63 Hence, I remove the columns: 1.Gender as less correlated to ALF than height 2.weight,BMI and Obesity because than thesse 3 Waist is more correlated to the target attribute 3.Max B.P as less correlated to ALF than hypertension.

The following attributes are dropped because they are comparitively less correlated to the target attribute.

```
data.drop('Gender',axis=1,inplace=True)
data.drop('Weight',axis=1,inplace=True)
data.drop('Body Mass Index',axis=1,inplace=True)
data.drop('Obesity',axis=1,inplace=True)
data.drop('Maximum Blood Pressure',axis=1,inplace=True)
```

Along with the above ones, Dyslipidemia is very less correlated to ALF 
```
data.drop('Dyslipidemia',axis=1,inplace=True)
```

### b) This displays the shape of the dataset. This gives the number of rows and columns in the existing  dataset.
```
data.shape()
```
(4322, 22)

# 5) Algorithm Selection
As in this the target variable is 0 or 1, hence its a classification problem and LOGISTIC REGRESSION has to be used.
### a) Creating arrays for the model
```
x=data.iloc[:,:-1].values #all rows and all columns except the target column
y=data.iloc[:,-1].values #all rows and target column
```

### b) Splitting the dataset for training and testing
```
x_train,x_test,y_train,y_test=tts(x,y,test_size=0.25,random_state=92)
```

### c) Creating object for the model
```
logreg=LogisticRegression()  
logreg.fit(x_train,y_train)
loregaccuracy=logreg.score(x_test,y_test)
loregaccuracy*100
```
95.09713228492137

Therefore, the model can accurately predict if a person has Acute liver failure or not with 95% accuracy

# 6) Prediction

### a) Checking the predicted values
```
logregprediction=logreg.predict(x_test)
logregprediction
```
array([0., 0., 0., ..., 0., 0., 0.])

### b) Compaarison between right and wrong values
```
conmat=confusion_matrix(y_test,logregprediction)
conmat
```
array([[1013,   11],
       [  42,   15]], dtype=int64)
