#!/usr/bin/env python
# coding: utf-8

# In[1]:


import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt


# In[2]:


data = pd.read_csv('property.csv')


# In[3]:


data.info()


# In[4]:


data.head()


# In[ ]:


# Q1. For the suburb Altona, it is postulated that a typical property sells for $800,000. Use the data at hand to test this assumption. Is the typical property price really $800,000 or has it increased? Use a significance level of 5%.


# In[5]:


#Sol 1. To test the assumption about the typical property price in Altona, we can perform a one-sample t-test
# The null hypothesis (H0): The mean price of properties in Altona is $800,000
# The alternative hypothesis (H1): The mean price of properties in Altona is not $800,000
# We use a significance level of 5%


# In[13]:


# Filter the dataset for Altona suburb
from scipy.stats import ttest_1samp
altona_data = data[data['Suburb'] == 'Altona']['Price'].dropna()


# In[14]:


# Perform the one-sample t-test against the hypothesized value of $800,000
t_statistic, p_value = ttest_1samp(altona_data, 800000)

t_statistic, p_value


# In[ ]:


Q2. # For the year 2016, is there any difference in prices of properties sold in the summer months vs winter months? Consider months from October till March as winter months and rest as summer months. Use a significance level of 5%.


# In[7]:


#Sol 2. To address this question, we'll first separate the property prices based on the defined summer and winter months for the year 2016.
# Summer months: April - September
# Winter months: October - March (of the next year)


# In[15]:


# Convert 'Date' column to datetime format
data['Date'] = pd.to_datetime(data['Date'])


# In[16]:


# Filter data for the year 2016
data_2016 = data[data['Date'].dt.year == 2016]


# In[17]:


# Define summer and winter months based on the question
summer_months = data_2016[data_2016['Date'].dt.month.isin([4, 5, 6, 7, 8, 9])]['Price'].dropna()
winter_months = data_2016[data_2016['Date'].dt.month.isin([10, 11, 12, 1, 2, 3])]['Price'].dropna()


# In[18]:


# Perform a two-sample t-test to compare the means of the two groups
from scipy.stats import ttest_ind

t_statistic, p_value = ttest_ind(summer_months, winter_months, equal_var=False)

t_statistic, p_value


# In[ ]:


Q3. #For the suburb Abbotsford, what is the probability that out of 10 properties sold, 3 will not have a car parking? Use the column car in the dataset. Round off your answer to 3 decimal places.


# In[ ]:


#Sol 3. To find the probability that out of 10 properties sold in Abbotsford, 3 will not have a car parking,
# we can model this scenario using a binomial distribution. The probability of success (p) in this case
# is the probability of a property not having a car parking in Abbotsford.


# In[19]:


# Filter the dataset for Abbotsford suburb
abbotsford_data = data[data['Suburb'] == 'Abbotsford']['Car'].dropna()


# In[20]:


# Calculate the probability of a property not having a car parking space
p_no_car = (abbotsford_data == 0).mean()


# In[21]:


# Number of trials (n) and number of successes (k) we are interested in
n = 10  # total properties
k = 3   # properties without a car parking


# In[22]:


# Calculate the binomial probability
from scipy.stats import binom

probability = binom.pmf(k, n, p_no_car)

round(probability, 3)


# In[ ]:


Q4. #In the suburb Abbotsford, what are the chances of finding a property with 3 rooms? Round your answer to 3 decimal places.


# In[ ]:


#Sol 4. To find the chances of finding a property with 3 rooms in the suburb Abbotsford,
# we'll calculate the proportion of properties with 3 rooms in Abbotsford.


# In[23]:


# Filter the dataset for Abbotsford suburb
abbotsford_room_data = data[data['Suburb'] == 'Abbotsford']['Rooms']


# In[24]:


# Calculate the probability of a property having 3 rooms
p_3_rooms = (abbotsford_room_data == 3).mean()

round(p_3_rooms, 3)


# In[ ]:


Q5. #In the suburb Abbotsford, what are the chances of finding a property with 2 bathrooms? Round your answer to 3 decimal places.


# In[ ]:


# Sol 5. To find the chances of finding a property with 2 bathrooms in the suburb Abbotsford,
# we'll calculate the proportion of properties with 2 bathrooms in that suburb.


# In[25]:


# Filter the dataset for Abbotsford suburb
abbotsford_bathroom_data = data[data['Suburb'] == 'Abbotsford']['Bathroom']


# In[26]:


# Calculate the probability of a property having 2 bathrooms
p_2_bathrooms = (abbotsford_bathroom_data == 2).mean()

round(p_2_bathrooms, 3)


# In[ ]:
