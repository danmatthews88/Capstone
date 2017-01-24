# Capstone Project

* For this project I studied the dataset [Inpatient Prospective Payment System (IPPS) Provider Summary for the Top 100 Diagnosis-Related Groups (DRG) - FY2011](https://data.cms.gov/Medicare/Inpatient-Prospective-Payment-System-IPPS-Provider/97k6-zzx3)

* I give a summary below, but the full technical writeup including figures can be found in the [HealthCost\_tech\_writeup.ipynb](https://github.com/danmatthews88/Capstone/blob/master/HealthCost_tech_writeup.ipynb) notebook. A more formal write up can be found in the [Capstone Report](https://github.com/danmatthews88/Capstone/blob/master/CapstoneReport.pdf).

* A description of this dataset can be found [here](https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Inpatient2011.html)
    * From the link: "The data provided here include hospital-specific charges for the more than 3,000 U.S. hospitals that receive Medicare Inpatient Prospective Payment System (IPPS) payments for the top 100 most frequently billed discharges, paid under Medicare based on a rate per discharge using the Medicare Severity Diagnosis Related Group (MS-DRG) for Fiscal Year (FY) 2011. These DRGs represent more than 7 million discharges or 60 percent of total Medicare IPPS discharges."
   * This dataset contains data from 3337 providers in all 50 states. Each provider has a Provider ID number, provider name, address and hospital referral region. Each provider has reported billed charges and received payment data for a number of procedure categories, i.e. DRG Definitions. There are 100 different DRG definitions in this dataset across all providers, although most providers do not report data for all 100 DRG definitions.

* For the different DRG definitions, each provider reports:
    * **Total Discharges:** the number of discharges billed by the provider for that DRG.
    * **Average Covered Charges:** The average charges billed to Medicare by the provider.
    * **Average Total Payments:** The total payments made to the provider (including payments from Medicare as well as the co-payments and deductibles paid by the beneficiary)
    * **Average Medicare Payments:** The average payment just from Medicare.
    
* This analysis contains four major parts. More detail on each part can be found in the CapstoneSummary document, but I will summarize them here:
    * **Data Cleaning: Making the data usable for analysis**
      * This was mainly small things like renaming columns and changing dollar amounts into floats that can be manipulated mathematically.
      * Major issue: City names longer than 15 characters were cut off at 15 in this dataset. This was corrected to improve geocoding results in the next section.
    * **Geocoding: Finding the GPS Coordinates of each provider**
      * Using the GoogleMaps API I found the GPS coordinates for every provider in the IPPS dataset.
      * Major issue: I found when querying the GoogleMaps API, in some cases including the name of the provider improved the geocoding results and in other cases including the name gave worse results. I ended up querying the API using both cases and compared them to improve the overall results, the details of which I describe in the notebook.
    * **Exploratory Data Analysis**
      * This is to get a general idea of the kinds of numbers in the dataset, such as the number of rows, the number of providers that report data for a given DRG, etc.
      * For each charge and payment data I also calculate the national median values as well as the fractional difference of each value compared to this national median.
      * For the average total charges I look at which providers have more DRGs with charges above the national median and which have more below, and I plot the GPS locations of providers that fall into each of these categories. Providers with more DRGs above the national median tend to be tightly clustered around larger cities as you might expect.
    * **Machine Learning: Random Forest Classification**
      * I use a random forest classifier to try and predict whether or not the average billed cost of a given DRG definition at a given provider will be above or below the national median. I split the data into the test set and training set and use the training set to train the model and then test the model by making predictions on the test set. The predictions are compared to the actual data to see how well the model performs.