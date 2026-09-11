# Airline Customer Experience

Analyzing customer experience data to identify the drivers behind declining revenue and recommend improvements for an airline company.

## Table of Contents

- [Overview](#overview)
- [Business Problem](#business-problem)
- [Dataset](#dataset)
- [Tools & Technologies](#tools--technologies)
- [Project Structure](#project-structure)
- [Data Cleaning & Preparation](#data-cleaning--preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis-eda)
- [DAX Measures & Power Query Formulas](#dax-measures--power-query-formulas)
- [Research Questions & Key Findings](#research-questions--key-findings)
- [Dashboard](#dashboard)
- [How to Run This Project](#how-to-run-this-project)
- [Final Recommendations](#final-recommendations)
- [Author & Contact](#author--contact)

## Overview

This project evaluates airline customer experience to understand its link to the company's declining revenue.

## Business Problem

This project aims to:

- Identify the root cause of the decline in revenue
- Analyze whether customer experience is linked to organizational performance
- Investigate which areas of the customer journey are impacting growth

## Dataset

The dataset contains 129,880 unique customer experience records, sourced from the [Airlines Customer Satisfaction dataset on Kaggle](https://www.kaggle.com/datasets/sjleshrac/airlines-customer-satisfaction/data).

## Tools & Technologies

- Excel — data analysis
- Power BI — exploratory analysis and visualizations

## Project Structure

This project consists of the following files:

- `.pbix` — Power BI file containing the data model, DAX measures, and dashboard visuals
- `.xlsx` — Source dataset used for cleaning, preparation, and analysis

## Data Cleaning & Preparation

Data cleaning was performed by identifying duplicate records and addressing missing values through imputation.

## Exploratory Data Analysis (EDA)

The analysis was performed in Power BI, where several calculated columns and measures were created for deeper insights:

- Departure and arrival delay times were converted into minutes and hours for more precise delay analysis.
- Age was classified into ranges to identify issues specific to age groups.
- The average of all questionnaire responses was calculated as an overall experience score and categorized on a satisfaction scale.

## DAX Measures & Power Query Formulas

### DAX Measures

Count of arrival delays for a specific time:
```dax
Count Arrival Delay = COUNTA(airline_passenger_satisfaction[Arrival Delay Time])
```

Count of departure delays for a specific time:
```dax
Count of Departure delay = COUNTA(airline_passenger_satisfaction[Departure Delay time])
```

Percentage of departure delays for a specific time, out of all flights:
```dax
Departure delay % = [Count of Departure delay] / [Total Customers]
```

Percentage of arrival delays for a specific time, out of all flights:
```dax
Arrival delay % = [Count Arrival Delay] / [Total Customers]
```
> Note: this was fixed from the original — it previously referenced `[Count of Departure delay]` by mistake.

Number of female customers:
```dax
Female Customers = COUNTROWS(
    FILTER(
        airline_passenger_satisfaction,
        airline_passenger_satisfaction[Gender] = "Female"
    )
)
```

Number of male customers:
```dax
Male Customer = COUNTROWS(
    FILTER(
        airline_passenger_satisfaction,
        airline_passenger_satisfaction[Gender] = "Male"
    )
)
```

Total number of customers:
```dax
Total Customers = COUNTA(airline_passenger_satisfaction[ID])
```

### Power Query (M) Formulas

Convert departure delay minutes into minutes/hours display:
```powerquery-m
Departure Delay time =
if [Departure Delay] <= 59
then Number.ToText([Departure Delay]) & " Mins"
else Number.ToText(Number.RoundDown([Departure Delay] / 60)) & " Hour"
```

Convert arrival delay minutes into minutes/hours display:
```powerquery-m
Arrival Delay Time =
if [Arrival Delay] <= 59
then Number.ToText([Arrival Delay]) & " Mins"
else Number.ToText(Number.RoundDown([Arrival Delay] / 60)) & " Hour"
```

Classify customers into age groups:
```powerquery-m
Age Classification =
if [Age] <= 18 then "Teens"
else if [Age] <= 30 then "20's"
else if [Age] <= 40 then "30's"
else if [Age] <= 50 then "40's"
else "50 & Above"
```

Average of all service experience scores:
```powerquery-m
Overall Score =
Number.Round(
    List.Average({
        [Departure and Arrival Time Convenience],
        [Ease of Online Booking],
        [#"Check-in Service"],
        [Online Boarding],
        [Gate Location],
        [#"On-board Service"],
        [Seat Comfort],
        [Leg Room Service],
        [Cleanliness],
        [Food and Drink],
        [#"In-flight Service"],
        [#"In-flight Wifi Service"],
        [#"In-flight Entertainment"],
        [Baggage Handling]
    }),
    1
)
```

Classify overall score into a satisfaction category:
```powerquery-m
Customer Experience =
if [Overall Score] >= 4 then "Satisfied"
else if [Overall Score] >= 3 then "Neutral"
else "Dissatisfied"
```

## Research Questions & Key Findings

1. Overall, only 15% of customers are satisfied with their experience.
2. Economy class satisfaction is lower, at 6%, compared to Business class at 24%.
3. By age group, teens report the lowest satisfaction at 7%, followed by customers in their 20s at 10% and 30s at 14%.
4. Most dissatisfied customers are flying for personal travel.
5. The top 5 factors negatively impacting the experience are:
   - In-flight Wi-Fi service — 2.73/5
   - Ease of online booking — 2.76/5
   - Gate location — 2.98/5
   - Departure and arrival time convenience — 3.06/5
   - Food and drink — 3.20/5
6. Even though the factors above are rated lowest, no factor scores above 4, indicating that an action plan is needed to improve scores across the board.
7. There is no major correlation between experience and delay, since delay times are generally low; this factor can be ruled out as a priority for improvement.

## Dashboard

The Power BI dashboard includes:

- Total customer count, broken down by male and female count
- A gender filter to identify any issues specific to one gender
- Satisfaction level (%) shown as a column chart
- Age classification shown as a column chart
- A funnel chart representing travel class
- A heat map showing experience across influencing factors
- Pie charts for type of travel (Personal / Business) and customer type (First-time / Returning)
- Tables showing delay time, flight count, and delay percentage

![Airline Customer Experience Power BI dashboard showing satisfaction breakdown, age classification, delay counts, travel type, and a heat map of experience ratings](images/dashboard.jpeg)

## How to Run This Project

1. Download or clone the project files (`.pbix` and `.xlsx`).
2. Open the `.pbix` file in Power BI Desktop.
3. If prompted, update the data source path to point to the `.xlsx` file on your machine, then refresh the data.
4. Explore the dashboard pages to view the visuals and insights.

## Final Recommendations

1. Economy class has the lowest satisfaction score; further investigation (interviews, surveys, workshops) is recommended to understand the root cause.
2. Address the identified factors above to increase satisfaction scores.
3. For future analysis, all other factors should also be reviewed, as their scores trend toward neutral.

## Author & Contact

- **Name:** [Your Name]
- **Email:** [your.email@example.com]
- **LinkedIn:** [linkedin.com/in/your-profile](https://linkedin.com/in/your-profile)
- **GitHub:** [github.com/your-username](https://github.com/your-username)
