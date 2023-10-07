# GCP-BigQuery-Project-Exploring-Londons-Travel-Network

Use BigQuery to build a project. Use SQL to analyze a database containing information about Transport for London journeys over 12 years.
This project is based on DataCamp project [Exploring London's Travel Network](https://app.datacamp.com/learn/projects/exploring_londons_travel_network).

You can find my solutions here: https://app.datacamp.com/workspace/w/0b014417-c584-4c69-aa51-e56cdfef8d84
Try to build your project using DataCamp Workspaces. Three SQL cells will be created for you in the Workspace. To access the Google BigQuery database, you will need to select data using the syntax `FROM TFL.JOURNEYS` (ensure you use upper-case).

# Context
London, or as the Romans called it "Londonium"! Home to [over 8.5 million residents](https://app.datacamp.com/workspace/external-link?url=https%3A%2F%2Fwww.ons.gov.uk%2Fpeoplepopulationandcommunity%2Fpopulationandmigration%2Fpopulationestimates%2Fbulletins%2Fpopulationandhouseholdestimatesenglandandwales%2Fcensus2021unroundeddata%23population-and-household-estimates-england-and-wales-data) who speak over [300 languages](https://app.datacamp.com/workspace/external-link?url=https%3A%2F%2Fweb.archive.org%2Fweb%2F20080924084621%2Fhttp%3A%2F%2Fwww.cilt.org.uk%2Ffaqs%2Flangspoken.htm). While the City of London is a little over one square mile (hence its nickname "The Square Mile"), Greater London has grown to encompass 32 boroughs spanning a total area of 606 square miles!

Given the city's roads were originally designed for horse and cart, this area and population growth has required the development of an efficient public transport system! Since the year 2000, this has been through the local government body called **Transport for London**, or *TfL*, which is managed by the London Mayor's office. Their remit covers the London Underground, Overground, Docklands Light Railway (DLR), buses, trams, river services (clipper and [Emirates Airline cable car](https://app.datacamp.com/workspace/external-link?url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FLondon_cable_car)), roads, and even taxis.

The Mayor of London's office make their data available to the public [here](https://app.datacamp.com/workspace/external-link?url=https%3A%2F%2Fdata.london.gov.uk%2Fdataset). In this project, you will work with a slightly modified version of a dataset containing information about public transport journey volume by transport type.

The data has been loaded into a Google BigQuery database called `TFL` with a single table called `JOURNEYS`, including the following data:

![image](https://github.com/janaom/GCP-BigQuery-Project-Exploring-Londons-Travel-Network/assets/83917694/862de861-5d75-41c5-9f4c-399a9bf03e18)


# Project Instructions

Write three SQL queries to answer the following questions:

1. What are the most popular transport types, measured by the total number of journeys? The output should contain two columns, 1) `journey_type` and 2) `total_journeys_millions`, and be sorted by the second column in descending order. Save the query as `most_popular_transport_types`.
2. Which five months and years were the most popular for the Emirates Airline? Return an output containing `month`, `year`, and `journeys_millions`, with the latter rounded to two decimal places and aliased as `rounded_journeys_millions`. Exclude null values and order the results by 1) `rounded_journeys_millions` in descending order and 2) `year` in ascending order, saving the result as `emirates_airline_popularity`.
3. Find the *five years* with the lowest volume of `Underground & DLR` journeys, saving as `least_popular_years_tube`. The results should contain the columns `year`, `journey_type`, and `total_journeys_millions`.

# Solutions

## most_popular_transport_types
```SQL
-- most_popular_transport_types
SELECT journey_type, SUM(journeys_millions) AS total_journeys_millions
FROM TFL.JOURNEYS
GROUP BY journey_type
ORDER BY total_journeys_millions DESC;
```
![image](https://github.com/janaom/GCP-BigQuery-Project-Exploring-Londons-Travel-Network/assets/83917694/057e4490-e257-4280-8453-130405a95078)

In this query:
- `SELECT journey_type` selects the journey_type column.
- `SUM(journeys_millions) AS total_journeys_millions` calculates the total number of journeys (journeys_millions) for each journey_type and aliases the result as total_journeys_millions.
- `FROM TFL.JOURNEYS` specifies the table TFL.JOURNEYS from which the data is being retrieved.
- `GROUP BY journey_type` groups the rows by journey_type, ensuring that each unique journey_type is aggregated separately.
- `ORDER BY total_journeys_millions DESC` sorts the result set in descending order based on the total_journeys_millions column.



## emirates_airline_popularity
```SQL
-- emirates_airline_popularity
SELECT month, year, ROUND(SUM(journeys_millions), 2) AS rounded_journeys_millions
FROM TFL.JOURNEYS
WHERE journey_type = 'Emirates Airline' AND journeys_millions IS NOT NULL
GROUP BY month, year
ORDER BY rounded_journeys_millions DESC, year ASC
LIMIT 5;
```
![image](https://github.com/janaom/GCP-BigQuery-Project-Exploring-Londons-Travel-Network/assets/83917694/eeaaf6a4-7bd3-4728-a045-c8dfb5f9417d)

In this code:
- `SELECT month, year` selects the month and year columns.
- `ROUND(SUM(journeys_millions), 2) AS rounded_journeys_millions` calculates the total number of journeys (journeys_millions) for each month and year, rounds it to two decimal places, and aliases it as rounded_journeys_millions.
- `FROM TFL.JOURNEYS` specifies the table TFL.JOURNEYS from which the data is being retrieved.
- `WHERE journey_type = 'Emirates Airline' AND journeys_millions IS NOT NULL` filters the data to include only rows where the journey_type is 'Emirates Airline' and journeys_millions is not null.
- `GROUP BY month, year` groups the rows by month and year, ensuring that the aggregation is performed for each unique combination of month and year.
- `ORDER BY rounded_journeys_millions DESC, year ASC` orders the result set in descending order based on rounded_journeys_millions and ascending order based on year.
- `LIMIT 5` limits the result to the top 5 rows, as required in the task.



## least_popular_years_tube
```SQL
-- least_popular_years_tube
SELECT year, journey_type, SUM(journeys_millions) AS total_journeys_millions
FROM TFL.JOURNEYS
WHERE journey_type IN ('Underground & DLR')
GROUP BY year, journey_type
ORDER BY total_journeys_millions ASC
LIMIT 5;
```
![image](https://github.com/janaom/GCP-BigQuery-Project-Exploring-Londons-Travel-Network/assets/83917694/cd41dfc4-023c-48c6-a371-2f8367d4c788)

In this code:
- `SELECT year, journey_type` selects the year and journey_type columns.
- `SUM(journeys_millions) AS total_journeys_millions` calculates the total number of journeys (journeys_millions) for each year and journey type and aliases it as total_journeys_millions.
- `FROM TFL.JOURNEYS` specifies the table TFL.JOURNEYS from which the data is being retrieved.
- `WHERE journey_type IN ('Underground & DLR')` filters the data to include only rows where the journey_type is either 'Underground' or 'DLR'.
- `GROUP BY year, journey_type` groups the rows by year and journey_type, ensuring that the aggregation is performed for each unique combination of year and journey type.
- `ORDER BY total_journeys_millions ASC` orders the result set in ascending order based on total_journeys_millions.
- `LIMIT 5` limits the result to the top 5 rows, which will be the years with the lowest volume of Underground & DLR journeys.
