# Q1:
## Create Table:
 ```
 CREATE TABLE countries (
	code CHAR(2) NOT NULL,
	year INT NOT NULL,
	gdp_per_capita DECIMAL(10, 2) NOT NULL,
	govt_debt DECIMAL(10, 2) NOT NULL
);
```

## Top 3 average government debts in percent of the gdp_per_capita for those countries over the course of **4** years only.
```
SELECT code, SUM(govt_debt) / SUM(gdp_per_capita) * 100 as average_percent_of_debt_to_gdp_per_year
FROM countries
WHERE gdp_per_capita >= 40000 AND (strftime('%Y', DATE()) - YEAR <= 4)
GROUP BY code
ORDER BY average_percent_of_debt_to_gdp_per_year DESC LIMIT 3
```

## Top 3 average government debts in percent of the gdp_per_capita for those countries over the course of all **historical** years. Filter on "countries of which gdp_per_capita was over 40,000 dollars in every year in the last four years." is also used here but average is calculated based on all historical data instead of just 4 recent years.
```
SELECT countries.code, (govt_debt) / (gdp_per_capita) * 100 as average_percent_of_debt_to_gdp_per_year
FROM(
  SELECT code
  FROM countries
  WHERE gdp_per_capita >= 40000 AND (strftime('%Y', DATE()) - YEAR <= 4)
  GROUP BY Code
  HAVING COUNT(YEAR) >= 4) as Countries_Over_4_Years
INNER JOIN countries ON Countries_Over_4_Years.code = countries.code
GROUP BY countries.code
ORDER BY average_percent_of_debt_to_gdp_per_year DESC LIMIT 3
```
