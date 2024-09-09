# Project Objective

For this data analysis project, I have conducted SQL subqueries using Big Query and created visualisations in Tableau to analyse countries and regions who are potentially high-risk and fall below the average score in regards to Anti-Money Laundering procedures, using the Index of Economic Freedom 2023 as my dataset. The link to the dataset can be found here on [Kaggle](https://www.kaggle.com/datasets/mlippo/freedom-economic-index) 

# Index of Economic Freedom

The Index of Economic Freedom (IEF) is an annual ranking of countries based on their degree of economic freedom. It is published annually by the Heritage Foundation and Wall Street Journal.

# Anti-Money Laundering

Anti-Money Laundering (AML) encompasses laws, regulations, and procedures aimed at preventing the disguise of illegally obtained funds as legitimate income. Its primary objectives are to detect, report, and prevent money laundering activities that conceal the illicit origin, nature, or ownership of funds. Of the 12 indicators in the IEF, 4 strongly relate to AML – Judicial Effectiveness, Government Integrity, Financial Freedom, and Property Rights.

# The Four Indicators

Judicial effectiveness gauges the efficiency and fairness of a legal system in enforcing laws, including those related to anti-money laundering. A strong, independent judiciary is crucial for combating money laundering through fair trials, upholding AML laws, and facilitating asset seizures, thereby effectively prosecuting financial crimes and enforcing AML regulations.

Government integrity assesses corruption levels and transparency in governance, with higher integrity supporting anti-money laundering efforts by reducing official corruption and promoting effective AML regulation enforcement. This factor is essential for creating an environment that discourages money laundering and enhances the effectiveness of AML measures by building trust in government institutions to combat financial crimes.

Financial freedom evaluates banking efficiency and independence from government control, supporting AML efforts by promoting transparency and enabling robust compliance programs in the financial sector. This factor facilitates international cooperation in tracking illicit funds, contributing to a more effective and responsive financial system in combating money laundering.

Property rights measure the strength of a country's legal protection for private ownership, supporting AML efforts by making it harder to conceal illicit assets and enabling authorities to trace and seize criminal proceeds. This factor fosters a legal environment that is less conducive to money laundering activities and more supportive of effective AML measures by promoting financial transparency.

# Research Questions

The following research questions will be used for this project:-

- What is the difference between the overall average score for countries on the IEF and the average score for countries across the four AML indicators?
- What are top 10 high-risk countries for AML and their lowest scoring AML Indicator?
- Which top 10 countries that have a high IEF score but below average AML score, and their lowest scoring AML indicator?
- What regions of the world contain countries who have below Average AML risk and high AML risk?

# Average Scores

The following query finds the average Index of Economic Freedom score across all of the 184 countries in the dataset:-
```sql
SELECT ROUND(AVG(Overall_Score), 2) AS avg_fei_score
FROM `economic-freedom-index.EFI.2024_Data`
WHERE Overall_Score IS NOT NULL;
```
This query resulted in an average score of 58.64, which suggests that many countries are mostly unfree and face significant barriers to economic freedom. 

The next query finds the average score of the four AML indicators combined. For the purpose of this query any country with null values for any of those indicators has been excluded. 
```sql
SELECT ROUND(AVG((Government_Integrity + Judicial_Effectiveness + Financial_Freedom + Property_Rights) / 4), 2) AS avg_aml_score,
       COUNT(*) AS num_countries
FROM `economic-freedom-index.EFI.2024_Data`
WHERE Government_Integrity IS NOT NULL AND Judicial_Effectiveness IS NOT NULL
  AND Financial_Freedom IS NOT NULL AND Property_Rights IS NOT NULL;
```
This query resulted in an average AML score of 49.55 and includes 177 countries who provided all the 4 indicators. This average is below the IEF, with a difference of 9.09. This may suggest that AML procedures in many countries around the world are ineffective. We can explore this further by analysing the high-risk countries.

# High-Risk AML Countries

The query below finds the top 10 high-risk countries in the world based on their AML scores across the four indicators.
```sql
SELECT 
    Country, 
    Government_Integrity, 
    Judicial_Effectiveness, 
    Financial_Freedom, 
    Property_Rights,
    CASE
        WHEN Government_Integrity <= Judicial_Effectiveness 
             AND Government_Integrity <= Financial_Freedom 
             AND Government_Integrity <= Property_Rights 
        THEN 'Low Government Integrity'
        WHEN Judicial_Effectiveness <= Government_Integrity 
             AND Judicial_Effectiveness <= Financial_Freedom 
             AND Judicial_Effectiveness <= Property_Rights 
        THEN 'Low Judicial Effectiveness'
        WHEN Financial_Freedom <= Government_Integrity 
             AND Financial_Freedom <= Judicial_Effectiveness 
             AND Financial_Freedom <= Property_Rights 
        THEN 'Low Financial Freedom'
        WHEN Property_Rights <= Government_Integrity 
             AND Property_Rights <= Judicial_Effectiveness 
             AND Property_Rights <= Financial_Freedom 
        THEN 'Low Property Rights'
        ELSE 'None'
    END AS low_score_indicator
FROM 
    `economic-freedom-index.EFI.2024_Data`
WHERE 
    Government_Integrity IS NOT NULL 
    AND Judicial_Effectiveness IS NOT NULL
    AND Financial_Freedom IS NOT NULL 
    AND Property_Rights IS NOT NULL
    AND (Government_Integrity < 50 
         OR Judicial_Effectiveness < 50 
         OR Financial_Freedom < 50 
         OR Property_Rights < 50)
ORDER BY 
    low_score_indicator
LIMIT 10
```
The bar chart below shows the results of this query:-
<img width="1132" alt="Screenshot 2024-09-08 at 19 51 02" src="https://github.com/user-attachments/assets/f99f0326-4db1-479d-abfd-a778a73921a6">
As you can see North Korea scores very poorly for Government Integrity, Judicial Effectiveness, Financial Freedom and Property Rights, suggesting that there major barriers to Economic Freedom and serious issues with AML regulations in place there. For each of the top 10 countries, the lowest scoring indicator was Financial Freedom.

# High IEF Score and Below Average AML Score

So far we have looked at the highest-risk AML countries, but another query that can be performed is looking at the top 10 countries that have an above average IEF score but below average AML score, with their lowest scoring AML indicator also shown. The query below finds this information and shows the countries with the biggest difference between the two figures:-  
```sql
WITH Averages AS (
    SELECT AVG(Overall_Score) AS avg_efi_score,
           AVG((Government_Integrity + Judicial_Effectiveness + Financial_Freedom + Property_Rights) / 4) AS avg_aml_score
    FROM `economic-freedom-index.EFI.2024_Data`
    WHERE Overall_Score IS NOT NULL AND Government_Integrity IS NOT NULL
      AND Judicial_Effectiveness IS NOT NULL AND Financial_Freedom IS NOT NULL
      AND Property_Rights IS NOT NULL
),
CountryScores AS (
    SELECT country, Overall_Score,
           ROUND((Government_Integrity + Judicial_Effectiveness + Financial_Freedom + Property_Rights) / 4, 2) AS combined_aml_score,
           ROUND(Government_Integrity, 2) AS Government_Integrity,
           ROUND(Judicial_Effectiveness, 2) AS Judicial_Effectiveness,
           ROUND(Financial_Freedom, 2) AS Financial_Freedom,
           ROUND(Property_Rights, 2) AS Property_Rights
    FROM `economic-freedom-index.EFI.2024_Data`
    WHERE Overall_Score IS NOT NULL AND Government_Integrity IS NOT NULL
      AND Judicial_Effectiveness IS NOT NULL AND Financial_Freedom IS NOT NULL
      AND Property_Rights IS NOT NULL
)
SELECT country, Overall_Score, combined_aml_score,
       Government_Integrity, Judicial_Effectiveness, Financial_Freedom, Property_Rights,
       CASE
           WHEN Government_Integrity <= Judicial_Effectiveness AND Government_Integrity <= Financial_Freedom AND Government_Integrity <= Property_Rights THEN 'Government Integrity'
           WHEN Judicial_Effectiveness <= Government_Integrity AND Judicial_Effectiveness <= Financial_Freedom AND Judicial_Effectiveness <= Property_Rights THEN 'Judicial Effectiveness'
           WHEN Financial_Freedom <= Government_Integrity AND Financial_Freedom <= Judicial_Effectiveness AND Financial_Freedom <= Property_Rights THEN 'Financial Freedom'
           ELSE 'Property Rights'
       END AS lowest_indicator,
       LEAST(Government_Integrity, Judicial_Effectiveness, Financial_Freedom, Property_Rights) AS lowest_aml_score
FROM CountryScores, Averages
WHERE Overall_Score > avg_efi_score AND combined_aml_score < avg_aml_score
LIMIT 10;
```
This query has been visualised below:-

<img width="578" alt="Screenshot 2024-09-08 at 21 23 59" src="https://github.com/user-attachments/assets/d2883ba0-52f2-4b3e-9611-93deadecd6f6">

The table shows the lowest scoring indicator for each of the top 10 countries and the score of that indicator. There is a greater variety of indicators which these countries perform particuarly poorly at with each of the four indicators featuring across the table, with Judicial Effectiveness being the most popular lowest-scoring indicator. 

# AML Risk for Regions of the World

The following query calculates the average AML score and counts the number of countries in each region with scores below the average:-
```sql
WITH Averages AS (
    SELECT AVG((Government_Integrity + Judicial_Effectiveness + Financial_Freedom + Property_Rights) / 4) AS avg_aml_score
    FROM `economic-freedom-index.EFI.2024_Data`
    WHERE Government_Integrity IS NOT NULL AND Judicial_Effectiveness IS NOT NULL
      AND Financial_Freedom IS NOT NULL AND Property_Rights IS NOT NULL
),
BelowAverage AS (
    SELECT Region, country,
           (Government_Integrity + Judicial_Effectiveness + Financial_Freedom + Property_Rights) / 4 AS combined_aml_score
    FROM `economic-freedom-index.EFI.2024_Data`
    WHERE Government_Integrity IS NOT NULL AND Judicial_Effectiveness IS NOT NULL
      AND Financial_Freedom IS NOT NULL AND Property_Rights IS NOT NULL
)
SELECT Region, COUNT(country) AS num_countries_below_avg
FROM BelowAverage, Averages
WHERE combined_aml_score < avg_aml_score
GROUP BY Region;
```

The treemap below visualises the results of the query:-

<img width="843" alt="Screenshot 2024-09-08 at 21 18 53" src="https://github.com/user-attachments/assets/19d8d050-a9e6-4da9-9da8-d790346d9637">

As you can see Sub-Saharan Africa has the most countries falling below the AML average with 41, and Europe has the least with 8. 

The following query gives the number of high-risk AML countries that fall below the IEF average and AML average:-
```sql
WITH Averages AS (
    SELECT AVG((Government_Integrity + Judicial_Effectiveness + Financial_Freedom + Property_Rights) / 4) AS avg_aml_score,
           AVG(Overall_Score) AS avg_efi_score
    FROM `economic-freedom-index.EFI.2024_Data`
    WHERE Government_Integrity IS NOT NULL AND Judicial_Effectiveness IS NOT NULL
      AND Financial_Freedom IS NOT NULL AND Property_Rights IS NOT NULL
      AND Overall_Score IS NOT NULL
),
HighRiskCountries AS (
    SELECT country, Region,
           (Government_Integrity + Judicial_Effectiveness + Financial_Freedom + Property_Rights) / 4 AS combined_aml_score,
           Overall_Score
    FROM `economic-freedom-index.EFI.2024_Data`
    WHERE Government_Integrity IS NOT NULL AND Judicial_Effectiveness IS NOT NULL
      AND Financial_Freedom IS NOT NULL AND Property_Rights IS NOT NULL
      AND Overall_Score IS NOT NULL)
SELECT Region, COUNT(*) AS num_high_risk_countries
FROM HighRiskCountries, Averages
WHERE combined_aml_score < avg_aml_score AND Overall_Score < avg_efi_score
GROUP BY Region;
```
In Tableau I have converted the count into a percentage and displayed the results as a pie chart:-

<img width="918" alt="Screenshot 2024-09-08 at 21 26 22" src="https://github.com/user-attachments/assets/3276f68b-d744-4330-86ad-3f96abe3ca8d">

The pie chart shows that Sub-Saharan Africa has the highest percentage with 48.75% and Europe has the least with 5%. 

# Summary and Findings

The analysis performed shows that AML indicators are bringing down the IEF averages for most countries around the world, and this is a concerning issue. 

Financial Freedom poses the biggest barrier for the highest risk AML countries, and may suggest that these countries have poor banking efficiency and that illicit financing is more prevalent. 

Countries that perform well above the IEF average but below the AML average have major shortcomings in Judicial Effectiveness, and suggests regulatory procedures and AML legislation is severely lacking. The array of low scoring indicators amongst these countries is more complex and nuanced. However, as these countries perform good overall on the IEF, it offers scope and encouragement that AML measures can be improved. 

It is evidently clear that Sub-Saharan Africa is a major problem for AML, for both the number of countries performing below the average AML score and the percentage being high-risk. It would a very challenging task to enforce and improve AML measures in that part of the world.  

# Changes to dataset and data license

For the purpose of the analysis I have edited column names in the dataset to assist with querying, and have created aggregated data as part of this analysis. The data license can be found in the folder. 


