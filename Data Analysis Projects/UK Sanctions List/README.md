# Project Objective

This data analysis project focuses on an explorative study of the UK Sanctions List website dataset using Tableau for visualisations. 

# The UK Sanctions List

The UK Sanctions List website provides details of individuals, entities, and ships designated under regulations made under the Sanctions and Anti-Money Laundering Act 2018. it indicates which sanctions measures apply to these designated persons or ships. The details of the UK Sanctions List are free to use in the public domain and can be found [here](https://search-uk-sanctions-list.service.gov.uk/)

## Dataset

The dataset contains the following columns:-
- **Unique ID** - A unique identifier for each entry in the sanctions list. This is likely a primary key used to distinguish each sanctioned individual or entity.
- **OFSI Group ID** - A numerical identifier that groups related individuals or entities. This could indicate organizational affiliations or relationships within a sanctioned network.
- **Name** - The name of the sanctioned individual or entity.
- **Regime Name** - The name of the sanctions regime under which the individual or entity is sanctioned (e.g., Afghanistan, ISIL (Da'esh) and Al-Qaida, Belarus).
- **Type** - Indicates whether the sanctioned party is an "Individual," "Entity," or "Ship."
- **Designation Source** - The authority or body that imposed the sanctions (e.g., UN, UK).
- **Date Designated** - The date on which the sanctions were imposed.
- **Sanction 1** - The first type of sanction applied (e.g., Asset freeze).
- **Sanction 2** - : The second type of sanction applied (e.g., Travel Ban).
- **Sanction 3** - The third type of sanction applied.
- **Sanction 4** - The fourth type of sanction applied.
- **Sanction 5** - The fifth type of sanction applied.
- **Sanction 6** - The sixth type of sanction applied.

Please note that the sanctions have been divided into 6 columns in order to separate each one to ease analysis - they were originally all amalgamated together. 

## Research Questions

The following questions will be posed for this dataset, with analysis and context provided for each:-

- How has the number sanctions changed over time?
- What are the most popular sanction party types?
- What is the most popular sanction?
- What are the most popular region names?
- What is the most popular region name for the most popular sanction?

## How has the number sanctions changed over time?

<img width="1062" alt="Sanctions Over Time" src="https://github.com/user-attachments/assets/b6a3c70b-37ae-4e40-8a68-b047880677ad" />


The filled line graph above shows that sanctions have dramatically increased between 2020 to 2025, with highs of 1141 sanctions in 2020 and 1498 sanctions in 2022. One reason for this is efforts to close loopholes - Governments have been working to address ineffective enforcement and close loopholes that allowed sanctioned entities to find workarounds. This has led to new rounds of sanctions and more targeted measures. Another factor is the emergence of new sanctions, including secondary sanctions. As Russia and other sanctioned entities sought ways to evade existing measures, new sanctions were introduced to counter these efforts, including secondary sanctions on third countries and individuals facilitating evasion.

## Who are the most popular sanction party types?

<img width="674" alt="Types of Sanctions" src="https://github.com/user-attachments/assets/783e13de-f52f-44cc-932c-594a9340c9f0" />


The pie chart above shows that individuals account for 75.94% of the total sanctions with Ship accounting for 3.07% of the total sanctions. This may be because of targeting decision-makers: Sanctions primarily focus on individuals who are responsible for or support Russia's actions, including government officials, oligarchs, and business figures with strong ties to the Kremlin. Wide-ranging impact: Sanctioning individuals can affect multiple sectors simultaneously, as many have diverse business interests and influence across various industries. Identifying and sanctioning ships involved in illicit activities can be more challenging and time-consuming than targeting individuals, leading to fewer ship sanctions overall.

## What is the most popular sanction?

<img width="495" alt="Most Popular Sanctions" src="https://github.com/user-attachments/assets/c52a0461-7a0e-4d9f-bc23-03d3c4d7f493" />


The table above shows that the most popular sanctions were asset freezes with 4638 entries, and trust service sanctions with 2115 entries. The least popular sanctions were internet service sanctions with 8 entries and de-flag sanctions with 15 instances. 

Asset freeze sanctions have been very popular as they are widely applicable and effective measure targeting individuals, entities, and financial institutions.They directly impact the financial capabilities of sanctioned parties, limiting their ability to fund activities or access resources. Trust service sanctions are very popular due to their introduction in December 2022 to limit access to UK trust services for persons connected with Russia. They reflect the UK's focus on preventing sanctions evasion and cutting off funding streams to the Putin regime. Internet service sanctions are one of the least popular due to limited application due to potential unintended consequences on ordinary Russian citizens. There is also difficulty in implementation and enforcement across borders with internet service sanctions. De-flag sanctions are less popular as they are highly specific measure targeting ships, which are fewer in number compared to individuals or entities.

## What are the most popular region names?

<img width="878" alt="Most Popular Regimes" src="https://github.com/user-attachments/assets/fcb3c78d-3b14-44bf-ace4-58f79178c719" />


The world cloud above shows that the most popular regime names were Russia, ISIL/AL-Qaida, Syria and Democratic People’s Republic of Korea.
Russia is the most popular due to their Invasion of Ukraine in 2022 led to unprecedented sanctions by the UK, US, EU, and allies. They have ongoing geopolitical tensions with Western nations, and many nations have concerns about their nuclear proliferation and arms control violations. ISIL/Al-Qaida are persistent global threat with terrorist activities. Within Syria there is an ongoing civil war and human rights concerns, with allegations of chemical weapons use and ties to other sanctioned regimes and terrorist groups. The Democratic People's Republic of Korea (North Korea) undertake nuclear weapons programs and ballistic missile tests, with report uman rights abuses and violations of international sanctions and involvement in illicit activities.

## What is the most popular region name for the most popular sanction?

<img width="874" alt="Asset Freeze Sanctions" src="https://github.com/user-attachments/assets/94ffc9bb-0ecb-405b-a4e8-4938cadd79ae" />


The bar chart above shows Russia is the most popular region name and asset freeze being the most popular sanction for that region name. Asset freezes directly impact Russia's financial capabilities, limiting its ability to fund military operations and support its economy. Asset freezes target a wide range of entities, including banks, strategic industries, government officials, and oligarchs. About 70% of the Russian banking system's assets are under sanctions. Asset freezes work in conjunction with other sanctions, such as trade restrictions and travel bans, to create comprehensive pressure on Russia.

## Summary

The number of sanctions has significantly increased from 2020 to 2025, with notable spikes in 2020 (1,141 sanctions) and 2022 (1,498 sanctions). This surge is attributed to efforts to close loopholes in existing sanctions and the introduction of new measures, including secondary sanctions. Governments have been working to address ineffective enforcement and counter evasion attempts by sanctioned entities, leading to new rounds of targeted sanctions. Individuals account for the majority of sanctions at 75.94%, while ships represent only 3.07%. This focus on individuals is due to targeting decision-makers responsible for or supporting Russia's actions, including government officials, oligarchs, and business figures with strong ties to the Kremlin.

Asset freezes are the most popular sanction with 4,638 entries, followed by trust service sanctions with 2,115 entries. Asset freezes are widely applicable and effective in directly impacting the financial capabilities of sanctioned parties. Trust service sanctions, introduced in December 2022, aim to limit access to UK trust services for persons connected with Russia. The most popular region names for sanctions are Russia, ISIL/Al-Qaida, Syria, and the Democratic People's Republic of Korea. Russia is the most prominent due to its invasion of Ukraine in 2022, which led to unprecedented sanctions by Western nations. Asset freezes are particularly prevalent in sanctions against Russia, directly impacting its financial capabilities and limiting its ability to fund military operations and support its economy. Approximately 70% of the Russian banking system's assets are under sanctions.
