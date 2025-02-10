## Project Objective

This Data Analyst project focuses on cluster analysis of a dataset relating to Bribery Reports in India between 2015 to 2022, which can be found on this Kaggle link. 

## Bribery

Bribery is the offering, giving, receiving, or soliciting of any item of value to influence the actions of an official or other person in charge of a public or legal duty.

## Bribery In India

India was ranked 86 out of 180 countries in Transparency International’s (TI) Corruption Perceptions Index 2021. TI’s Global Corruption Barometer (GCB) Asia found that India has the highest bribery rate in Asia (39%) and the highest number of people who had to use personal connections to access public services (46%). Although reporting corruption is essential to curb the spread, a majority of those surveyed (63%) feared retaliation if they reported corruption. A large number of people (89%) felt government corruption was a big problem.(1)(https://www.gov.uk/government/publications/overseas-business-risk-india/overseas-business-risk-india#bribery-and-corruption) "Overseas Business Risk - India: Bribery and corruption"

The analysis will delve deeper into the bribery reporting in different Indian states.

## Dataset

This dataset contains information about the bribes taken from public.

**Title** : The title of the complaint posted by citizens.
**Date** : The Date when the complaint was posted.
**Location** : Location where the incident took place.
**Department** : The department which took bribery.
**Views** : Views of posted complaint.
**Amount(INR)** : The Amount taken as bribery in Indian Rupees.

## Research Questions

The following questions will be used as part of this analysis:-

- What are the top 5 states by number of bribery complaints?
- What are the top 5 locations by average views?
- What are the top 5 states by average views?
- What are the top 5 departments by average views?
- What are the top 5 locations by bribe amount range frequency?

## Top 5 states by number of bribery complaints

```python
import pandas as pd
bribery_reports = pd.read_csv('bribery_reporting.csv')
import matplotlib.pyplot as plt
import seaborn as sns

result = bribery_reports['State'].value_counts().reset_index().head(5)
result.columns = ['State', 'ComplaintCount']

plt.figure(figsize=(8, 8))
plt.pie(result['ComplaintCount'], labels=result['State'], autopct='%1.1f%%', startangle=140, colors=['orange', 'lightblue', 'green', 'red', 'purple'])
plt.title('Top 5 States by Number of Complaints')
plt.axis('equal')  
plt.show()
```
![Top 5 States by Number of Complaints](https://github.com/user-attachments/assets/9b31cdbf-f21f-40d0-ba28-2b6633b1a763)

## Top 5 locations by average views

```python
result = bribery_reports.groupby('Exact Location').agg(
    AvgViews=('Views', 'mean')
).reset_index().sort_values('AvgViews', ascending=False).head(5)

plt.figure(figsize=(10, 6))
plt.bar(result['Exact Location'], result['AvgViews'], color='green')
plt.title('Top 5 Locations by Average Views')
plt.xlabel('Exact Location')
plt.ylabel('Average Views')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
![Top 5 Locations by Average Views](https://github.com/user-attachments/assets/d74d129a-82fb-4879-a962-5ec615ab404f)

## Top 5 states by average views

```python
result = bribery_reports.groupby('State').agg(
    AvgViews=('Views', 'mean')
).reset_index().sort_values('AvgViews', ascending=False).head(5)

plt.figure(figsize=(10, 6))
plt.plot(result['State'], result['AvgViews'], marker='o', linestyle='-', color='purple')
plt.title('Top 5 States by Average Views')
plt.xlabel('State')
plt.ylabel('Average Views')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
![Top Five States by Average Views](https://github.com/user-attachments/assets/bac7657e-f6e5-4a08-9b12-4e4392e200ff)

## Top 5 departments by average views

```python
result = bribery_reports.groupby('Department').agg(
    AvgViews=('Views', 'mean')
).reset_index().sort_values('AvgViews', ascending=False).head(5)
plt.figure(figsize=(10, 6))
plt.bar(result['Department'], result['AvgViews'], color='orange')
plt.title('Top 5 Departments by Average Views')
plt.xlabel('Department')
plt.ylabel('Average Views')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
![Top 5 Departments by Average Views](https://github.com/user-attachments/assets/66ed3273-e4d5-4c5c-9cc8-84a0e10d01f0)

## Top 5 locations by bribe amount range frequency

```python
bribery_reports['BribeRange'] = pd.cut(bribery_reports['Amount'], 
    bins=[0, 1000, 5000, float('inf')], 
    labels=['Low', 'Medium', 'High'])
result = bribery_reports.groupby(['Exact Location', 'BribeRange']).size().unstack(fill_value=0)
top_locations = result.sum(axis=1).nlargest(5).index
result = result.loc[top_locations]
plt.figure(figsize=(10, 6))
sns.heatmap(result, annot=True, cmap='Reds', fmt='d')
plt.title('Top 5 Locations by Bribe Amount Range Frequency')
plt.xlabel('Bribe Amount Range')
plt.ylabel('Exact Location')
plt.tight_layout()
plt.show()
```
![Top 5 Locations by Bribe Amount Range Frequency](https://github.com/user-attachments/assets/62f3fefd-986a-4f8a-8b31-03bd0a1e9ca7)
