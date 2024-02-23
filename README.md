# Statistics_for_Decision_Making_Properties-
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
data = pd.read_csv('property.csv')
data.info()
data.head()
# Filter the dataset for Altona suburb
from scipy.stats import ttest_1samp
altona_data = data[data['Suburb'] == 'Altona']['Price'].dropna()
# Perform the one-sample t-test against the hypothesized value of $800,000
t_statistic, p_value = ttest_1samp(altona_data, 800000)

t_statistic, p_value
# Convert 'Date' column to datetime format
data['Date'] = pd.to_datetime(data['Date'])
# Filter data for the year 2016
data_2016 = data[data['Date'].dt.year == 2016]
# Define summer and winter months based on the question
summer_months = data_2016[data_2016['Date'].dt.month.isin([4, 5, 6, 7, 8, 9])]['Price'].dropna()
winter_months = data_2016[data_2016['Date'].dt.month.isin([10, 11, 12, 1, 2, 3])]['Price'].dropna()
# Perform a two-sample t-test to compare the means of the two groups
from scipy.stats import ttest_ind

t_statistic, p_value = ttest_ind(summer_months, winter_months, equal_var=False)

t_statistic, p_value
# Filter the dataset for Abbotsford suburb
abbotsford_data = data[data['Suburb'] == 'Abbotsford']['Car'].dropna()
# Calculate the probability of a property not having a car parking space
p_no_car = (abbotsford_data == 0).mean()
# Number of trials (n) and number of successes (k) we are interested in
n = 10  # total properties
k = 3   # properties without a car parking
# Calculate the binomial probability
from scipy.stats import binom

probability = binom.pmf(k, n, p_no_car)

round(probability, 3)
# Filter the dataset for Abbotsford suburb
abbotsford_room_data = data[data['Suburb'] == 'Abbotsford']['Rooms']
# Calculate the probability of a property having 3 rooms
p_3_rooms = (abbotsford_room_data == 3).mean()

round(p_3_rooms, 3)
# Filter the dataset for Abbotsford suburb
abbotsford_bathroom_data = data[data['Suburb'] == 'Abbotsford']['Bathroom']
# Calculate the probability of a property having 2 bathrooms
p_2_bathrooms = (abbotsford_bathroom_data == 2).mean()

round(p_2_bathrooms, 3)
