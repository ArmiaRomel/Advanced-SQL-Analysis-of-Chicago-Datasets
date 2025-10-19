# Advanced SQL Analysis of Chicago's Datasets

## Overview

This project demonstrates advanced SQL querying and data analysis techniques using three distinct datasets from the city of Chicago:
1.  **Chicago Census Data:** Socioeconomic indicators at the community area level.
2.  **Chicago Public Schools:** Performance and demographic data for schools.
3.  **Chicago Crime Data:** Records of reported crimes.

The analysis was conducted within a Jupyter Notebook environment, leveraging SQL magic functions to query a Db2 database. The primary goal is to answer complex questions by joining these datasets and applying a range of SQL functionalities, including subqueries, common table expressions (CTEs), and aggregate functions.

---

## Database Schema

The analysis relies on three tables, each corresponding to one of the Chicago datasets.

| Table Name | Description |
| :--- | :--- |
| `CENSUS_DATA` | Contains socioeconomic data from the 2008-2012 American Community Survey for Chicago's 77 community areas. Includes metrics like per capita income, hardship index, and population demographics. |
| `CHICAGO_PUBLIC_SCHOOLS` | Includes performance data for Chicago public schools, such as safety scores, average student attendance, and college enrollment rates. |
| `CHICAGO_CRIME_DATA` | Contains records of reported crimes in Chicago from 2001 to the present, including case numbers, crime types, locations, and arrest information. |

---

## Dataset Previews

To provide a glimpse into the structure and content of each table, here are the first 5 rows from each dataset.

### 1. Chicago Census Data

```sql
SELECT * FROM CENSUS_DATA LIMIT 5;
```

| COMMUNITY_AREA_NUMBER | COMMUNITY_AREA_NAME | PERCENT_OF_HOUSING_CROWDED | PERCENT_HOUSEHOLDS_BELOW_POVERTY | PERCENT_AGED_16_UNEMPLOYED | PERCENT_AGED_25_WITHOUT_HIGH_SCHOOL_DIPLOMA | PERCENT_AGED_UNDER_18_OR_OVER_64 | PER_CAPITA_INCOME | HARDSHIP_INDEX |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Rogers Park | 7.7 | 23.6 | 8.7 | 18.2 | 27.5 | 23939 | 39 |
| 2 | West Ridge | 7.8 | 17.2 | 8.8 | 20.8 | 38.5 | 23040 | 46 |
| 3 | Uptown | 3.8 | 24.0 | 8.9 | 11.8 | 22.2 | 35787 | 20 |
| 4 | Lincoln Square | 3.4 | 10.9 | 8.2 | 13.4 | 25.5 | 37524 | 17 |
| 5 | North Center | 0.3 | 7.5 | 5.2 | 4.5 | 26.2 | 57123 | 6 |

### 2. Chicago Public Schools

```sql
SELECT * FROM CHICAGO_PUBLIC_SCHOOLS LIMIT 5;
```

| School_ID | NAME_OF_SCHOOL | Elementary, Middle, or High School | Street_Address | City | State | ZIP_Code | Phone_Number | Safety_Icon | SAFETY_SCORE | AVERAGE_STUDENT_ATTENDANCE | Gr3_5_Grade_Level_Math__ | Gr3_5_Grade_Level_Read__ | Gr6_8_Grade_Level_Math__ | Gr6_8_Grade_Level_Read__ | Graduation_Rate__ | ... |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 610038 | Abraham Lincoln Elementary School | ES | 615 W Kemper Pl | Chicago | IL | 60614 | (773) 534-5720 | Very Strong | 99.0 | 96.00% | 89.6 | 84.9 | 81.9 | 85.2 | NDA | ... |
| 610281 | Adam Clayton Powell Paideia Community Academy Elementary School | ES | 7511 S South Shore Dr | Chicago | IL | 60649 | (773) 535-6650 | Average | 54.0 | 95.60% | 21.9 | 15.1 | 38.5 | 27.4 | NDA | ... |

### 3. Chicago Crime Data

```sql
SELECT * FROM CHICAGO_CRIME_DATA LIMIT 5;
```

| ID | CASE_NUMBER | DATE | BLOCK | IUCR | PRIMARY_TYPE | DESCRIPTION | LOCATION_DESCRIPTION | ARREST | DOMESTIC | BEAT | DISTRICT | WARD | COMMUNITY_AREA_NUMBER | FBICODE | X_COORDINATE | Y_COORDINATE | YEAR | LATITUDE | LONGITUDE | LOCATION |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 3512276 | HK587712 | 2004-08-28 | 047XX S KEDZIE AVE | 890 | THEFT | FROM BUILDING | SMALL RETAIL STORE | 0 | 0 | 911 | 9 | 14.0 | 58.0 | 6 | 1155838.0 | 1873050.0 | 2004 | 41.8074405 | -87.70395585 | (41.8074405, -87.703955849) |
| 3406613 | HK456306 | 2004-06-26 | 009XX N CENTRAL PARK AVE | 820 | THEFT | $500 AND UNDER | OTHER | 0 | 0 | 1112 | 11 | 27.0 | 23.0 | 6 | 1152206.0 | 1906127.0 | 2004 | 41.89827996 | -87.71640551 | (41.898279962, -87.716405505) |
| 8002131 | HT233595 | 2011-04-04 | 043XX S WABASH AVE | 820 | THEFT | $500 AND UNDER | NURSING HOME/RETIREMENT HOME | 0 | 0 | 221 | 2 | 3.0 | 38.0 | 6 | 1177436.0 | 1876313.0 | 2011 | 41.81593313 | -87.62464213 | (41.815933131, -87.624642127) |
| 7903289 | HT133522 | 2010-12-30 | 083XX S KINGSTON AVE | 840 | THEFT | FINANCIAL ID THEFT: OVER $300 | RESIDENCE | 0 | 0 | 423 | 4 | 7.0 | 46.0 | 6 | 1194622.0 | 1850125.0 | 2010 | 41.74366532 | -87.56246276 | (41.743665322, -87.562462756) |
| 10402076 | HZ138551 | 2016-02-02 | 033XX W 66TH ST | 820 | THEFT | $500 AND UNDER | ALLEY | 0 | 0 | 831 | 8 | 15.0 | 66.0 | 6 | 1155240.0 | 1860661.0 | 2016 | 41.7734553 | -87.70648047 | (41.773455295, -87.706480471) |

---

## Key Insights & Queries

This section details the analytical questions posed and the SQL queries used to derive answers.

#### 1. Count Total Crimes
This query counts the total number of rows (records) in the `CHICAGO_CRIME_DATA` table. It is used to get a quick overview of the total volume of reported crimes in the dataset.
```sql
SELECT COUNT(*) FROM CHICAGO_CRIME_DATA;
```

#### 2. Find Low-Income Communities
This query retrieves the name, number, and per capita income for all community areas from the `CENSUS_DATA` table where the per capita income is less than $11,000. This helps identify the most economically challenged communities based on income.
```sql
SELECT COMMUNITY_AREA_NAME, COMMUNITY_AREA_NUMBER, PER_CAPITA_INCOME FROM CENSUS_DATA WHERE PER_CAPITA_INCOME < 11000;
```

#### 3. List All Crimes Involving Minors
This query selects all columns for crime records where the `DESCRIPTION` field contains the word "MINOR". This is used to filter for and analyze all incidents that specifically involve a minor.
```sql
SELECT * FROM CHICAGO_CRIME_DATA WHERE DESCRIPTION LIKE '%MINOR%';
```

#### 4. Isolate Kidnapping Cases
This query retrieves all information for crime incidents where the `PRIMARY_TYPE` is 'KIDNAPPING'. It allows for a focused analysis of this specific, serious crime category.
```sql
SELECT * FROM CHICAGO_CRIME_DATA WHERE PRIMARY_TYPE = 'KIDNAPPING';
```

#### 5. Identify Crime Types Occurring at Schools
This query lists the unique types of crimes (`PRIMARY_TYPE`) that have occurred in locations described as a "SCHOOL". The `DISTINCT` keyword ensures that each crime type is listed only once, providing a summary of what kinds of crimes happen at schools.
```sql
SELECT DISTINCT PRIMARY_TYPE FROM CHICAGO_CRIME_DATA WHERE LOCATION_DESCRIPTION LIKE '%SCHOOL%';
```

#### 6. Calculate Average Safety Score by School Level
This query calculates the average `SAFETY_SCORE` for each category of school (Elementary, Middle, or High School). It groups the rows by the school level and then computes the average score for each group, allowing for a comparison of safety across different school levels.
```sql
SELECT `Elementary, Middle, or High School`, AVG(SAFETY_SCORE) FROM CHICAGO_PUBLIC_SCHOOLS GROUP BY `Elementary, Middle, or High School`;
```

#### 7. Find the Top 5 Communities with the Highest Poverty Rates
This query identifies the top 5 community areas with the highest rates of poverty. It sorts all community areas by their `PERCENT_HOUSEHOLDS_BELOW_POVERTY` in descending order and returns only the top 5 results.
```sql
SELECT COMMUNITY_AREA_NUMBER, COMMUNITY_AREA_NAME, PERCENT_HOUSEHOLDS_BELOW_POVERTY FROM CENSUS_DATA ORDER BY PERCENT_HOUSEHOLDS_BELOW_POVERTY DESC LIMIT 5;
```

#### 8. Determine the Community with the Most Crime
This query finds the community area with the most recorded crime incidents. It groups all crimes by `COMMUNITY_AREA_NUMBER`, counts the number of incidents in each group, sorts the results to find the highest count, and returns the single community area with that highest count.
```sql
SELECT COMMUNITY_AREA_NUMBER, COUNT(*) AS Number_of_incidents FROM CHICAGO_CRIME_DATA GROUP BY COMMUNITY_AREA_NUMBER ORDER BY Number_of_incidents DESC LIMIT 1;
```

#### 9. Find the Community with the Highest Hardship Index
This query identifies the community area with the highest `HARDSHIP_INDEX`. It uses a subquery to first find the maximum hardship index value in the entire `CENSUS_DATA` table and then retrieves the name of the community area that corresponds to that maximum value.
```sql
SELECT COMMUNITY_AREA_NAME FROM CENSUS_DATA WHERE HARDSHIP_INDEX = (SELECT MAX(HARDSHIP_INDEX) FROM CENSUS_DATA);
```

#### 10. Correlate Crime and Community Data
This query is designed to find the name of the community area that has the most reported crimes. It joins the `CENSUS_DATA` and `CHICAGO_CRIME_DATA` tables. The subquery within the `SELECT` statement identifies the `COMMUNITY_AREA_NUMBER` with the highest crime count, and the main query then retrieves the corresponding `COMMUNITY_AREA_NAME` for that number.
```sql
SELECT CD.COMMUNITY_AREA_NAME, ... FROM CHICAGO_CRIME_DATA CCA, CENSUS_DATA CD WHERE CD.COMMUNITY_AREA_NUMBER = CCA.COMMUNITY_AREA_NUMBER;
```
