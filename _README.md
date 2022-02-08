{
 "cells": [
  {
   "cell_type": "markdown",
   "id": "eeb9d01b-c367-45d9-843f-3e3929513db6",
   "metadata": {},
   "source": [
    "# Help in a SNAP"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "937e6cf8-3101-40c5-a8b9-1850f032feed",
   "metadata": {},
   "source": [
    "Many families depend on benefits like SNAP (previously known as \"Food Stamps\") to fully feed or supplement their grocery bills. During times of economic insecurity certain communities are at greater risk. And these communities are less likely to bounce back from economic turbulence. This study attempts to categorize these vulnerable groups for targeted additional help during financial uncertainty. A previous geographical analysis determined a merging hotspot of San Juan County, NM for increased SNAP benefits. And an emerging cold spot of Cherry County, NE for decreased SNAP benefits. I will do a comparison between the states of New Mexico and Nebraska using SNAP datasets from 2007 and 2017. Some things of note:\n",
    "\n",
    "The broad scope for the SNAP analysis will be at the state level since that is the lowest granularity that I can go with SNAP datasets.\n",
    "\n",
    "2007 and 2017 will be compared to see the characteristics of SNAP households before the 2008 crisis and those that continued to rely on SNAP 10 years after the housing crash, during an economic boom. This will give us some insight on vulnerable populations during the COVID19 current crisis.\n"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "7dee14ea-8cf3-47b7-bb4f-f2d41fff85c8",
   "metadata": {},
   "source": [
    "## The data science problem"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "208ffb35-0929-4567-a045-90351621d6db",
   "metadata": {},
   "source": [
    "What do food insecurity community characteristics look like? I will put these into a model to predict communities at risk. This will give me a risk assessment strategy for states vulnerable to food insecurity."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "3de7f5d4-7bab-4579-abe7-5f48d33713ef",
   "metadata": {},
   "source": [
    "## The model"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "fe8c6c96-a28e-48b1-917a-00891949174b",
   "metadata": {},
   "source": [
    "The target variable is CAT_ELIG. It was recorded differently in 2007 and 2017. For uniformity, I calculated the column to be:\n",
    "\n",
    "0 = Not Eligible\n",
    "1 = Eligible\n",
    "I started with almost 800 features. After addressing nulls and correlation analysis, I ended up with 32 features, including the target variable.\n",
    "\n",
    "My intention was to use an interpretable model so I could see the coefficients that are influencing prediction of SNAP receipients. After testing several models, I landed on a Vote Ensemble with Random Forest, Gradient Boost and Bagging Classifiers."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "8efa2585-a64e-465c-aabe-b11d89c02f33",
   "metadata": {},
   "source": [
    "## Acquiring the data"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "ce4b94a0-ebf6-41ce-9b04-8fb813375d95",
   "metadata": {},
   "source": [
    "The SNAP data was acquired through the USDA and is part of their SNAP Quality Control Datasets.\n",
    "The state shapefiles were acquired through the US Census Bureau Boundary Shapefiles\n",
    "Data dictionaries can be found in the reports folder for 2007 and 2017 datasets. A note on government data, though it was processed by the same company \"Mathmatica\", I ran into data conversion issues. For example, I used an online converter to turn the 07_DataDict.pdf into a CSV file, which ran great. But, when I tried the same method on the 17DataDict.pdf there were different formatting issues (ie. unusual spaces) that caused the conversion to fail."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "b8e109f9-2b57-42dd-b31c-909e82f03f3f",
   "metadata": {},
   "source": [
    "## The process"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "5b846547-f944-4fa2-8b33-6f8c61ba42ae",
   "metadata": {},
   "source": [
    "First, I broke the data into two states of interest: New Mexico and Nebraska. As well as a 10 year gap analysis by comparing 2007 and 2017 datasets, totaling 4 datasets. My target variable is the field CAT_ELIG which is whether someone is eligibile to receive benefits.\n",
    "Next, I needed to address the large amount of null data present in both 2007 and 2017 datasets.\n",
    "After that, I looked at correlations between sets of category defined in the technical documents as it related to CAT_ELIG. Those categories are \"Unit Demographics\", \"Unit Countable Income\", \"Unit Countable Assets\", \"Unit Expenses and Deductions\", \"Person-level Characteristics: 1-16\", and \"Person-level Countable Income: 1-16\".\n",
    "I then combined my final 31 features + 1 target feature, across all 4 datasets into a single, combined dataset for processing through my model.\n",
    "I used a Vote Ensemble with Random Forest, Gradient Boost and Bagging Classifier which gave me a CV best accuracy score of 95%.\n",
    "I used sklearn.treeinterpreter to get a list of the most impactful classifiers on model prediction."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "597e718e-22a4-48ba-96db-9c0cf4d4514e",
   "metadata": {},
   "source": [
    "## Conclusions"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "282502ac-98a0-481f-a484-d14671241eef",
   "metadata": {},
   "source": [
    "The model correlations concluded that the most important indictor of receiving SNAP benefits is housing security. The top 4 most impactful features all relate to housing allowable housing deductions.\n",
    "\n",
    "FSSLTDED and SHELDED are indicators of how much someone is paying for their home.\n",
    "FSTOTDED & FSSTDDE2 are deductions relating to housing."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "c56d6b61-b12a-49f4-9907-b0ac69f51fc8",
   "metadata": {},
   "source": [
    "## Next steps"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "e268e9d7-ddda-4662-9417-48b1a6afe2bf",
   "metadata": {},
   "source": [
    "A geographic analysis that consists of access to housing resources(HUD GIS), food pantries and counties showing high levels of SNAP dependency. This would pinpoint areas where local governments or charitable organizations could present the most assistance to communities in need. Especially during COVID, targeting high need areas would be good way to direct tight resources.\n",
    "Post this to an interactive dashboard such as Leaflet.json."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "656ec967-ecd0-4e5d-99fb-9d34cb958222",
   "metadata": {},
   "source": [
    "## List of files:"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "399c96bc-262b-4970-a146-495634a5c97b",
   "metadata": {},
   "source": [
    "## Resources"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "7d69245c-3423-43f7-8be4-5adfbfb90a52",
   "metadata": {},
   "source": [
    "## Author"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "516ff01a-61be-4287-9224-830b20972db5",
   "metadata": {},
   "source": [
    "Bhakti Patel"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "fdc0319e-34c2-461b-96b6-0880d068478a",
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.8.8"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
