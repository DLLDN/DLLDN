# Project Objective

This Data Analyst project focuses on transaction monitoring analysis of suspicious money laundering transactions. The dataset used is a simulated transaction monitoring dataset and can be found at this link 

## Dataset

The dataset contains the following columns:-
- **Time** - The time of the transaction.
- **Date** - The date of the transaction.
- **Sender_account** - The sender account number.
- **Receiver_account** - The receiver account number.
- **Amount** - The amount of the transaction. 
- **Payment_currency** - The payment currency.
- **Received_currency** - The received currency.
- **Sender_bank_location** - The location of the sender.
- **Receiver_bank_location** - The location of the receiver.
- **Payment_type** - The payment type for the transaction.
- **Is_laundering** - Whether or not the transaction was considered as money laundering (1 for yes, 0 for no).
- **Laundering_type** - The type of money laundering suspicion (for example structuring or layering).

## Research Questions

The following questions will be used as part of this analysis using the dataset above:-

- How many transactions are potentially laundering transactions?
- What are the total transaction amounts for these laundering transactions over the time period?
- What is the distribution of transaction amounts by laundering type?
- What is the frequency of different laundering types?
- What is the distribution of laundering transaction amounts by payment type?

## Potential laundering transactions

The following R query finds all the potentially laundering transactions from the S-AML dataset:-





