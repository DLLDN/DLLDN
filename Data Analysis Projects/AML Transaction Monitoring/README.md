# Project Objective

For this data analysis project, I have created fictional data tables of information relating to AML transaction monitoring for a fictional bank called XCVBVN Finance. The scenario is that AML and compliance professionals have come to the bank to improve the bank's AML functionality and screening of high risk customers, assessing of customer due diligence and uncovering any potential financial crime. 

# Data Tables

The data tables were created in R and have no confidential or real data, and exported to csv format for SQL querying in Big Query. The tables related to the following areas, with column names in italics:-
- **Customers** | *CustomerID*, *RiskScore*, *Country*;
- **Their transactions** | *TransactionID*, *CustomerID*, *Amount*, *TransactionType*, *Country*, *IPAddress*, *DateTime*;
- **The type of account they hold** | *AccountID*, *CustomerID*, *AccountType*, *Balance*, *OpenDate*;
- **Number of beneficial owners associated with them** | *CustomerID*, *NumberOfBeneficialOwners*, *OwnershipStructure*, *JurisdictionsInvolved*, *OwnershipTransparency*, *LastReviewDate*; 
- **Customers who have had adverse media coverage** | *CustomerID*, *NatureOfAdverseMedia*, *MediaSource*, *DateReported*;
- **Customers associated with politically exposed persons (PEPs)** | *CustomerID*, *PEPAssociationType*, *PEPPosition*, *PEPCountry*, *AssociationStartDate*, *LastReviewDate*;
- **Whether they were able to explain their source of funds** | *InconsistencyID*, *AccountID*, *CustomerID*, *Date*, *Amount*, *SourceType*;
- **Whether they had supplied potentially suspicious documents** | DocumentID, CustomerID, CustomerName, DocumentType, IssueDate, *SuspicionType*, *VerificationStatus*, *ReportedToAuthorities*;
- **Whether there had been any third party involvement** | *InvolvementID*, *TransactionID*, *AccountID*, *CustomerID*, *ThirdPartyName*, *RelationshipType*, *JustificationProvided*, *RiskLevel*;
- **Potential alerts with any of the customers** | *AlertID*, *TransactionID*, *AccountID*, *CustomerID*, *AlertType*, *AlertDate*, *Resolved*, *AssignedTo*.  

The areas covered in the tables relate to real AML concepts and risks and act to simulate areas AML professionals would investigate. As you can see, some column attributes such as Customer_ID appear in multiple tables. Many of the areas relate to [Financial Action Task Force (FATF) recommendation 10](https://www.fatf-gafi.org/content/dam/fatf-gafi/recommendations/FATF%20Recommendations%202012.pdf.coredownload.inline.pdf), which focuses on the need to investigate potential suspicious transactions and scrutinize documentation and information provided by a customer. 

# Research Questions

The following research questions will be used for this project:-

How many high-value transactions are there in the dataset?
Which customers have a high number of small transactions (under $1000) and have complex beneficial ownership?
How many customers have suspicious documents, their suspicious documentation and the reason for suspicion?
How many transactions had cryptocurrency and unknown origin as their source of funds?
Which customer did not provide an explanation for their source of funds and their tranasction amount?

# High-Value Transactions

The following query in SQL returns all high-value transactions in the dataset - where the transaction amount is more than $10,000.

```sql
SELECT *
FROM `transaction-monitoring-for-aml.TM.Transactions`
WHERE Amount > 10000
ORDER BY Amount DESC;
```

```sql
SELECT 
  t.Customer_ID,
  COUNT(*) AS TransactionCount,
  ROUND(SUM(t.Amount), 2) AS TotalAmount,
  ROUND(AVG(t.Amount), 2) AS AverageAmount,
  ROUND(MAX(t.Amount), 2) AS LargestTransaction,
  ROUND(MIN(t.Amount), 2) AS SmallestTransaction,
  MIN(t.Date_Time) AS FirstTransactionDate,
  MAX(t.Date_Time) AS LastTransactionDate,
  cbo.Number_Of_Beneficial_Owners,
  cbo.Ownership_Structure
FROM `transaction-monitoring-for-aml.TM.Transactions` t
JOIN `transaction-monitoring-for-aml.TM.Complex_Beneficial_Ownership` cbo
  ON t.Customer_ID = cbo.Customer_ID
WHERE t.Amount BETWEEN 50 AND 999
GROUP BY t.Customer_ID, cbo.Number_Of_Beneficial_Owners, cbo.Ownership_Structure
HAVING COUNT(*) > 1
ORDER BY TransactionCount DESC, TotalAmount DESC;
```

```sql
SELECT 
  Customer_ID,
  COUNT(*) AS SuspiciousDocCount,
  STRING_AGG(DISTINCT Document_Type, ', ') AS DocumentTypes,
  STRING_AGG(DISTINCT Suspicion_Type, ', ') AS SuspicionTypes
FROM `transaction-monitoring-for-aml.TM.Suspicious_Documentation`
GROUP BY Customer_ID
HAVING COUNT(*) >= 3
ORDER BY SuspiciousDocCount DESC
LIMIT 100
```

```sql
SELECT 
  icf.Source_Type,
  COUNT(DISTINCT icf.Customer_ID) as Customer_Count,
  ROUND(AVG(t.Amount), 2) as AvgTransactionAmount,
  ROUND(MAX(t.Amount), 2) as MaxTransactionAmount,
  ROUND(SUM(t.Amount), 2) as TotalTransactionAmount
FROM `transaction-monitoring-for-aml.TM.Inconsistent_Funding` icf
JOIN `transaction-monitoring-for-aml.TM.Transactions` t
  ON icf.Customer_ID = t.Customer_ID
WHERE icf.Source_Type IN ('Cryptocurrency', 'Unknown Origin')
GROUP BY icf.Source_Type;
```

```sql
SELECT 
  icf.Customer_ID,
  icf.Source_Type,
  icf.Amount,
  t.Transaction_ID,
  t.Date_Time
FROM `transaction-monitoring-for-aml.TM.Inconsistent_Funding` icf
JOIN `transaction-monitoring-for-aml.TM.Transactions` t
  ON icf.Customer_ID = t.Customer_ID
WHERE icf.Explanation = 'Not Provided'
ORDER BY icf.Amount DESC
```
