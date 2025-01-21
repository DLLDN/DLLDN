# Project Objective

For this data analysis project, I have create fictional data tables of information relating to AML transaction monitoring for a fictional bank called XCVBVN Finance. The scenario is that AML and compliance professionals have come to the bank to improve the bank's AML functionality and screening of high risk customers, customer due diligence and uncovering any potential financial crime. 

# Data Tables

The data tables were created in R and have no confidential or real data, and exported to csv format for SQL querying in Big Query. The tables related to the following areas, with column names in italics:-
- **Customers** | *CustomerID*, *RiskScore*, *Country*;
- **Their transactions** | *TransactionID*, *CustomerID*, *Amount*, *TransactionType*, *Country*, *IPAddress*, *DateTime*;
- **The type of account they hold**;
- **Their occupation**;
- **Number of beneficial owners associated with them**;
- **Customers who have had adverse media coverage**;
- **Customers associated with politically exposed persons (PEPs)**;
- **Whether they were able to explain their source of funds**;
- **Whether they had supplied potentially suspicious documents**;
- **Whether there had been any third party involvement**;
- **Potential alerts with any of the customers**.  

The areas covered in the tables relate to real AML concepts and risks and act to simulate areas AML professionals would investigate. Many of the areas relate to [Financial Action Task Force (FATF) recommendation 10](https://www.fatf-gafi.org/content/dam/fatf-gafi/recommendations/FATF%20Recommendations%202012.pdf.coredownload.inline.pdf), which focuses on the need to investigate potential suspicious transactions and scrutinize documentation and information provided by a customer. 
