
  

# SQL: Structured Query Language Exercise

  

### Getting Started

1. Go to BigQuery UI https://console.cloud.google.com/bigquery

2. Add in the public data sets.

3. Click the Add Data icon

4. Add any dataset

5.  `bigquery-public-data` should become visible and populate in the BigQuery UI.

3. Add your queries where it says [YOUR QUERY HERE].

4. Make sure you add your query in between the triple tick marks.

---

  

# SQL: Structured Query Language Exercise

  
  

For this section of the exercise we will be using the `bigquery-public-data.new_york_311.311_service_requests` table.

  

1. Write a query that tells us how many rows are in the table.

```

SELECT
  COUNT(*) AS count_rows
FROM
  `bigquery-public-data.new_york_311.311_service_requests`
  
```

  

4. Write a query that tells you how many open cases there are.

```

SELECT
  COUNT(*) AS open_cases
FROM
  `bigquery-public-data.new_york_311.311_service_requests`
WHERE
  status = "Open"
  
```

2. Write a query that returns all of the records for just _your_ zip code. (zipcode column is `incident_zip`)

```

SELECT
  *
FROM
  `bigquery-public-data.new_york_311.311_service_requests`
WHERE
  incident_zip = "11368"

```

  

2. Lets find out what the most common complaint_type there is in NYC. Write a query that counts all the records for each complaint_type. Hint.. you are going to have to do a group by.

```
SELECT
  complaint_type,
  COUNT(*) AS count
FROM
  `bigquery-public-data.new_york_311.311_service_requests`
GROUP BY
  complaint_type
ORDER BY
  count DESC
LIMIT
  1

```

3. For every agency, count how many _DISTINCT_ agency_names each one has. Hint you must use a group by for this as well. Which agency has the most sub-agencies.

```

SELECT
  agency,
  COUNT(DISTINCT(agency_name)) as dist_count
FROM
  `bigquery-public-data.new_york_311.311_service_requests`
GROUP BY
  agency
ORDER BY
  dist_count DESC

```

  

4. What are the 5 agency_names with the most complaints?

```

SELECT
  agency_name,
  COUNT(complaint_type) as count
FROM
  `bigquery-public-data.new_york_311.311_service_requests`
GROUP BY
  agency_name
ORDER BY
  count DESC
LIMIT 5

```

  
  

5. Lets inspect which complaint_type has the most open cases. Write a query that lists and counts all the records for each complaint_type, just for the where the stats is 'Open' and order the results from highest to lowest.

```

SELECT
  complaint_type,
  COUNT(complaint_type) as open_count
FROM
  `bigquery-public-data.new_york_311.311_service_requests`
WHERE
  status = "Open"
GROUP BY
  complaint_type
ORDER BY
  open_count DESC

```

  
  

6. Write a query to check if there are any duplicate values in the unique_key column (hint.. There are two was to do this, one is to use a temporary table for the groupby, then filter for values that have more than one count, or, using just one table but including the `having` function).

```

WITH T AS (
  SELECT
      unique_key,
      COUNT(*) AS count
    FROM
      `bigquery-public-data.new_york_311.311_service_requests`
    GROUP BY
      unique_key
  )
SELECT * FROM T WHERE count > 1

```

  
  

## For this next section, use the `new_york_citibike` datasets.

  

1. Using the `bigquery-public-data.new_york_citibike.citibike_trips` table, who went on more bike trips, self-identified Males or Females?

```

SELECT
  gender,
  COUNT(*) AS count
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips`
GROUP BY
  gender

```

2. What was the average, shortest, and longest bike trip taken in minutes?

```

SELECT
  AVG(tripduration) / 60 AS avg_duration,
  MIN(tripduration) / 60 AS min_duration,
  MAX(tripduration) / 60 AS max_duration
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips`

```

  

3. Write a query that, for every station_name, has the amount of trips that started there and the amount of trips that ended there. (Hint, use two temporary tables, one that counts the amount of starts, the other that counts the number of ends, and then join the two and displays each station name, total starts, total ends and the sum total starts and total ends.)

```

WITH
TABLE_STARTS AS (
SELECT
    start_station_name AS station,
    COUNT(*) AS n_starts
FROM
    `bigquery-public-data.new_york_citibike.citibike_trips`
GROUP BY
    start_station_name),
TABLE_ENDS AS (
SELECT
    end_station_name AS station,
    COUNT(*) AS n_ends
FROM
    `bigquery-public-data.new_york_citibike.citibike_trips`
GROUP BY
    end_station_name)
SELECT
*, (n_starts + n_ends) as total
FROM
TABLE_STARTS
JOIN
TABLE_ENDS
USING
(station)
ORDER BY
  total DESC
  
```

  
  
  

## Political Ads

  

1. Using the `bigquery-public-data.google_political_ads.advertiser_weekly_spend` table, Lets find who (`advertiser_name`) is spending the most on political ads. Using the `advertiser_weekly_spend` table, write a query that lists the advertiser_names that spent the most in usd sort them by the amount the spent highest to lowest.

```

[YOUR ANSWER HERE]

```

  

2. Using the same table as above, How much did the advertiser name `TOM STEYER 2020` spend in total?

```

[YOUR ANSWER HERE]

```

  

3. Using the same table as above, find what week_start_date had the highest spend..?

```

[YOUR ANSWER HERE]

```

  

5.  *SWITCHING TABLES* Using the `bigquery-public-data.google_political_ads.creative_stats` How many ads did the 'TOM STEYER 2020' campaign run?

```

[YOUR ANSWER HERE]

```

  

5. Using the same table as above, write a query that finds which campaign (advertiser_name) ran the most ads.

```

[YOUR ANSWER HERE]

```

  

5. OPTIONAL Extra credit question, using the same table as above, find out how much each advertiser_name spent. This is difficult.

```

[YOUR ANSWER HERE]

```
  
  

### For the next question, use the `census_bureau_usa` tables.

*  `bigquery-public-data.census_bureau_usa.population_by_zip_2000`

*  `bigquery-public-data.census_bureau_usa.population_by_zip_2010`

  

1. Write a query that returns each zipcode and their population for 2000 and 2010. Hint... You are probably going to have to use two temporary tables, then join them.

```

[YOUR ANSWER HERE]

```

  
  

## EXTRA CREDIT, using python for BigQuery

  

Extra credit: For the next section, open up a google colab notebook.

1. Open up this [this Colab notebook](https://colab.research.google.com/drive/1kHdTtuHTPEaMH32GotVum41YVdeyzQ74?usp=sharing).

2. Save a copy of it in your drive.

3. Rename your saved version with your initials.

4. Click the 'Share' button on the top right.

5. Change the permissions so anyone with link can view.

6. Copy the link and paste it right below this line.

7. [THE LINK TO YOUR COLAB NOTEBOOK GOES HERE]

8. Begin working on the questions in that file.
