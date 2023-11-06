# (SQL) Covid-19 Analysis 2023

Within the context of this study, I'm refining my understanding of the foundational principles of SQL by analyzing COVID-19 data across the world.
- The dataset contain information on deaths, cases, hospitalizations, mortality risk and others of Covid-19, covering the period from 8/01/2020 until 1/11/2023.
- The dataset used for this experiment was from <a href="https://ourworldindata.org/covid-deaths">Coronavirus Pandemic (COVID-19)</a><br>
- Date of data extraction: 1/11/2023.
- The results that produced were solely intended for the purpose of practicing and learning.
- Data visualization - in progress...

## Data Analysis

### Covid Cases in Malaysia
1. How many are the total cases, deaths and death rate in Malaysia?
``` javascript
SELECT location, MAX(total_cases) as total_cases, MAX(total_deaths) AS total_deaths, MAX((total_deaths/total_cases)) * 100 AS percentage_death
FROM dbo.[covid-data]
WHERE location = 'Malaysia'
GROUP BY location
ORDER BY percentage_death
```
#### Answer
![Q1 (MY)](https://github.com/shahrulbakar96/Covid-19-Analysis/assets/118825569/74b43eb9-b3c4-451e-9ac7-bf5fc82df3e8)



2. What is the weekly hospital admissions number in Malaysia?
``` javascript
SELECT location, date, weekly_hosp_admissions  
FROM dbo.[covid]
WHERE location = 'Malaysia' and date > '2023-01-01'
GROUP BY location, date, weekly_hosp_admissions
```
#### Answer
- Will be displayed in data visualization chart.
  

3. What is the highest total death in Malaysia?
``` js
SELECT location, population, MAX(total_deaths) AS total_deaths, 
       MAX(ROUND((total_deaths / population) * 100, 2)) AS death_rate_by_population
FROM dbo.[covid-data]
WHERE location = 'Malaysia'
GROUP BY location, population
```
#### Answer
![Q3 (MY)](https://github.com/shahrulbakar96/Covid-19-Analysis/assets/118825569/8ff21fc6-0044-48a8-a34c-34aca8a5058c)


4. Show the timeline of the infection rate of Covid-19 in Malaysia for the past 10 months in 2023.
 ``` js
  SELECT location, date, total_cases, ROUND(total_cases/population*100, 2) AS infection_rate
FROM dbo.[covid-data]
WHERE location = 'Malaysia' 
	  AND date > '2023-01-01'
```
#### Answer
- There was an increase of 0.31% with 103237 total of new cases and 336 deaths in the span of 10 months in 2023.



### Covid Cases from All Countries
1. How many are the total cases, deaths and death rate from all countries?

``` js
SELECT location, MAX(total_cases) as total_cases, MAX(total_deaths) AS total_deaths, MAX((total_deaths/total_cases)) * 100 AS percentage_death
FROM dbo.[covid-data] WHERE total_cases IS NOT NULL AND 
total_deaths IS NOT NULL
GROUP BY location
ORDER BY location, percentage_death
```
#### Answer
![Q1 (CT)](https://github.com/shahrulbakar96/Covid-19-Analysis/assets/118825569/90487d42-e85c-437c-9085-1dda65ba3bd9)


2. How much are the infection rate per population from all countries

``` js
SELECT location, date, total_cases, population, 
	   (total_cases/ population) * 100 AS infection_rate
FROM dbo.[covid-data]
WHERE total_cases is not null
ORDER BY location, date
```
#### Answer
- Will be displayed in data visualization chart.

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
#### Answer
![Q3 (CT)](https://github.com/shahrulbakar96/Covid-19-Analysis/assets/118825569/0bd100b2-d310-4250-961f-c947770c749c)

4. List countries with the highest death rate by population.

``` js
SELECT location, population, MAX(total_deaths) AS total_deaths, MAX((total_deaths/population))*100 AS highest_death
FROM dbo.[covid-data] WHERE total_deaths IS NOT NULL 
GROUP BY location, population
ORDER BY highest_death DESC
```
#### Answer
![Q4 (CT)](https://github.com/shahrulbakar96/Covid-19-Analysis/assets/118825569/95e56055-06dc-4ad3-a8e6-a8c4d5214cd4)


   
   
