# Covid-19 Analysis 2023

Within the context of this study, I am experimenting with the fundamentals of SQL to apply my newly acquired skills by analyzing COVID-19 data.


## Data Analysis

### Covid Cases in Malaysia
1. How many are the total cases, deaths and death rate in Malaysia?
``` js
SELECT location, MAX(total_cases) as total_cases, MAX(total_deaths) AS total_deaths, MAX((total_deaths/total_cases)) * 100 AS percentage_death
FROM dbo.[covid-data]
WHERE location = 'Malaysia'
GROUP BY location
ORDER BY percentage_death
```

2. What is the highest total death in Malaysia?
``` js
SELECT location, population, MAX(total_deaths) AS total_deaths, 
       MAX(ROUND((total_deaths / population) * 100, 2)) AS death_rate_by_population
FROM dbo.[covid-data]
WHERE location = 'Malaysia'
GROUP BY location, population
```

3. Show the timeline of the infection rate of Covid-19 in Malaysia for the past 6 months in Malaysia.
 ``` js
  SELECT location, date, total_cases, ROUND(total_cases/population*100, 2) AS infection_rate
FROM dbo.[covid-data]
WHERE location = 'Malaysia' 
	  AND (date = '2023-01-01' 
		OR date = '2023-02-01'
		OR date = '2023-03-01'
		OR date = '2023-04-01'
		OR date = '2023-05-01'
		OR date = '2023-06-01'
		OR date = '2023-07-01'
		OR date = '2023-08-01'
		OR date = '2023-09-01'
		OR date = '2023-10-01')
```
There was an increase of 0.31% with 103237 total of new cases and 336 deaths in the span of 10 months in 2023.


### Covid Cases from All Countries
1. How many are the total cases, deaths and death rate from all countries?

``` js
SELECT location, MAX(total_cases) as total_cases, MAX(total_deaths) AS total_deaths, MAX((total_deaths/total_cases)) * 100 AS percentage_death
FROM dbo.[covid-data] WHERE total_cases IS NOT NULL AND 
total_deaths IS NOT NULL
GROUP BY location
ORDER BY location, percentage_death
```

2. How much are the infection rate per population from all countries

``` js
SELECT location, date, total_cases, population, 
	   (total_cases/ population) * 100 AS infection_rate
FROM dbo.[covid-data]
WHERE total_cases is not null
ORDER BY location, date
```

3. List countries with the highest infection rate.

``` js
WITH infection_rates AS (
  SELECT location, population, total_cases, (total_cases / population) * 100 AS infection_rate
  FROM dbo.[covid-data]
  WHERE total_cases is not null
)
SELECT location, MAX(infection_rate) AS max_infection_rate
FROM infection_rates
GROUP BY location
ORDER BY max_infection_rate DESC
```

OR

``` js
SELECT location, population, MAX(total_cases) AS total_cases, MAX((total_cases/population)) * 100 AS infection_rate
FROM dbo.[covid-data]
WHERE total_cases is not null
GROUP BY location, population
ORDER BY infection_rate DESC
```

4. List countries with the highest death rate by population.

``` js
SELECT location, population, MAX(total_deaths) AS total_deaths, MAX((total_deaths/population))*100 AS highest_death
FROM dbo.[covid-data] WHERE total_deaths IS NOT NULL 
GROUP BY location, population
ORDER BY highest_death DESC
```


   
   
