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

The following R query finds all the potentially laundering transactions from the S-AML dataset. This dataset will be called suspicious:-
```r
suspicious <- SAML_D[SAML_D$Is_laundering == TRUE, ]
```

The query returned 9,873 entries which were potentially suspicious. This dataset will be the focus of the analysis and subsequent queries.

## Total transaction amounts for laundering transactions over time

The following query creates an line chart in R showing the total transaction amounts for laundering transactions over time:- 
```r
library(ggplot2)

suspicious$Date <- as.Date(suspicious$Date)

daily_transactions <- suspicious %>%
  group_by(Date) %>%
  summarise(TotalAmount = sum(Amount))

ggplot(daily_transactions, aes(x = Date, y = TotalAmount)) +
  geom_line(color = "steelblue") +
  scale_y_continuous(labels = scales::dollar_format()) +
  theme_minimal() +
  labs(title = "Total Transaction Amounts Over Time",
       x = "Date",
       y = "Total Transaction Amount")
```
![Total Transaction Amounts Over Time](https://github.com/user-attachments/assets/2426d594-72d0-4fae-9a72-9535912aa69e)

```r
library(ggplot2)
library(ggridges)

ggplot(suspicious, aes(x = Amount, y = Laundering_type, fill = Laundering_type)) +
  geom_density_ridges(scale = 3, alpha = 0.7) +
  scale_fill_viridis_d() +
  labs(title = "Distribution of Transaction Amounts by Laundering Type",
       x = "Amount",
       y = "Laundering Type") +
  theme_ridges() +
  theme(legend.position = "none") +
  scale_x_log10(labels = scales::dollar_format())
```

![Distribution Of Transaction Amounts Laundering Type](https://github.com/user-attachments/assets/766320f4-cb38-4e61-8eb0-f098615a0885)

```r
library(ggplot2)

ggplot(type_counts, aes(x = reorder(Laundering_type, -N), y = N)) +
  geom_bar(stat = "identity") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  labs(x = "Laundering Type", y = "Count", title = "Frequency of Laundering Types")
```

![Frequency Of Different Laundering Types](https://github.com/user-attachments/assets/e24b4b4a-cfad-4a78-bc51-2cf13f0145e7)

```r
library(ggplot2)
library(dplyr)

ggplot(suspicious, aes(x = Payment_type, y = Amount, fill = Payment_type)) +
  geom_violin(trim = FALSE) +
  geom_boxplot(width = 0.1, fill = "white", color = "black", alpha = 0.5) +
  theme_minimal() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1),
    legend.position = "none"
  ) +
  labs(
    title = "Distribution of Transaction Amounts by Payment Type",
    x = "Payment Type",
    y = "Amount"
  ) +
  scale_y_log10(labels = scales::dollar_format()) +
  scale_fill_brewer(palette = "Set3")
```

![Distribution Of Transaction Amounts Payment Type](https://github.com/user-attachments/assets/3a8d49b3-6662-4004-9c65-1a507e35a9d4)



