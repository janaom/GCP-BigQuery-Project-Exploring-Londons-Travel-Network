# GCP-BigQuery-Project-Exploring-Londons-Travel-Network
Use BigQuery to build a project. Use SQL to analyze a database containing information about Transport for London journeys over 12 years

# Project Instructions

Write three SQL queries to answer the following questions:

1. What are the most popular transport types, measured by the total number of journeys? The output should contain two columns, 1) `journey_type` and 2) `total_journeys_millions`, and be sorted by the second column in descending order. Save the query as `most_popular_transport_types`.
2. Which five months and years were the most popular for the Emirates Airline? Return an output containing `month`, `year`, and `journeys_millions`, with the latter rounded to two decimal places and aliased as `rounded_journeys_millions`. Exclude null values and order the results by 1) `rounded_journeys_millions` in descending order and 2) `year` in ascending order, saving the result as `emirates_airline_popularity`.
3. Find the *five years* with the lowest volume of `Underground & DLR` journeys, saving as `least_popular_years_tube`. The results should contain the columns `year`, `journey_type`, and `total_journeys_millions`.

https://app.datacamp.com/workspace/w/0b014417-c584-4c69-aa51-e56cdfef8d84
