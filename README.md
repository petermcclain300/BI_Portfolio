# Business Intelligence Portfolio
Google Business Intelligence Certificate Project – Cyclistic

## Overview
This project was completed as part of the Google Business Intelligence Certificate. The fictional client, Cyclistic, is a bike-share company seeking to understand customer usage patterns to inform station expansion and improve service.

Using a real-world BI workflow, this portfolio demonstrates practical knowledge in:
* Stakeholder analysis
* SQL and BigQuery
* Tableau dashboards
* Executive storytelling

## Step-by-Step Breakdown
***
### Step 1: Stakeholder and Strategy Documentation
Goal: Understand the business problem and stakeholder needs.

Key Deliverables:
* Stakeholder Requirements Document
* Project Requirements Document
* Strategy Document


***
### Step 2: Data Acquisition & SQL in BigQuery
Goal: Explore and manipulate data using SQL

Datasets Used:
* NYC Citi Bike Trips
* NOAA Weather Data
* Census Bureau US Boundaries
* Custom Zip Code CSV (uploaded to BigQuery)

Skills Demonstrated:
* Creating datasets in BigQuery
* Joining multiple data sources
* Writing SQL to generate a clean summary table
* Uploading a custom CSV to BigQuery

<details>
  <summary>Click to expand: SQL Query</summary>
  
  <pre><code class="language-sql">
SELECT
TRI.usertype,
 ZIPSTART.zip_code AS zip_code_start,
 ZIPSTARTNAME.borough borough_start,
 ZIPSTARTNAME.neighborhood AS neighborhood_start,
  ZIPEND.zip_code AS zip_code_end,
  ZIPENDNAME.borough borough_end,
 ZIPENDNAME.neighborhood AS neighborhood_end,
  DATE_ADD(DATE(TRI.starttime), INTERVAL 5 YEAR) AS start_day,
  DATE_ADD(DATE(TRI.stoptime), INTERVAL 5 YEAR) AS stop_day,
  WEA.temp AS day_mean_temperature, -- Mean temp
 WEA.wdsp AS day_mean_wind_speed, -- Mean wind speed
  WEA.prcp day_total_precipitation, -- Total precipitation
 -- Group trips into 10 minute intervals to reduces the number of rows
  ROUND(CAST(TRI.tripduration / 60 AS INT64), -1) AS trip_minutes,
  COUNT(TRI.bikeid) AS trip_count
FROM
 `bigquery-public-data.new_york_citibike.citibike_trips` AS TRI
INNER JOIN
  `bigquery-public-data.geo_us_boundaries.zip_codes` ZIPSTART
 ON ST_WITHIN(
 ST_GEOGPOINT(TRI.start_station_longitude, TRI.start_station_latitude),
ZIPSTART.zip_code_geom)
INNER JOIN
  `bigquery-public-data.geo_us_boundaries.zip_codes` ZIPEND
  ON ST_WITHIN(
ST_GEOGPOINT(TRI.end_station_longitude, TRI.end_station_latitude),
ZIPEND.zip_code_geom)
INNER JOIN
`bigquery-public-data.noaa_gsod.gsod20*` AS WEA
  ON PARSE_DATE("%Y%m%d", CONCAT(WEA.year, WEA.mo, WEA.da)) = DATE(TRI.starttime)
INNER JOIN
  -- Note! Add your zip code table name, enclosed in backticks: `example_table`
  `(insert your table name) zipcodes` AS ZIPSTARTNAME
  ON ZIPSTART.zip_code = CAST(ZIPSTARTNAME.zip AS STRING)
INNER JOIN
  -- Note! Add your zipcode table name, enclosed in backticks: `example_table`
  `(insert your table name) zipcodes` AS ZIPENDNAME
  ON ZIPEND.zip_code = CAST(ZIPENDNAME.zip AS STRING)
WHERE
 -- This takes the weather data from one weather station
 WEA.wban = '94728' -- NEW YORK CENTRAL PARK
 -- Use data from 2014 and 2015
 AND EXTRACT(YEAR FROM DATE(TRI.starttime)) BETWEEN 2014 AND 2015
GROUP BY
 1,
 2,
 3,
 4,
 5,
 6,
 7,
 8,
 9,
 10,
 11,
 12,
 13
  </code></pre>

</details>



***
### Step 3: Data Visualization & Dashboard Design in Tableau
Goal: Create a clear, stakeholder-ready dashboard

Skills Demonstrated:
* Chart selection based on business goals
* Calculated fields (e.g., average trip duration, seasonal filters)
* Use of pages, filters, tooltips, and interactivity
* Strategic dashboard layout decisions

Key Deliverables:
* Trip Totals by Month & Usertype
* Trip Minutes by Zip Code
* Monthly Trends by Neighborhood
* Seasonal Demand Map
* Interactive Table with Filters

<details>
  <summary>Click to view Dashboard</summary>

<img src="https://github.com/user-attachments/assets/31ed35a7-31b9-40c4-b3c0-885e360478ab" alt="Dashboard">

<a href="https://public.tableau.com/views/Activity-BuildadashboardforCyclistic/1stDashboard?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link" target="_blank">
  View Dashboard on Tableau Public
</a>
</details>



***
### Step 4: Executive Summary & Presentation
Goal: Tell a clear story with data

Key Deliverables:
* Executive summary document
* Slide deck presentation highlighting key insights and recommendations

Key Insights:
* September had the highest demand, February the lowest
* Manhattan neighborhoods showed the highest engagement
* Subscribers consistently rode longer than customers
* Weather significantly influenced trip volume


[Slide Deck](https://docs.google.com/presentation/d/155NP8eQ9FG2RyPugbQHnFYgALUqyBaAEe4YMiKr7pR4/edit?usp=sharing)

[Executive Summary](https://docs.google.com/document/d/10As2i2HLyDR0WAEkm28EdJ_Dl_g91f3fp2T1FDOd7j8/edit?usp=sharing)


## Final Skills Demonstrated
* SQL (filtering, joins, aggregations in BigQuery)
* Tableau (charts, calculated fields, filters, dashboards)
* BI documentation (strategy, project scoping, stakeholder needs)
* Executive storytelling (summarizing results into clear business insights)

## Summary

This project showcases a complete BI workflow — from understanding business needs to data storytelling — and reflects my ability to create real-world, actionable solutions using SQL and Tableau.
