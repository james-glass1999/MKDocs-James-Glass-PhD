# Trends in Startups

## How to Analyze Startup Trends Using SQL


This guide walks you step by step through analyzing startup data using SQL. 
By the end, you’ll understand how to explore a dataset, use aggregate functions, group data, and extract meaningful business insights.


---


## What You’ll Be Working With


You are given a SQLite database containing a table called `startups`. 
This table represents a portfolio of well-known startups and includes information such as valuation, funding stage, location, and number of employees.


Your goal is to ask useful questions of the data and answer them using SQL.


---


## Step 1: Explore the Data Structure


Before doing any analysis, you need to understand what the table looks like.


```sql
SELECT *
FROM startups;
```


This query displays all rows and columns, helping you understand what data is available.


---


## Step 2: Count the Number of Startups


```sql
SELECT COUNT(*)
FROM startups;
```


This query counts the total number of startups in the table.


---


## Step 3: Calculate the Total Market Value


```sql
SELECT SUM(valuation)
FROM startups;
```


This calculates the combined valuation of all startups in the dataset.


---


## Step 4: Find the Largest Funding Round


```sql
SELECT MAX(raised)
FROM startups;
```


This returns the highest funding amount raised by any startup.


---


## Step 5: Filter by Startup Stage


```sql
SELECT MAX(raised)
FROM startups
WHERE stage = 'Seed';
```


This limits the analysis to Seed-stage startups only.


---


## Step 6: Identify the Oldest Startup


```sql
SELECT MIN(founded)
FROM startups;
```


This returns the earliest founding year in the dataset.


---


## Step 7: Calculate the Average Valuation


```sql
SELECT AVG(valuation)
FROM startups;
```


This provides a baseline average valuation across all startups.


---


## Step 8: Compare Valuation by Category


```sql
SELECT category, AVG(valuation)
FROM startups
GROUP BY category;
```


This groups startups by category and calculates the average valuation for each.


---


## Step 9: Round Results for Readability


```sql
SELECT category, ROUND(AVG(valuation), 2)
FROM startups
GROUP BY category;
```


This rounds average valuations to two decimal places.


---


## Step 10: Rank Categories by Valuation


```sql
SELECT category, ROUND(AVG(valuation), 2)
FROM startups
GROUP BY 1
ORDER BY 2 DESC;
```


This sorts categories from highest to lowest average valuation.


---


## Step 11: Identify Competitive Markets


```sql
SELECT category, COUNT(*)
FROM startups
GROUP BY category;
```


This shows how many startups exist in each category.


---


## Step 12: Filter Competitive Categories


```sql
SELECT category, COUNT(*)
FROM startups
GROUP BY category
HAVING COUNT(*) > 3
ORDER BY 2 DESC;
```


This keeps only categories with more than three startups.


---


## Step 13: Analyze Startup Size by Location


```sql
SELECT location, AVG(employees)
FROM startups
GROUP BY location;
```


This calculates average startup size by location.


---


## Step 14: Focus on Large Startup Hubs


```sql
SELECT location, AVG(employees)
FROM startups
GROUP BY location
HAVING AVG(employees) > 500;
```


This highlights locations with large average team sizes.


---


## Key Takeaways


- Always explore the data first
- Use aggregate functions to summarize information
- Use GROUP BY to compare categories or locations
- Use HAVING to filter aggregated results
- Order results to highlight trends


This workflow applies to almost any SQL-based data analysis task.

