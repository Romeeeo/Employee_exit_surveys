# Employee Exit Surveys Python Data Analysis Project
Clean and Analyze Employee Exit Surveys with Python
## Introduction
In this case study, I will perform many real-world tasks of a junior data analyst at a fictional company, Cyclistic. In order to answer the key business questions, I will follow the steps of the data analysis process: [Ask](https://github.com/Romeeeo/Employee_exit_surveys/edit/main/README.md#ask), [Prepare](https://github.com/Romeeeo/Employee_exit_surveys/edit/main/README.md#prepare), [Process](https://github.com/Romeeeo/Employee_exit_surveys/edit/main/README.md#process), [Analyze](https://github.com/Romeeeo/Employee_exit_surveys/edit/main/README.md#analyze-and-share), [Share](https://github.com/Romeeeo/Employee_exit_surveys/edit/main/README.md#analyze-and-share), and [Act](https://github.com/Romeeeo/Employee_exit_surveys/edit/main/README.md#act).
### Quick Links
Data Source: In this project we will work with exit surveys from employees of the [Department of Education, Training and Employment (DETE)](https://en.wikipedia.org/wiki/Department_of_Education_and_Training_(Queensland)) and the Technical and Further Education (TAFE) institute in Queensland, Australia. You can find the DETE exit survey data [here](https://data.gov.au/dataset/ds-qld-fe96ff30-d157-4a81-851d-215f2a0fe26d/details?q=exit%20survey)

Jupyter Notebook: [Jupyter Notebook](https://github.com/Romeeeo/Employee_exit_surveys/blob/main/employee_exit_survey.ipynb)


## Background

### Scenario
We will play the role of the Data Analysis and provide insights to our stakeholders. In this project we will work with exit surveys from employees of the [Department of Education, Training and Employment (DETE)](https://en.wikipedia.org/wiki/Department_of_Education_and_Training_(Queensland)) and the Technical and Further Education (TAFE) institute in Queensland, Australia. You can find the DETE exit survey data [here](https://data.gov.au/dataset/ds-qld-fe96ff30-d157-4a81-851d-215f2a0fe26d/details?q=exit%20survey). The original TAFE exit survey data is no longer available. We've made some slight modifications to the original datasets to make them easier to work with, including changing the encoding to UTF-8 (the original ones are encoded using cp1252.)

## Ask
### Business Task
Figure out reason as to why employees are leaving
### Analysis Questions
Two questions that the stakeholders would like to know will guide us:  
1. Are employees who only worked for the institutes for a short period of time resigning due to some kind of dissatisfaction? What about employees who have been there longer?
2. Are younger employees resigning due to some kind of dissatisfaction? What about older employees?

The insights will help keep employees from leaving and helping grow the company satisfaction overall.

## Prepare
### Data Source
For this projcet we will be using employee exit surveys from both the __Department of Education, Trainging and Employment (DETE)__ and the __Tachnical and FUrther Education (TAFE)__. You can find the DETE exit survey data [here](https://data.gov.au/dataset/ds-qld-fe96ff30-d157-4a81-851d-215f2a0fe26d/details?q=exit%20survey). The original TAFE exit survey data is no longer available. We've made some slight modifications to the original datasets to make them easier to work with, including changing the encoding to UTF-8 (the original ones are encoded using cp1252.)
### Data Organization
There are two data files that we will be using. They are both CSV files. One for the DETE data and one for TAFE survey data. They both contain unique data and column types.

## Process
I will be using the Jupyter Notebook with Python and utilizing both Pandas and NumPy for analyzing the data.
### Reading in both CSV files
```
import pandas as pd
import numpy as np

dete_survey = pd.read_csv('Data/dete_survey.csv')
tafe_survey = pd.read_csv('Data/tafe_survey.csv')
```
### Data Exploration
```
dete_survey.head()
```
```
tafe_survey.head()
```
Displays first five rows of data.
```
dete_survey.info()
```
```
tafe_survey.info()
```
Tells us info about the data in the dataset. __#__ of columns, Column __names__, __Non-Null Count__ for each column, and __data types__ for each column
```
tafe_survey.isnull().sum()
```
```
dete_survey.isnull().sum()
```
Tells us how many __Nulls__ each column contain.
We can make the following observations based on the work above:
* The dete_survey dataframe contains 'Not Stated' values that indicate values are missing, but they aren't represented as NaN.
* Both the dete_survey and tafe_survey contain many columns that we don't need to complete our analysis.
* Each dataframe contains many of the same columns, but the column names are different.
* There are multiple columns/answers that indicate an employee resigned because they were dissatisfied.
### Data Cleaning
__Identify Missing Values and Drop Unnecessary Columns__
```
dete_survey = pd.read_csv('Data/dete_survey.csv', na_values='Not Stated')
```
This will Read in the data again, but this time read `Not Stated` values as `NAN`

Here is what the dataset looks like now:
![c1](Images\c1.png)

Remove columns that we don't need for our analysis:
```
dete_survey_updated = dete_survey.drop(dete_survey.columns[28:49], axis=1)
tafe_survey_updated = tafe_survey.drop(tafe_survey.columns[17:66], axis=1)
```
__Clean Column Names__
```
dete_survey_updated.columns = dete_survey_updated.columns.str.lower().str.strip().str.replace(' ', '_')
```
Running this line of code updates all column names to lowercase using `str.lower()`, remove leading and trailing whitespaces `str.strip()`, and replace all spaces with _ using `str.replace(' ', '_')`

We can check the new column names by running:
```
dete_survey_updated.columns
```

Next in order to correlate the data from each dataset we will need to make sure that the column names match. 

To do this we can run the code:
```
mapping = {'Record ID': 'id', 'CESSATION YEAR': 'cease_date', 'Reason for ceasing employment': 'separationtype', 'Gender. What is your Gender?': 'gender', 'CurrentAge. Current Age': 'age',
       'Employment Type. Employment Type': 'employment_status',
       'Classification. Classification': 'position',
       'LengthofServiceOverall. Overall Length of Service at Institute (in years)': 'institute_service',
       'LengthofServiceCurrent. Length of Service at current workplace (in years)': 'role_service'}
```
```
tafe_survey_updated = tafe_survey_updated.rename(mapping, axis = 1)
```

Here are the updates column names for both datasets:

__DETE Survey__
![c2](Images\c2.png)

__TAFE Surevey__
![c3](Images\c3.png)

__Filter the Data__

