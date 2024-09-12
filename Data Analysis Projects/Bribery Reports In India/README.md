## Project Objective

This Data Analyst project focuses on cluster analysis of a dataset relating to Bribery Reports in India between 2015 to 2022, which can be found on this Kaggle link. 

## Bribery

Bribery is the offering, giving, receiving, or soliciting of any item of value to influence the actions of an official or other person in charge of a public or legal duty.

## Bribery In India

India was ranked 86 out of 180 countries in Transparency International’s (TI) Corruption Perceptions Index 2021. TI’s Global Corruption Barometer (GCB) Asia found that India has the highest bribery rate in Asia (39%) and the highest number of people who had to use personal connections to access public services (46%). Although reporting corruption is essential to curb the spread, a majority of those surveyed (63%) feared retaliation if they reported corruption. A large number of people (89%) felt government corruption was a big problem.[1] https://www.gov.uk/government/publications/overseas-business-risk-india/overseas-business-risk-india#bribery-and-corruption "Overseas Business Risk - India: Bribery and corruption"

## Research

```python
import pandas as pd
bribery_reports = pd.read_csv('bribery_reporting.csv')
import matplotlib.pyplot as plt
import seaborn as sns

#Query 1
# Assuming bribery_reports is your DataFrame
result = bribery_reports['State'].value_counts().reset_index().head(5)
result.columns = ['State', 'ComplaintCount']

plt.figure(figsize=(8, 8))
plt.pie(result['ComplaintCount'], labels=result['State'], autopct='%1.1f%%', startangle=140, colors=['orange', 'lightblue', 'green', 'red', 'purple'])
plt.title('Top 5 States by Number of Complaints')
plt.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.
plt.show()
```


