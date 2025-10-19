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

| School_ID | NAME_OF_SCHOOL | Elementary, Middle, or High School | Street_Address | City | State | ZIP_Code | Phone_Number | Link | Network_Manager |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 610038 | Abraham Lincoln Elementary School | ES | 615 W Kemper Pl | Chicago | IL | 60614 | (773) 534-5720 | http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011/610038.pdf | Full School Network |
| 610281 | Adam Clayton Powell Paideia Community Academy Elementary School | ES | 7511 S South Shore Dr | Chicago | IL | 60649 | (773) 535-6650 | http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011/610281.pdf | SKYWAY ELEMENTARY NETWORK |
| 610185 | Adlai E Stevenson Elementary School | ES | 8010 S Kostner Ave | Chicago | IL | 60652 | (773) 535-2280 | http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011/610185.pdf | Mid-South Elementary Network |
| 609993 | Agassiz Elementary School | ES | 2851 N Seminary Ave | Chicago | IL | 60657 | (773) 534-5725 | http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011/609993.pdf | Full School Network |
| 610213 | Agustin Lara Elementary Academy | ES | 4619 S Wolcott Ave | Chicago | IL | 60609 | (773) 535-4389 | http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011/610213.pdf | Pershing Elementary Network |

### 3. Chicago Crime Data

```sql
SELECT * FROM CHICAGO_CRIME_DATA LIMIT 5;
```

| ID | CASE_NUMBER | DATE | BLOCK | IUCR | PRIMARY_TYPE | DESCRIPTION | LOCATION_DESCRIPTION | ARREST | DOMESTIC |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 10224738 | HY411648 | 2015-09-05 13:30:00 | 043XX S WOOD ST | 0486 | BATTERY | DOMESTIC BATTERY SIMPLE | RESIDENCE | false | true |
| 10224739 | HY411615 | 2015-09-04 11:30:00 | 008XX N CENTRAL AVE | 0870 | THEFT | POCKET-PICKING | CTA BUS | false | false |
| 10224740 | HY411595 | 2015-09-05 12:45:00 | 035XX W BARRY AVE | 2023 | NARCOTICS | POSS: HEROIN(WHITE) | SIDEWALK | true | false |
| 10224741 | HY411611 | 2015-09-05 13:00:00 | 0000X N LARAMIE AVE | 0560 | ASSAULT | SIMPLE | APARTMENT | false | true |
| 10224742 | HY411435 | 2015-09-05 10:55:00 | 082XX S LOOMIS BLVD | 0486 | BATTERY | DOMESTIC BATTERY SIMPLE | RESIDENCE | false | true |

---

## Key Insights & Queries

This section details the analytical questions posed and the SQL queries used to derive answers.

#### 1. Total Number of Crimes Recorded
This query counts the total number of crime records in the `CHICAGO_CRIME_DATA` table to understand the overall volume of reported incidents.

```sql
SELECT COUNT(*) AS TOTAL_CRIMES
FROM CHICAGO_CRIME_DATA;
```

#### 2. Community Areas with a Hardship Index Greater Than 90
This query identifies communities facing extreme hardship by filtering the `CENSUS_DATA` table for areas where the `HARDSHIP_INDEX` is above 90.

```sql
SELECT COMMUNITY_AREA_NAME
FROM CENSUS_DATA
WHERE HARDSHIP_INDEX > 90.0;
```

#### 3. All Crimes Involving Minors
This query filters the `CHICAGO_CRIME_DATA` table for crimes where the description includes "MINOR," helping to isolate incidents related to children.

```sql
SELECT DISTINCT(PRIMARY_TYPE), DESCRIPTION
FROM CHICAGO_CRIME_DATA
WHERE DESCRIPTION LIKE '%MINOR%';
```

#### 4. All Kidnapping Crimes Involving a Child
This query identifies severe crimes against children by filtering for "KIDNAPPING" incidents where the description specifically mentions "CHILD."

```sql
SELECT *
FROM CHICAGO_CRIME_DATA
WHERE PRIMARY_TYPE = 'KIDNAPPING' AND DESCRIPTION LIKE '%CHILD%';
```

#### 5. Crimes Recorded at Schools
This query finds all crimes that occurred at school locations by joining `CHICAGO_CRIME_DATA` with `CHICAGO_PUBLIC_SCHOOLS` and filtering for location descriptions that include "SCHOOL."

```sql
SELECT DISTINCT(CR.PRIMARY_TYPE), CR.LOCATION_DESCRIPTION
FROM CHICAGO_CRIME_DATA AS CR, CHICAGO_PUBLIC_SCHOOLS AS PS
WHERE CR.LOCATION_DESCRIPTION LIKE '%SCHOOL%';
```

#### 6. Average Safety Score for Each Community Area
This query calculates the average safety score for schools within each community area. It uses a Common Table Expression (CTE) to first aggregate school-level data before joining it with census data to retrieve community names.

```sql
WITH COMMUNITY_SAFETY (COMMUNITY_AREA_NUMBER, AVG_SAFETY_SCORE) AS (
    SELECT COMMUNITY_AREA_NUMBER, AVG(SAFETY_SCORE)
    FROM CHICAGO_PUBLIC_SCHOOLS
    GROUP BY COMMUNITY_AREA_NUMBER
)
SELECT CD.COMMUNITY_AREA_NAME, CS.AVG_SAFETY_SCORE
FROM CENSUS_DATA AS CD, COMMUNITY_SAFETY AS CS
WHERE CD.COMMUNITY_AREA_NUMBER = CS.COMMUNITY_AREA_NUMBER
ORDER BY CS.AVG_SAFETY_SCORE;
```

#### 7. Top 5 Community Areas with the Highest Percentage of Households Below Poverty
This query identifies the most economically disadvantaged community areas by sorting the `CENSUS_DATA` table by the `PERCENT_HOUSEHOLDS_BELOW_POVERTY` column in descending order and selecting the top 5.

```sql
SELECT COMMUNITY_AREA_NAME, PERCENT_HOUSEHOLDS_BELOW_POVERTY
FROM CENSUS_DATA
ORDER BY PERCENT_HOUSEHOLDS_BELOW_POVERTY DESC
LIMIT 5;
```

#### 8. Community Area with the Most Reported Crimes
This query identifies the community area with the highest crime rate. It groups the `CHICAGO_CRIME_DATA` by `COMMUNITY_AREA_NUMBER`, counts the crimes in each area, and returns the one with the maximum count.

```sql
SELECT COMMUNITY_AREA_NUMBER, COUNT(COMMUNITY_AREA_NUMBER) AS NUMBER_OF_CRIMES
FROM CHICAGO_CRIME_DATA
GROUP BY COMMUNITY_AREA_NUMBER
ORDER BY NUMBER_OF_CRIMES DESC
LIMIT 1;
```

#### 9. Community Area with the Highest Hardship Index
This query finds the community area that suffers the most overall hardship by identifying the maximum `HARDSHIP_INDEX` value in the `CENSUS_DATA` table.

```sql
SELECT COMMUNITY_AREA_NAME
FROM CENSUS_DATA
WHERE HARDSHIP_INDEX = (SELECT MAX(HARDSHIP_INDEX) FROM CENSUS_DATA);
```

#### 10. Community Area with the Most Crimes and Highest Hardship Index
This query combines insights from the previous two queries to determine if the community with the highest hardship index also has the most reported crimes. It uses a subquery to get the community area number with the most crimes and joins it with the census data to retrieve the community name and hardship index.

```sql
SELECT COMMUNITY_AREA_NAME, HARDSHIP_INDEX,
       (SELECT COUNT(COMMUNITY_AREA_NUMBER)
        FROM CHICAGO_CRIME_DATA
        WHERE COMMUNITY_AREA_NUMBER = 25) AS TOTAL_CRIMES
FROM CENSUS_DATA
WHERE COMMUNITY_AREA_NUMBER = 25;
```
