# Flight Predictions

Air travel has always been and still is a headache for many travelers. The unknowns of delays and cancellations are some of the biggest contributors to the stress. In this analysis we’ll attempt to shine a light on the unknowns and try to predict the probability of delay and even the delay length of a given future flight. We’ll take a look at 15 years of airline performance data, containing over 75 million flights in a dataset available from US Department of Transportation.

### Prerequisites

Library requirements are located in the requirements.txt file

### Data

Passenger Loading Data:
https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=311

Complete Datasets:
https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236

Weather:
https://mesonet.agron.iastate.edu/ASOS/

Airline Info (automatic download):
https://www.transtats.bts.gov/Download_Lookup.asp?Lookup=L_UNIQUE_CARRIERS

Airport Coordinates:
https://drive.google.com/file/d/1bMVXqd8Tm30RwmYLBgKk8RMr786IuDoK/view?usp=sharing

FAA OPSNET:
https://aspm.faa.gov/opsnet/sys/Delays.asp

## Jupyter Notebooks

There are five notebooks in the "Notebook" folder which contain all code used to generate the report and predictive models. Notebooks are labeled per the documentation in the README file, and can be viewed in the ascending order.

## Cleaning and combining data for analysis

In summary, the following steps were taken:
 - Create a list of files in each year directory via glob method.
 - Loop through each file, unzip, and read into a Pandas Dataframe via pd.read_csv, utilizing the
“compression=’zip’” parameter.
 - Since each year’s file will be ~3GB in memory, convert the datatypes in each column to reduce size. d. Repeat the process for each year by looping through year folders.
 - Concatenate all csv files into one via shell and upload to Google BigQuery.

## Cleaning, combining, and feature engineering data for modeling

 - Extracted and merged passenger loading data
 - Combined and merged weather data
 - Added airport coordinates data
 - Combined FAA data on delays
 - Scraped and merged individual plane information from FAA website

## Modeling

We started with the fastest and easiest implementation of Logistic Regression and Naïve Bayes classifiers. An 80/20 test/train split was utilized initially. We don’t have a concern about not having enough training data as the there are over 2M samples. Additionally, since there is a class imbalance in the dataset, only ~18% of flights tend to be delayed, we want to make sure this is taken into account. The goal is to be able to predict as many delayed flights accurately as possible, but also without sacrificing on-time predictions.



### Boosting SVD predictions

In order to improve SVD predictions we'll use Random Forest Classifier and Regressor algorithms to predict whether a user will like a given restaurant and what rating are they likely to give, respectively. We'll utilize each user's unique review style which was extracted from the LDA Topic modeling to help the algorithm learn users' preferences. 
Once the training is complete we'll fit the entire dataset and combine SVD, RF Classifier, and RF Regressor into a single predicted star value for the given user/restaurant combination.

## Results

Interpreting the final results, we can calculate some key metrics:

 - Recall / Sensitivity: When the flight is actually on-time, how often does our model predict that it is on-time? Recall = 269,557 / 401,109 = 0.67
 - Precision: When the flight is predicted to be on-time, how often is the model predicting it to be on-time correctly? Precision = 269,557 299,732 = 0.90
 - False Positive Rate: When the flight is actually delayed, how often does it predict that it’s not? False Positive Rate = 131,552 / 90,673 = 1.45
 - True Negative Rate: When the flight is actually delayed, how often is the model correctly predicting that the flight is delayed?
 - True Negative Rate = 60,498 / 90,673 = 0.67

We were able to adjust and tune our model to predict ~67% of flights correctly whether it’s delayed or on-time. This was the goal of the project, to maximize prediction rate for both types of flights.



In this analysis we aimed to shine a light on flight delays and predict weather a future flight would be delayed or not. The flight data were acquired from US Department of Transportation. We focused only on 2017 data for this analysis.

There are three major contributors to delayed flights, as we found in the EDA, Figure 14: NAS (mostly weather), Carrier delay (e.g. mechanical delay, flight crew, etc.), and Late Aircraft.
We had data on both, departure delays and arrival delays and we chose to focus on the latter, as this is the most important factor for travelers.

In order to improve our chances of prediction, we added a number of other datasets to the flight data. Passenger Load Factor data was added to account for delays due to more flyers. Weather data was included to account for weather delays. And additional features were engineered to account for airport traffic.
Since we wanted to predict whether a flight was going to be delayed or not, a classification algorithm was chosen. A number of methods were evaluated, and Random Forest was ultimately chosen for the reasons described in the Modeling Section.

Although the model performed relatively well, there a number of improvements that can be made. Adding passenger load factor data on an hourly basis, the intuition is that the predicted delayed flights will increase. The other key contribution to delays is Carrier performance, which can be explored via airline data on maintenance records and even age of aircraft based on the tail number and FAA data. Our model was only trained on 2017 data and can be further improved by introducing more training data. And finally, Late Aircraft delays can be traced using the tail number data and origin / destination information to better predict overall delays.

## Authors

* **Dmitriy Kats** - *Initial work* - 

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
