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

| School_ID | NAME_OF_SCHOOL | Elementary, Middle, or High School | Street_Address | City | State | ZIP_Code | Phone_Number | Link | Network_Manager | Collaborative_Name | Adequate_Yearly_Progress_Made_ | Track_Schedule | CPS_Performance_Policy_Status | CPS_Performance_Policy_Level | HEALTHY_SCHOOL_CERTIFIED | Safety_Icon | SAFETY_SCORE | Family_Involvement_Icon | Family_Involvement_Score | Environment_Icon | Environment_Score | Instruction_Icon | Instruction_Score | Leaders_Icon | Leaders_Score | Teachers_Icon | Teachers_Score | Parent_Engagement_Icon | Parent_Engagement_Score | Parent_Environment_Icon | Parent_Environment_Score | AVERAGE_STUDENT_ATTENDANCE | Rate_of_Misconducts__per_100_students_ | Average_Teacher_Attendance | Individualized_Education_Program_Compliance_Rate | Pk_2_Literacy__ | Pk_2_Math__ | Gr3_5_Grade_Level_Math__ | Gr3_5_Grade_Level_Read__ | Gr3_5_Keep_Pace_Read__ | Gr3_5_Keep_Pace_Math__ | Gr6_8_Grade_Level_Math__ | Gr6_8_Grade_Level_Read__ | Gr6_8_Keep_Pace_Math_ | Gr6_8_Keep_Pace_Read__ | Gr_8_Explore_Math__ | Gr_8_Explore_Read__ | ISAT_Exceeding_Math__ | ISAT_Exceeding_Reading__ | ISAT_Value_Add_Math | ISAT_Value_Add_Read | ISAT_Value_Add_Color_Math | ISAT_Value_Add_Color_Read | Students_Taking__Algebra__ | Students_Passing__Algebra__ | 9th Grade EXPLORE (2009) | 9th Grade EXPLORE (2010) | 10th Grade PLAN (2009) | 10th Grade PLAN (2010) | Net_Change_EXPLORE_and_PLAN | 11th Grade Average ACT (2011) | Net_Change_PLAN_and_ACT | College_Eligibility__ | Graduation_Rate__ | College_Enrollment_Rate__ | COLLEGE_ENROLLMENT | General_Services_Route | Freshman_on_Track_Rate__ | X_COORDINATE | Y_COORDINATE | Latitude | Longitude | COMMUNITY_AREA_NUMBER | COMMUNITY_AREA_NAME | Ward | Police_District | Location |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 610038 | Abraham Lincoln Elementary School | ES | 615 W Kemper Pl | Chicago | IL | 60614 | (773) 534-5720 | 'http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011Eng_610038.pdf' | Fullerton Elementary Network | NORTH-NORTHWEST SIDE COLLABORATIVE | No | Standard | Not on Probation | Level 1 | Yes | Very Strong | 99.0 | Very Strong | 99 | Strong | 74.0 | Strong | 66.0 | Weak | 65 | Strong | 70 | Strong | 56 | Average | 47 | 96.00% | 2.0 | 96.40% | 95.80% | 80.1 | 43.3 | 89.6 | 84.9 | 60.7 | 62.6 | 81.9 | 85.2 | 52 | 62.4 | 66.3 | 77.9 | 69.7 | 64.4 | 0.2 | 0.9 | Yellow | Green | 67.1 | 54.5 | NDA | NDA | NDA | NDA | NDA | NDA | NDA | NDA | NDA | NDA | 813 | 33 | NDA | 1171699.458 | 1915829.428 | 41.92449696 | -87.64452163 | 7 | LINCOLN PARK | 43 | 18 | (41.92449696, -87.64452163) |
| 610281 | Adam Clayton Powell Paideia Community Academy Elementary School | ES | 7511 S South Shore Dr | Chicago | IL | 60649 | (773) 535-6650 | 'http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011Eng_610281.pdf' | Skyway Elementary Network | SOUTH SIDE COLLABORATIVE | No | Track_E | Not on Probation | Level 1 | No | Average | 54.0 | Strong | 66 | Strong | 74.0 | Very Strong | 84.0 | Weak | 63 | Strong | 76 | Weak | 46 | Average | 50 | 95.60% | 15.7 | 95.30% | 100.00% | 62.4 | 51.7 | 21.9 | 15.1 | 29 | 42.8 | 38.5 | 27.4 | 44.8 | 42.7 | 14.1 | 34.4 | 16.8 | 16.5 | 0.7 | 1.4 | Green | Green | 17.2 | 27.3 | NDA | NDA | NDA | NDA | NDA | NDA | NDA | NDA | NDA | NDA | 521 | 46 | NDA | 1196129.985 | 1856209.466 | 41.76032435 | -87.55673627 | 43 | SOUTH SHORE | 7 | 4 | (41.76032435, -87.55673627) |

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
