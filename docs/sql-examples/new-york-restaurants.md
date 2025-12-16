# New York Restaurants — SQL Analysis Project

## Introduction

This project demonstrates the use of SQL to explore and analyse a fictional dataset of New York City restaurants. Using a series of structured queries, I examined the data, filtered restaurants based on different criteria, ranked results, and transformed numeric values into meaningful categories.

The aim of this project is to showcase practical SQL skills commonly used in data analysis and real-world problem solving.

---

## Project Overview

The dataset is stored in a table called `nomnom`, which contains information about restaurant names, locations, cuisines, review scores, pricing, and health grades.

Throughout this analysis, I used SQL to:

- Explore the structure of the dataset  
- Identify patterns and categories within the data  
- Filter results based on realistic scenarios  
- Rank and classify restaurants using conditional logic  

---

## Exploring the Data
### Column Names

The dataset includes the following fields:

- 'name'
- 'neighborhood'
- 'cuisine'
- 'review'
- 'price'
- 'health'

---

### Distinct Neighborhoods

To identify which neighborhoods are represented in the dataset, I queried the unique values in the `neighborhood` column:

```sql
SELECT DISTINCT neighborhood
FROM nomnom;
``` 

---

**Neighborhoods identified:**
- Brooklyn
- Midtown
- Chinatown
- Uptown
- Queens
- Downtown

---

### Distinct Cuisine Types

To understand the range of cuisines available, I queried the distinct cuisine values:

```sql
SELECT DISTINCT cuisine
FROM nomnom;
``` 

---

### Cuisine types included:
- Steak
- Korean
- Chinese
- Pizza
- Ethiopian
- Vegetarian
- Italian
- Japanese
- American
- Mediterranean
- Indian
- Soul Food
- Mexican

---

## Filtering Restaurants
### Chinese Takeout Options

To find all restaurants offering Chinese cuisine:

```sql
SELECT *
FROM nomnom
WHERE cuisine = 'Chinese';
``` 

This query filters the dataset using a categorical condition.

---

### Restaurants with High Review Scores

To identify restaurants with review scores of 4 and above:

```sql
SELECT *
FROM nomnom
WHERE review >= 4;
``` 

This demonstrates numeric filtering based on customer ratings.

---

### Italian Restaurants for a Fancy Dinner

To find Italian restaurants priced at exactly three dollar signs:

```sql
SELECT *
FROM nomnom
WHERE cuisine = 'Italian'
  AND price = '$$$';
``` 

To include Italian restaurants priced at at least three dollar signs:

```sql
SELECT *
FROM nomnom
WHERE cuisine = 'Italian'
  AND price LIKE '%$$$%';
``` 

These queries demonstrate the use of logical operators and pattern matching.

---

## Searching with Wildcards

If the name of a restaurant is only partially remembered (for example, containing the word “meatball”), wildcard searching can be used:

```sql
SELECT *
FROM nomnom
WHERE name LIKE '%meatball%';
``` 

The % symbol allows flexible pattern matching within text fields.

---

## Location-Based Queries
### Delivery-Friendly Neighborhoods

To find restaurants located in Midtown, Downtown, or Chinatown:

```sql
SELECT *
FROM nomnom
WHERE neighborhood = 'Midtown'
   OR neighborhood = 'Downtown'
   OR neighborhood = 'Chinatown';
``` 

This query combines multiple conditions using the OR operator.

---

## Handling Missing Data
### Restaurants with Pending Health Grades

Some restaurants do not yet have a recorded health grade. These missing values are represented as NULL.

```sql
SELECT *
FROM nomnom
WHERE health IS NULL;
``` 

This query demonstrates how to identify incomplete data.

---

## Ranking Restaurants
### Top 10 Restaurants by Review Score

To rank restaurants from highest to lowest review score:

```sql
SELECT *
FROM nomnom
ORDER BY review DESC;
```  

To limit the results to the top 10 restaurants:

```sql 
SELECT *
FROM nomnom
ORDER BY review DESC
LIMIT 10;
```  

This approach is commonly used to identify top-performing entries in a dataset.

---

### Transforming Data with CASE Statements

To convert numeric review scores into descriptive categories, I used a CASE statement:

```sql
SELECT name,
CASE
  WHEN review > 4.5 THEN 'Extraordinary'
  WHEN review > 4 THEN 'Excellent'
  WHEN review > 3 THEN 'Good'
  WHEN review > 2 THEN 'Fair'
  ELSE 'Poor'
END AS Review
FROM nomnom;
``` 

This transformation improves readability and makes the data more meaningful to non-technical audiences.

---

## Skills Demonstrated
- SQL querying (SELECT, WHERE)
- Data exploration (DISTINCT)
- Logical operators (AND, OR)
- Pattern matching (LIKE)
- Handling missing values (IS NULL)
- Sorting and ranking (ORDER BY, LIMIT)
- Conditional logic (CASE)


