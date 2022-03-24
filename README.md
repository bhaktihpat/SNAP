# Help in a SNAP

"SNAP" or "Food Stamps" is a federal nutrition program that provides monetary aid to supplement grocery bills for low income families in the US. In times of economic insecurity, some communities are at greater risk of not having enough food on the table than others. This study examines SNAP applicant data from 2007, 2017, and 2018 in an attempt to categorize these vulnerable groups for targeted additional help during financial uncertainty. Each datasets contains information about households that recieved benefits within these years. We will be looking at Cherry County in Nebraska, and San Juan County in New Mexico. These Counties are of interst due to previous geographical analysis highlighting them as emerging hot or cold spots for SNAP benefits. Cherry County is considered a cold spot and San Juan County may be a hot spot. 
 I will do a comparison between the states of New Mexico and Nebraska using SNAP datasets from 2007 and 2017. Some things of note:
1. The scope of analysis will be at the state level.
2. The 2007 and 2017 datasets will be compared to observe the characteristics of SNAP households before the 2008 housing crash crisis and those households that continued to rely on SNAP 10 years after, during an economic boom. This will give us some insight on who and where vulnerable populations are during the current COVID19 crisis.

# The data science problem
How can we recognize that a community is at risk of food insecurity? I will build a prediction model using community characteristics in order to create a risk assessment strategy for states vulnerable to food insecurity.

# The model
The target variable is `CAT_ELIG`. Which tells us whether someone is eligible for benefits. It was recorded differently in 2007 and 2017. For uniformity, I calculated the column to be:
- 0 = Not Eligible
- 1 = Eligible

I started with almost 800 features and after addressing nulls and performing correlation analysis, I ended up with 32 features, including the target variable.

I wanted to create an interpretive model so I could see the coefficients that are influencing prediction of SNAP receipients. The final models were a Vote Ensemble with Random Forest, Gradient Boost and Bagging Classifiers.  

# Acquiring the data
- The SNAP data was acquired through the USDA and is part of their [SNAP Quality Control Datasets](https://www.fns.usda.gov/resource/snap-quality-control-data). 
- The state shapefiles were acquired through the [US Census Bureau Boundary Shapefiles](https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.html)

Data dictionaries can be found in the reports folder for [2007](http://localhost:8888/lab/tree/reports/07_DataDict.pdf) and [2017](http://localhost:8888/lab/tree/reports/17_DataDict.pdf) datasets. 

# The process
1. First, I broke the data into two states of interest: New Mexico and Nebraska. As well as a 10 year gap analysis by comparing 2007 and 2017 datasets, totaling 4 datasets. My target variable is the field `CAT_ELIG` which is whether someone is eligibile to receive benefits.
2. There was a large amount of null data present in both 2007 and 2017 datasets. In order to clean up the datasets I first adressed all columns with 100% null values, and then dropped the columns with more than 50% nullarity. I then used scikitlearn imputer to subsitute mean values for the dropped null values.
3. After that, I looked at correlations between the datasets. I specifcally compared the remainder of the columns with `CAT_ELIG`using the pandas correlation function. The compared categories are "Unit Demographics", "Unit Countable Income", "Unit Countable Assets", "Unit Expenses and Deductions", "Person-level Characteristics: 1-16", and "Person-level Countable Income: 1-16".
4. I then combined all 32 features, including the target feature into a single dataset to prep it for processing through my model. Once the data was prepped, I created the model and fit the data.
1. I used a Vote Ensemble with Random Forest, Gradient Boost and Bagging Classifier which gave me a CV best accuracy score of 95%.
1. I used sklearn.treeinterpreter to get a list of the most impactful classifiers on model prediction.

# Conclusions
The model correlations concluded that the most important indictor of receiving SNAP benefits is housing security. The top 4 most impactful features all relate to housing allowable housing deductions.  
- FSSLTDED and SHELDED are indicators of how much someone is paying for their home. 
- FSTOTDED & FSSTDDE2 are deductions relating to housing. 

# List of files:
1. Jupyter Files:
    1. [1_EDA](1_EDA.ipynb)
    1. [2_Correlations](2_Correlations.ipynb)
    1. [3_Model](3_Model.ipynb)
    1. [4_Predictions](4_Predictions.ipynb)
    1. [5_Conclusions](5_Conclusions.ipynb)    
1. Folders: 
    1. [Reports (Incl Data Dictionaries and Technical Documents)](https://github.com/bhaktihpat/SNAP/tree/main/reports)
    1. [GIS Analysis Files](https://github.com/bhaktihpat/SNAP/blob/main/QGISBGIS.zip)
    1. [Data](https://github.com/bhaktihpat/SNAP/tree/main/SNAP_Data)
    1. [Python Code](https://github.com/bhaktihpat/SNAP/tree/main/python_code)
    1. [Images](https://github.com/bhaktihpat/SNAP/tree/main/images)
1. [Final Model](https://github.com/bhaktihpat/SNAP/blob/main/final_model.sav)

# Resources
- [SNAP Quality Control Datasets](https://www.fns.usda.gov/resource/snap-quality-control-data)
- [Project mentor GitHub](https://github.com/decca10/Help-in-a-SNAP)

# Author
Bhakti Patelgit 